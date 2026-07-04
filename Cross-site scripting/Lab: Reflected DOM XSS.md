<img width="1519" height="478" alt="image" src="https://github.com/user-attachments/assets/75e0d2ca-304d-43d8-9f2f-b6406137b7db" />

Yêu cầu: tạo một payload tiêm nhiễm (injection) để gọi hàm alert()

Gợi ý: Bài lab này minh họa một lỗ hổng reflected DOM. Các lỗ hổng reflected DOM xảy ra khi ứng dụng phía máy chủ (server-side) xử lý dữ liệu từ một yêu cầu (request) và phản hồi lại (echoes) dữ liệu 
đó trong phần hồi đáp (response). Sau đó, một tập lệnh (script) trên trang web sẽ xử lý dữ liệu được phản xạ này một cách không an toàn, và cuối cùng ghi nó vào một điểm tiếp nhận nguy hiểm (dangerous 
sink).

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/096375e8-e9a7-4ce5-8de2-45cc021aaa0b" />

<img width="2507" height="1443" alt="image" src="https://github.com/user-attachments/assets/15b11fa6-9e80-4b92-adaa-bdd111e5ca95" />

Với source code và kết quả được hiển thị ra màn hình như kia thì tôi biết được rằng Thay vì render (kết xuất) trực tiếp kết quả tìm kiếm vào thẳng mã nguồn HTML ban đầu, lập trình viên đã thiết kế 
trang web tải dữ liệu động thông qua cơ chế AJAX bằng cách gọi hàm `search()`.

<img width="2507" height="1441" alt="image" src="https://github.com/user-attachments/assets/7777d623-b71e-4186-ba4d-d706f10ded04" />

Mở source code file `searchResults` lên ta thấy hàm search đang nhận tham số path (đóng vai trò là API endpoint) và nó có khởi tạo đối tượng `XMLHttpRequest` và sau đó nó sẽ cấu hình request bằng 
phương thức `open()` tới endpoint kết hợp với chuỗi truy vấn Query String (window.location.search) là giá trị ta nhập vào rồi gửi đi, sau đó nó kiểm tra trạng thái hoàn tất của request 
(readyState == 4) và mã trạng thái HTTP thành công (status == 200) nếu đúng thì nó gọi hàm eval tạo Object `searchResultsObj` được gán với HTTP Response Body rồi hiển thị bằng hàm 
`displaySearchResults`.

Ở mã nguồn HTML ban đầu ta phát hiện ra hàm search đang trỏ tới endpoint `search-results` và sau khi thêm endpoint đó vào trước biến search thì tôi phát hiện ra rằng nó trả về là 1 đoạn `json data`
với 1 mảng result và phần ta nhập vào thanh tìm kiếm.

<img width="2515" height="1443" alt="image" src="https://github.com/user-attachments/assets/4c9f659e-4455-4714-a6be-ed16b57aab2f" />

Và nó sẽ khởi tạo Object `searchResultsObj` dựa trên kết quả trả về này, vậy tôi sẽ thử tìm cách để có thể Sử dụng kỹ thuật `Context Breakout` (Thoát khỏi ngữ cảnh) để phá vỡ cấu trúc chuỗi JSON, sau 
đó inject (tiêm nhiễm) mã JavaScript độc hại.

Ta có cấu trúc của `Json data` được trả về sẽ như sau:

```json
{"results":[kết_quả_trùng_khớp_với_searchTerm],"searchTerm":"[payload_của_ta]"}
```

Thì với cấu trúc trên tôi có thể truyền vào payload như sau:

```
"}; alert(origin) //
```

thì đi vào hàm `eval()` của hàmn `search `sẽ thành ra:

```javascript
eval('var searchResultsObj = {"results": [], "searchTerm": "\\"}; alert(origin) //"}')
```

Payload sử dụng dấu `;` để kết thúc sớm câu lệnh khai báo biến, tạo không gian để thực thi hàm `alert(origin)`. Cặp gạch chéo `//` đóng vai trò vô hiệu hóa (comment out) phần cú pháp JSON thừa do 
server sinh ra, giúp tránh lỗi `Syntax Error` trong quá trình `eval()` biên dịch.

<img width="2501" height="837" alt="image" src="https://github.com/user-attachments/assets/4a185fcd-ad57-4fbc-96fd-9f581d1760d4" />

Sau khi thử bằng payload trên thì thấy nó không bật lên alert, tôi sẽ thử lại với endpoint trước.

<img width="2488" height="296" alt="image" src="https://github.com/user-attachments/assets/365df637-9871-450e-bbe7-39b2045a0370" />

Server có cơ chế filter (lọc) bằng cách thêm ký tự thoát (backslash) trước dấu ngoặc kép. Để bypass cơ chế này, ta áp dụng kỹ thuật `Escaping the escape character` (Thoát khỏi ký tự thoát) bằng cách 
truyền vào `\"`. Trình duyệt sẽ hiểu `\\` là một dấu gạch chéo ngược hợp lệ, giải phóng dấu `"` phía sau để đóng chuỗi

```
\"}; alert(origin) //
```

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/c7edfeeb-ccbe-472d-bc16-499a62f10c40" />

Thế là ta đã thành công bật được alert lên

<img width="2487" height="1443" alt="image" src="https://github.com/user-attachments/assets/54ac3621-1b60-40c2-ba06-398deb652a7f" />
