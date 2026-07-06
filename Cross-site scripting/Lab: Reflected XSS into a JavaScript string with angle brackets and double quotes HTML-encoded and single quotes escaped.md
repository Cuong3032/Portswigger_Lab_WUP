<img width="1488" height="592" alt="image" src="https://github.com/user-attachments/assets/0872d5f0-bc07-4146-aa4f-6738068fd621" />

Yêu cầu: Thực hiện `cross-site scripting attack` thoát khỏi Javascript string và gọi hàm `alert()`.

Gợi ý: Lab chứa lỗi `reflected cross-site scripting` ở tính năng theo dõi `query search`. Reflect xảy ra bên trong `Javascript string` và nháy đơn, nháy kép và ngoặc nhọn đã bị `escape`.

Tôi thử nhập vào `"+'+\+>+<` và nhìn phần source code ta thấy được các ký tự trên trừ `\` đã bị mã hóa hoặc escape.

<img width="2511" height="1443" alt="image" src="https://github.com/user-attachments/assets/2ffd0ee1-1931-4bc3-ba1c-7e480e39028c" />

Nhận thấy server đã escape dấu nháy đơn `'` nhưng lại bỏ sót dấu gạch chéo ngược `\`, ta có thể lợi dụng điều này để vô hiệu hóa chính dấu escape của hệ thống. Cụ thể, bằng cách truyền vào `\'`, ta 
tạo ra chuỗi `\\'`, qua đó đóng chuỗi hợp lệ và thoát khỏi ngữ cảnh chuỗi (string context) của biến `searchTerms`.

Tiếp theo, ta sử dụng toán tử trừ `(-)` để ép JavaScript Engine đánh giá biểu thức toán học, từ đó buộc nó phải thực thi hàm `print()`. Cuối cùng, để payload hoạt động trơn tru, ta thêm dấu comment 
`//` nhằm loại bỏ phần cú pháp thừa phía sau, tránh gây lỗi Syntax Error khiến khối lệnh bị hủy bỏ.

```javascript
\' - print() //
```

Ở đây tôi sẽ comment lại mọi thứ phía sau hàm `print()` để tránh gây lỗi.

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/2578b4ed-7474-4dee-bcee-da95bb9ee8ee" />

Tiếp theo là thực hiện mục tiêu bài lab là kích hoạt `alert()`.

<img width="2496" height="1443" alt="image" src="https://github.com/user-attachments/assets/7b4def9e-5f1f-4aab-aafd-0bedc94f0dd3" />

<img width="2496" height="1443" alt="image" src="https://github.com/user-attachments/assets/8cc801e7-c208-4c30-88fe-7d7f0fb56f96" />
