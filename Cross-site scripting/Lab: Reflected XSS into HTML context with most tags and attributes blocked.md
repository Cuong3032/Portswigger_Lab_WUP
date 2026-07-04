<img width="1494" height="912" alt="image" src="https://github.com/user-attachments/assets/500fe3ee-c0df-4906-88f1-5eeb6b0216f7" />

Yêu cầu: Gọi thành công hàm `print()` mà không cần bất kỳ tương tác nào từ người dùng (0-click).

Gợi ý: Lab chứa lỗ hổng Reflected XSS trong chức năng search nhưng bị bảo vệ bởi tường lửa WAF (cần bypass).

Lưu ý: 

- Payload phải tự động kích hoạt mà không cần người dùng tương tác (0-click).

- Tự thao tác thủ công để gọi hàm trên máy bạn sẽ không được tính là qua bài.

<img width="2512" height="799" alt="image" src="https://github.com/user-attachments/assets/132dda22-f71f-47b2-877e-c3b7308ab874" />

Ở đây tôi thử nhập vào 1 tag bình thường là tag `<h1>` thì respone trả về là "Tag is not allowed"

<img width="1684" height="312" alt="image" src="https://github.com/user-attachments/assets/d094221d-af05-4b0b-ae46-638cc9825b11" />

Có vẻ ở đây đã chặn các tag nguy hiểm, thì sau khi thử 1 vài tag thì tôi phát hiện ra tôi có thể dùng tag `body`

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/7ca942e6-6994-4920-9099-e2fabf50f344" />

TIếp tục tôi sẽ dùng các event handler để thử bật 1 cái thông báo lên xem

<img width="2507" height="1037" alt="image" src="https://github.com/user-attachments/assets/7a4089d9-1ecd-4d62-950a-318154090886" />

Thì nó báo lại với tôi là: "Attribute is not allowed"

<img width="2507" height="306" alt="image" src="https://github.com/user-attachments/assets/00410d32-3dd2-4d45-8b6e-1c8df422f4c3" />

Có thể thấy ở đây `WAF` cũng đã chặn cả các attribute nguy hiểm thường thấy.

Tôi sẽ thử brute force bằng cách thử từng loại event handler xem loại nào không bị chặn bằng tab `Intruder` của `Burpsuite`.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/77571c28-0dd4-4651-bd5d-7a424c7a05e8" />

Tôi sẽ `Add` vào vị trí `onload` và dùng cheatsheet của `PortSwigger` để làm payload lists.

<img width="1937" height="611" alt="image" src="https://github.com/user-attachments/assets/fae410b4-e003-4a80-875e-241ede9d0e22" />

Thì sau khi chạy xong tôi phát hiện ra có khá nhiều thuộc tính có thể dùng.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/228db1a9-7f55-4264-b1e6-b0e7770ff37c" />

Thì ở đây tôi chọn dùng là `oncontentvisibilityautostatechange` thì muốn nó tự động kích hoạt `alert`, thì tôi cần thêm 1 thuộc tính nữa của CSS là `content-visibility: auto` vì 
`oncontentvisibilityautostatechange` bóc tách ra là `on - content-visibility - auto - state - change`, có thể hiểu là kích hoạt khi trạng thái (state) của thuộc tính `content-visibility: auto` bị 
thay đổi (change). Nếu không có thuộc tính đó trong tag này, trình duyệt sẽ không theo dõi trạng thái của thẻ này. Vậy nên ở đây tôi sẽ kết hợp với thuộc tính style để áp dụng thuộc tính CSS 
kia.

Payload:

```html
<body style="content-visibility: auto" oncontentvisibilityautostatechange="alert()">
```

<img width="2500" height="1443" alt="image" src="https://github.com/user-attachments/assets/61c4006e-5ea7-4ac4-b1c3-f9a1b7ee6919" />

Đã thành công bật `alert()` mà không cần bất kỳ tác động nào. Tiếp theo là gửi cho `victim` thì tôi phải sửa lại `alert()` thành `print()`.

<img width="2498" height="1443" alt="image" src="https://github.com/user-attachments/assets/ba09fab2-d1d7-4b04-928f-fcf3600b5716" />

Mặc dù đã kích hoạt thành công hàm `print()` khi tự test, nhưng hệ thống Lab vẫn không đánh dấu hoàn thành khi gửi cho victim. Nguyên nhân rất có thể là do môi trường giả lập của Bot nạn nhân (thường 
là các `Headless Browser`) không hỗ trợ hoặc không xử lý các sự kiện phụ thuộc vào việc render CSS như `oncontentvisibilityautostatechange`.

Để loại trừ khả năng này, tôi chuyển sang sử dụng một sự kiện không phụ thuộc vào hiển thị hình ảnh là `onresize`. Bằng cách nhúng URL vào thẻ `<iframe>` trên `Exploit Server` 
và kết hợp thuộc tính `onload="this.style.width='100px'"`, tôi có thể ép `Iframe` tự động đổi kích thước, từ đó bóp cò sự kiện `onresize` để gọi hàm `print()`.

```html
<iframe src="https://0a1e001303aae2dd809e3fbc00f10096.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/f68b84e3-3783-407e-9ca2-39609ea94be2" />
