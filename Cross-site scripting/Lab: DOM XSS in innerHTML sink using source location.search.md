<img width="1552" height="667" alt="image" src="https://github.com/user-attachments/assets/ad38f0cc-5c4d-4ce4-b470-e52a1035bd3b" />

Yêu cầu: gọi 1 hàm alert().

Gợi ý: Lab này chứa lỗ hổng `DOM-based cross-site scripting` trong thanh search. Nó sử dụng innerHTML assignment, nơi mà có thể thay đổi nội dung của bất kỳ thẻ nào và nó sử dụng nội dung lấy từ 
`location.search` - Nội dung ta có thể kiểm soát được.

<img width="2518" height="1443" alt="image" src="https://github.com/user-attachments/assets/5e549562-3b7e-42eb-bcb5-f200acab5019" />

<img width="2563" height="1439" alt="image" src="https://github.com/user-attachments/assets/ec67ab48-37bf-42bf-ba82-f199c6cf242d" />

Sau khi search 1 ký tự thì nó sẽ được in ra trong 1 thẻ `span` nhưng ở trong mã nguồn gốc (View Source) không thấy nội dung ta vừa nhập là vì ở đây backend không xử lí cái nội dung ta nhập mà giao cho 
Client-side xử lí bằng đoạn script javascript ở dưới:

```html
                        <script>
                            function doSearchQuery(query) {
                                document.getElementById('searchMessage').innerHTML = query;
                            }
                            var query = (new URLSearchParams(window.location.search)).get('search');
                            if(query) {
                                doSearchQuery(query);
                            }
                        </script>
```

Thì với đoạn script trên thì nó sẽ lấy nội dung mà ta nhập rồi ghi đè trực tiếp vào thẻ `span` có id `searchMessage` mà không cần backend xử lí.

Vậy với cách xử lí thông qua Client-side kia thì tôi thấy có thể chèn vào 1 tag html vì không có filter gì ở đây, tôi sẽ thử với tag `h2`

<img width="2494" height="1443" alt="image" src="https://github.com/user-attachments/assets/5ef633d3-b3f0-4310-9df4-c12fb3199f07" />

Có thể thấy ở đây tag `h2` đã khiến cho kích thước nội dung bên trong trở nên khác đi.

Vậy tiếp theo tôi sẽ thử chèn 1 script để nó bật alert lên xem sao.

<img width="2495" height="1443" alt="image" src="https://github.com/user-attachments/assets/4e6d1b47-fc50-4ebf-9718-b6d2f57fdaf6" />

Ở đây thì nó đã không bật ra alert nào nhưng nội dung cũng đã không hiển thị gì hết có vẻ đã bị xóa đi nhưng bên trong devtool thì tôi vẫn thấy payload.

<img width="816" height="224" alt="image" src="https://github.com/user-attachments/assets/4e68e670-f368-44ac-aacc-d5360f8446c2" />

Thì được biết là với `innerHTML` thì từ HTML5, nó sẽ chặn đứng và không cho phép chạy thẻ `<script>`. Vậy phải làm sao để chạy được thẻ `<script>`?

Thì ta sẽ dùng `Event Handler`, thì với `Event Handler` tôi có thể thực thi Javascript ở 1 thuộc tính thuộc thẻ khác. Ở đây tôi sẽ dùng thẻ `<img>` kèm thuộc tính `onerror` để nó tự động bật alert
khi có lỗi xảy ra.

```html
<img src="x" onerror="alert(origin)">
```

Tôi thêm 1 cái src giả vào để khi nó đi tới địa chỉ đó sẽ báo lỗi và tự động kích hoạt thuộc tính `onerror`.

<img width="2507" height="1443" alt="image" src="https://github.com/user-attachments/assets/b7cc1eb4-f356-4c11-9366-7316350c5a66" />
