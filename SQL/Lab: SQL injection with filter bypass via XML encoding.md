<img width="1491" height="770" alt="image" src="https://github.com/user-attachments/assets/ea48c6f8-cdc6-4f0b-bd8f-42a449fbc16c" />

Yêu cầu: Tìm được thông tin đăng nhập của admin

Gợi ý: Có thể thực thi query SQL trên tính năng check giá. Kết quả từ query được trả về ở respone, có thể dùng `UNION` để đọc từ bảng khác, và có 1 bảng users chứa thông tin đăng nhập của user. 
Web application firewall (WAF) sẽ chặn các ký tự khả nghi của 1 cái SQLi. Có thể dùng `Hackvertor extension` để obfuscate query.

<img width="2541" height="1443" alt="image" src="https://github.com/user-attachments/assets/eef7fa81-3e6d-4536-a85c-f83f9dcac52e" />

Ở `home` ta có thể vào `my account` để đăng nhập tài khoản, hoặc `view detail` để xem chi tiết sản phẩm.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/c5e25dae-cf52-417b-af98-a5d851ed098b" />

Trong chi tiết sản phẩm ta có thể check giá sản phẩm này ở từng vùng.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/855e953c-ea84-494a-af67-255b9fd5a319" />

Theo gợi ý từ bài lab, tính năng 'Check stock' có khả năng dính lỗi SQL Injection nhưng các ký tự khai thác thông thường đã bị WAF chặn lại. Tuy nhiên, khi quan sát kỹ HTTP Request, ta nhận thấy dữ 
liệu đầu vào đang được truyền đi dưới định dạng XML. Từ đây, một ý tưởng nảy ra: Nếu chúng ta mã hóa câu lệnh query thành các thực thể XML (XML entities) thì liệu có bypass được WAF hay không?

Theo Hint từ bài lab, ta có thể dùng `Hackvertor extension` đề làm điều này.

<img width="2057" height="1186" alt="image" src="https://github.com/user-attachments/assets/3ecce5d2-0042-44df-8f7e-d3cfd0ef86e1" />

Đây là bằng chứng cho việc WAF đã chặn các ký tự khai thác bình thường đã bị chặn lại.

Do cấu trúc đang dược sử dụng là XML nên ta sẽ dùng `Hackvertor extension` để encode query thành 1 `XML entity` để tận dụng luôn trình XML parser của sever.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a6f4d609-a2b0-47d8-9a56-a68c538b18eb" />

Ta encode query bằng `hex_entities` để nó trở thành 1 `XML entity` xuống tới backend sẽ được decode về thành 1 query lúc đó nó sẽ được thực thi dẫn tới output như trên.

Ở đây ta phát hiện ra 1 account có username là `administrator` tôi nghĩ đây là username của `admin`, ta sẽ dùng account này để đăng nhập.

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/15edc5de-827f-4f9f-bf35-f3984003c4f4" />
