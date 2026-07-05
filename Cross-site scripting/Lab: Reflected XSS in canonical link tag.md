<img width="1519" height="810" alt="image" src="https://github.com/user-attachments/assets/312add36-322d-4634-8d22-7572a98a435f" />

Yêu cầu: Thực hiện tấn công ở homepage để gọi hàm `alert()`

Gợi ý: Lab này reflect user input vào trong 1 thẻ `canonical link` và đã escape (mã hóa) các dấu ngoặc góc.

Để hỗ trợ cho việc khai thác, có thể giả định rằng người dùng mô phỏng sẽ nhấn các tổ hợp phím sau:

* `ALT+SHIFT+X`

* `CTRL+ALT+X`

* `Alt+X`

Lưu ý rằng cách giải theo đúng dự kiến của bài lab này chỉ có thể thực hiện được trên trình duyệt Chrome.

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/9dcd298d-6bb6-4e67-a066-1abfa6e15032" />

Ở đây tôi phát hiện ra rằng trang này có sử dụng 1 cái `canonical link` để báo cho "bot của công cụ tìm kiếm (Search Engine Bot / Crawler) biết đâu là trang gốc và sau khi thử bấm vào xem 1 bài post 
thì tôi thấy dev đã lấy cái URL hiện tại của user cho vào trong `canonical link` bao gồm cả tham số, điều này có nghĩa là ta có thể tiêm nhiễm dữ liệu vào bên trong thẻ `canonical link`.

<img width="1317" height="49" alt="image" src="https://github.com/user-attachments/assets/1bf0ed09-046c-41c5-a2f7-83323e4c79f4" />

Và mục tiêu mà lab này cũng đã nói là tôi có thể thực hiện tấn công ở ngay homepage, nhưng do hệ thống đã escape các dấu ngoặc góc, ta không thể tạo thẻ mới. Tuy nhiên, bằng cách dùng dấu nháy đơn 
(') để thoát khỏi thuộc tính `href`, ta có thể chèn thêm các thuộc tính độc hại mới vào thẻ hiện tại.

Thẻ `<link>` là một thẻ rỗng không được render lên giao diện, do đó các sự kiện tương tác thông thường đều vô hiệu. Để vượt qua rào cản này, tôi sử dụng thuộc tính `accesskey`. Khi thiết lập 
`accesskey`, trình duyệt sẽ mô phỏng một sự kiện click ngầm (synthetic click) vào phần tử đó nếu người dùng nhấn đúng tổ hợp phím tắt. Bằng cách này, tôi có thể kích hoạt sự kiện `onclick` chứa mã 
độc JS.

Payload:

```html
?'accesskey='x'onclick='print()
```
và nhớ phải encode lại trước khi cho lên thanh URL tránh 1 số thành phần bị encode không mong muốn

```html
?%27accesskey=%27x%27onclick=%27print()
```

Thì ở đây tôi sẽ truyền 1 tham số giả vào để cho trang trả về respone code `200 OK` trước sau đó thì cả cái url chứa payload sẽ được update vào `canonical link`.

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/1e5df0d2-1cfe-43f8-b958-bdc4557d6848" />

Sau khi truyền payload vào url và bấm tổ hợp phím của `FireFox` là `ALT+SHIFT+X` thì nó đã tự động kích hoạt `print()`

Tiếp theo là `alert`.

```html
?%27accesskey=%27x%27onclick=%27alert(1)
```

<img width="2506" height="1443" alt="image" src="https://github.com/user-attachments/assets/9aaaac3a-796c-43c7-ab4d-63d4eed2cba9" />

<img width="2497" height="1443" alt="image" src="https://github.com/user-attachments/assets/a422c541-bfbf-4a90-b82a-8eb38cba61e3" />
