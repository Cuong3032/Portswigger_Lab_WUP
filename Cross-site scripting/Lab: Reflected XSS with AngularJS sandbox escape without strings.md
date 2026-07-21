<img width="1562" height="663" alt="image" src="https://github.com/user-attachments/assets/8aebef83-7de8-4a2b-aae7-559729c9e74a" />

Yêu cầu: escape sandbox để thực thi hàm `alert` mà không cần hàm `eval`.

Gợi ý: Lab này sử dụng AngularJS theo một cách khá đặc biệt, trong đó hàm `$eval` không khả dụng và bạn sẽ không thể sử dụng bất kỳ chuỗi (strings) nào trong AngularJS.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/a21d0c02-ee56-4444-8ec6-6f8393aaafc1" />

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/de0058f4-ac4b-477f-b0b9-9db01235c736" />

Khi kiểm tra mã nguồn, ta có thể thấy ứng dụng sử dụng AngularJS để lấy giá trị tham số từ URL và gán vào biến key. Lỗ hổng Reflected XSS xuất hiện tại dòng logic: `$scope.value = $parse(key)($scope.query);`.

```html
                        <script>angular.module('labApp', []).controller('vulnCtrl',function($scope, $parse) {
                            $scope.query = {};
                            var key = 'search';
                            $scope.query[key] = 'abc';
                            $scope.value = $parse(key)($scope.query);
                        });</script>
                        <h1 ng-controller=vulnCtrl>0 search results for {{value}}</h1>
```

Thay vì in ra dữ liệu thuần túy, ứng dụng lại đẩy trực tiếp User Input (biến `key`) vào hàm `$parse`. Hàm này sẽ biên dịch chuỗi đầu vào thành một function. Ngay sau đó, đoạn `($scope.query)` sẽ kích hoạt thực thi function vừa được biên dịch này. Lợi dụng việc đầu vào không được kiểm tra (sanitize), kẻ tấn công có thể truyền vào một payload đặc biệt trên URL để ép `$parse` thực thi mã JavaScript độc hại, từ đó vượt qua Sandbox của Angular.

Vậy ta sẽ thử chèn thêm 1 biến bất kỳ do ta kiểm soát xem nó có thể được hiển thị ra không?

<img width="2499" height="1443" alt="image" src="https://github.com/user-attachments/assets/80687151-23c8-4472-9d97-f241571632b3" />

Nhìn trên ta đã thêm 1 thuộc tính bất kỳ `abc` với giá trị là `fds` và nó đã có thể in ra ngoài

```html
                        <script>angular.module('labApp', []).controller('vulnCtrl',function($scope, $parse) {
                            $scope.query = {};
                            var key = 'search';
                            $scope.query[key] = 'abc';
                            $scope.value = $parse(key)($scope.query);
                            var key = 'abc';
                            $scope.query[key] = 'fds';
                            $scope.value = $parse(key)($scope.query);
                        });</script>
                        <h1 ng-controller=vulnCtrl>0 search results for {{value}}</h1>
```

<img width="2511" height="1443" alt="image" src="https://github.com/user-attachments/assets/49806a7a-6a9c-461a-b7d6-56237925f93c" />

Tiếp theo tôi sẽ thử thay vào bằng 1 phép toán xem nó có thể biên dịch và chạy phép toán đó, thì ở đây ta sẽ chèn phép toán vào `key` để nó được biên dịch, nếu truyền vào `value` thì nó sẽ không được biên dịch mà chỉ là 1 chuỗi bình thường do `$parse` chỉ biên dịch biến `key`.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/9a23fd20-791a-4e61-8976-8852e1822f7c" />

Lúc này thì `$parse` đã đọc và không coi `key` là 1 một tên định danh (identifier) hợp lệ dẫn tới nó sẽ không đi tìm dữ liệu trong object `$scope`. Dẫn tới là sau khi biên dịch phép toán và thực thi nó thông qua `($scope.query)` thì `$scope.value` được gán giá trị bằng 2.

Vậy có nghĩa là ta sẽ có thể kiểm soát và thực thi 1 hàm của Javascript thông qua thuộc tính bất kỳ.

Kết hợp với gợi ý đề bài là không cần hàm eval và không thể sử dụng chuỗi bất kỳ nào trong AngularJs. Đầu tiên biết được là các hàm global đã bị chặn và ta chỉ có thể sử dụng các hàm có sẵn bên trong `$scope`, do đó tôi không thể sử dụng các hàm global như `eval`. Vậy thì tôi sẽ sử dụng 1 hàm có thể gọi tới trong cái sandbox này và dùng thuộc tính `constructor` thì tôi có thể truy xuất ngược về hàm khởi tạo gốc Function của JavaScript.

```js
toString().constructor
```

