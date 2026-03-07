Mục tiêu cuối cùng của bài lab này là đăng nhập thành công vào ứng dụng dưới quyền tài khoản administrator

Đầu tiên ta phải xác định số cột được truy vấn trả về

<img width="1246" height="686" alt="image" src="https://github.com/user-attachments/assets/bea40c80-1b14-463c-b966-65079bab10cb" />

ta thấy ở đây có 2 cột được trả về.

Tiếp đến thì ta sẽ xác định cột nào chứa dữ liệu kiểu text

<img width="1237" height="689" alt="image" src="https://github.com/user-attachments/assets/849980dc-8685-4aed-a343-700763e7fdab" />

sau khi check thì thấy cả 2 cột đều chứa dữ liệu kiểu text.

Theo lab có gợi ý thì có 1 bảng là `users` và 2 cột là `username` và `password`, thì ta có thể thực hiện câu lệnh `select username,password from users` để lấy thông tin chi tiết
về username và password.

<img width="1244" height="659" alt="image" src="https://github.com/user-attachments/assets/5f7cbe20-baaf-4953-927b-a52396504960" />

<img width="343" height="98" alt="image" src="https://github.com/user-attachments/assets/9d37d2da-327a-490c-bc31-cc3ea66abd9f" />
<img width="343" height="84" alt="image" src="https://github.com/user-attachments/assets/40cbfe7e-5e20-4ff0-bc97-5b1c48103c11" />
<img width="426" height="101" alt="image" src="https://github.com/user-attachments/assets/2109b353-3772-4a25-a634-d1de80e3fe68" />

như hình ta thấy được khả năng cao là thông tin đăng nhập của adminstrator sẽ ở cặp username-password này
<img width="343" height="98" alt="image" src="https://github.com/user-attachments/assets/3626b62a-6c61-4dbc-9f58-c6154338bf7f" />

hãy thử đăng nhập xem có đúng 

<img width="1248" height="609" alt="image" src="https://github.com/user-attachments/assets/23961bbf-f5ee-4d39-aded-3e73b43be37e" />

vậy là ta đã lấy thành công thông tin đăng nhập của adminstrator.






