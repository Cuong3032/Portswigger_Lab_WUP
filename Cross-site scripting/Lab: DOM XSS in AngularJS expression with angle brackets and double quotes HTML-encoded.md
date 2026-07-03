<img width="1495" height="854" alt="image" src="https://github.com/user-attachments/assets/3f7fefb5-c300-4da8-b572-2d43d2eb5cfc" />

Yêu cầu: gọi 1 hàm alert().

Gợi ý: Lab này chứa một lỗ hổng `cross-site scripting (XSS)` dựa trên DOM trong một biểu thức `AngularJS` nằm trong chức năng tìm kiếm. `AngularJS` là một thư viện JavaScript phổ biến, nó quét nội dung 
của các node HTML có chứa thuộc tính `ng-app` (còn được gọi là một directive của `AngularJS`). Khi một directive được thêm vào mã HTML, bạn có thể thực thi các biểu thức JavaScript bên trong cặp dấu 
ngoặc nhọn kép. Kỹ thuật này rất hữu ích khi các dấu ngoặc nhọn (`angle brackets` < >) đang bị mã hóa.

<img width="2521" height="1443" alt="image" src="https://github.com/user-attachments/assets/74c99763-461b-4a37-b0de-438342a4964c" />

<img width="2465" height="557" alt="image" src="https://github.com/user-attachments/assets/be579a0b-71af-473e-b3d5-6536369c7657" />

Ở đây ta phát hiện ra rằng các ký tự `angle brackets` đã bị mã hóa, nhưng ta phát hiện ra rằng trang này đang sử dụng AngularJS, thì với thư viện này thì bất kỳ trong tag nào sử dụng thuộc tính 
`ng-app` sẽ có thể thực thi được các mã của Angular ở bên trong. Và với AngularJS ta có thể dùng `{{...}}` để thực thi 1 đoạn Javascript nhỏ hoặc phép tính toán.

<img width="2506" height="1443" alt="image" src="https://github.com/user-attachments/assets/961b7ed6-60b9-47c5-8f66-9bc4ca0a328f" />

Ở đây tôi thử nhập payload này vào

```javascript
{{1+1}}
```

<img width="2500" height="1443" alt="image" src="https://github.com/user-attachments/assets/e547eaa6-bfc4-44af-9e14-f9cb238e60ee" />

Nó đã tự động tính toán và trả về kết quả là 2 đã được hiển thị ra màn hình. Tiếp theo tôi sẽ thử truyền 1 hàm `alert()` vào 

```javascript
{{alert(origin)}}
```

Thì tôi phát hiện ra rằng mặc dù nó được xử lí nhưng nó không bật alert lên thì sau khi tìm hiểu tôi được biết rằng cặp `{{}}` không thể gọi tới các hàm global do chúng nằm ngoài phạm vi đối tượng 
`$scope` mà AngularJS quản lý

Vậy thì ở đây tôi sẽ sử dụng 1 hàm có thể gọi tới trong cái sandbox này và dùng thuộc tính  `constructor` thì tôi có thể truy xuất ngược về hàm khởi tạo gốc Function của JavaScript. 
Khi đã nắm quyền kiểm soát Function, tôi có thể truyền vào một chuỗi mã độc và ép trình duyệt biên dịch nó thành mã thực thi, từ đó gọi được bất kỳ hàm global nào (như alert) ra ngoài môi trường 
thực.

Do đó tôi sẽ sửa payload thành:

```javascript
{{$on.constructor('alert(origin)')()}}
```

<img width="2494" height="1443" alt="image" src="https://github.com/user-attachments/assets/e22de0d3-a2f3-4a33-b798-4d065c9ad30e" />

<img width="2512" height="1443" alt="image" src="https://github.com/user-attachments/assets/92e247d8-5f50-46dd-8cc7-55ca4af78f18" />
