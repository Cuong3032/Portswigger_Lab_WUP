<img width="1498" height="750" alt="image" src="https://github.com/user-attachments/assets/7ce447f6-a0db-4ebb-b493-e80ca6c28728" />

Yêu cầu: login bằng `administrator` account từ bảng users

Gợi ý: Bài lab này có lỗ hổng SQLi ở phần tracking cookie. Kết quả của lệnh SQL sẽ không được trả về và không có bất kỳ lỗi nào được trả về. Nhưng sẽ có đoạn message `Welcome Back` được trả về nếu như
query trả về bất kỳ rows nào.

Hint: Password có thể chứa số và các chữ cái alphabet.

<img width="1449" height="214" alt="image" src="https://github.com/user-attachments/assets/f62c3518-c420-45a1-8d3b-c8a865540ef4" />

Đây là giao diện của bài lab này, có home, my account, catagories với có thể xem chi tiết sản phẩm trong từng loại sản phẩm.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/dfeb591f-9131-485d-88fc-3bf597c8c6df" />

Với tính năng my account thì sau khi nhấp vào sẽ bắt ta phải đăng nhập và ta thấy nó xuất hiện dòng chữ `Welcome Back` thì dựa vào gợi ý ở đây có thể thấy đã query đã trả về ít nhất 1 rows.

Và được biết rằng SQL query có thể thực thi ở trên `Tracking Cookie` và sẽ chỉ hiển thị `Welcome Back` khi mà có ít nhất 1 row được trả về, vậy ta thử chèn thêm 1 payload vào `Tracking Cookie` xem nào. Ở đây ta sẽ dùng payload như này để cả 2 vế đều true và trả về ít nhất 1 row:

    [cookie]' and '1' = '1

Vì tôi dự đoán query mà dev sẽ sử dụng sẽ là:

    select ... from ... where tracking_cookie = '????'

<img width="891" height="1129" alt="image" src="https://github.com/user-attachments/assets/2d0fb5fb-aecf-4985-8bd4-b2a27f96e186" />

Có thể thấy dòng message `Welcome Back` vẫn được trả về bình thường nhưng `Tracking Cookie` đã bị chèn thêm 1 đoạn payload do đó có thể thấy query đã thực thi thành công. Và để kiểm chứng xem nếu ta
chèn 1 payload sai nó sẽ có trả về gì không?

<img width="954" height="1181" alt="image" src="https://github.com/user-attachments/assets/ba778061-97cc-46a3-80f0-25e41a59499b" />

Có thể thấy ở đây sau khi sửa sai payload thì dòng `Welcome Back` đã biến mất. Vậy thì ở đây ta có thể dựa vào respone này mà exploit được không?

Được biết có 1 kỹ thuật là Blind SQL Injection khi mà dựa vào respone để phán đoán, từ đó có thể khai thác được database.

Thì trước khi bắt đầu tìm password ta vẫn cần xác nhận trước là có bảng `users` và có user `administrator` trong bảng user hay không?

Ta dùng payload sau để xác định là có bảng users hay không:

    [cookie]' AND (SELECT 'a' FROM users LIMIT 1)='a

Ở đây ta dùng kết quả trả về ảo là 'a' để xác định xem có bảng users nào không và đề phòng bảng users có nhiều user thì ta sẽ giới hạn nó trả về là 1 dòng để lúc kết quả so sánh tránh vì nhiều chữ `a`
quá mà dẫn tới lỗi hệ thống. Và khi mà có thì vế sau `AND` sẽ trả về `true` và message `Welcome Back` sẽ xuất hiện nếu không có thì sẽ không xuất hiện.

<img width="2113" height="1227" alt="image" src="https://github.com/user-attachments/assets/13e93301-8bc6-439b-8459-5a6845bf2c43" />

Thì ở đây là có 1 bảng tên là `users`. Tiếp đến dùng payload như này để xác định có `administrator` user trong bảng `users` hay không:

    [cookie]' AND (SELECT 'a' FROM users WHERE username='administrator')='a

<img width="2064" height="1316" alt="image" src="https://github.com/user-attachments/assets/b95c99d5-46b4-4077-8815-4f561c158c00" />

Kết quả là có user như vậy. Tiếp đến là ta phải xác định được độ dài của `password` thì để làm được điều này ta dùng hàm `LENGTH` để lấy độ dài password và payload sẽ như sau:

    [cookie]' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password) > 1)='a

Ta sẽ so sánh cho tới khi respone trả về không còn message `Welcome Back` nữa. Thì lúc này độ dài password sẽ bằng độ dài so sánh

<img width="2073" height="1204" alt="image" src="https://github.com/user-attachments/assets/6ad9bd12-5a8c-480d-bfc9-1941e853ab12" />

Thì sau khi làm lại vài lần thì ta thấy khi so sánh với 20 thì không còn hiện message nữa. Nghĩa là độ dài của password là 20.

<img width="2119" height="1208" alt="image" src="https://github.com/user-attachments/assets/c589f6e1-a36b-4abc-9bb3-38dcca05900b" />

Tiếp đến là phần mò pasword thì ta sẽ dùng tab Intruder của Burp Suite làm cái này với payload như sau:

    [cookie]' AND (SELECT substring(password,1,1) FROM users WHERE username='administrator')='parameter

Thì với cách này ta sẽ xác định được từng ký tự trong password. Như payload trên là ký tự thứ nhất của password.

Ở trong payload tab Intruder ta chọn list a-z, A-z, 0-9 và grep-match với `Welcome Back` để dễ dàng nhận ra đâu là ký tự đúng.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/d8b980d1-f819-4457-a3b5-013347fb4d07" />

Ở đấy ký tự đầu là `a`

Tiếp với ký tự 2:

    [cookie]' AND (SELECT substring(password,2,1) FROM users WHERE username='administrator')='parameter

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/3516f287-611e-4392-be38-823caa9bfe76" />

Ký tự 2 là `t`.

Ta tiếp tục thử cho tới ký tự 20 và được 1 đoạn hoàn chỉnh là: `atvpp8ve0gc4cucmfrll`

<img width="2548" height="970" alt="image" src="https://github.com/user-attachments/assets/ddf40033-cb98-465c-b8fd-b894ba1cc94e" />

Và thử đăng nhập account `administrator` với password trên

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/b037bcbc-6107-4061-86e1-9e33b0826186" />
