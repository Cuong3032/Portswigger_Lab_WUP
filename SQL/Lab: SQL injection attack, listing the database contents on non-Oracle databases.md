Mục tiêu của bài lab này là đăng nhập thành công vào hệ thống với quyền administrator

Đầu tiên kiểm tra số cột được trả về

<img width="1250" height="676" alt="image" src="https://github.com/user-attachments/assets/2ba9a45a-242e-4ae0-a16f-94a3e6bcac7e" />

như hình ta thấy được có 2 cột.

Tiếp đến kiểm tra cột nào chứa text data

<img width="1234" height="682" alt="image" src="https://github.com/user-attachments/assets/e1cb494f-8121-495a-9be4-613688429fd2" />

ta thấy được cả 2 cột đều trả về text data.

Sau đó ta liệt kê ra list table bằng câu lệnh `Select table_name from information_schema.tables`

<img width="1260" height="688" alt="image" src="https://github.com/user-attachments/assets/6d468361-d8a5-4ee6-9c01-9e161a252c96" />

ta thấy bảng `administrable_role_authorizations` khá đáng ngờ nghi vấn là chứ username và password của administrator.

Tiếp đến ta dùng câu lệnh `Select column_name from information_schema.tables where table_name = 'administrable_role_authorizations'` để lấy thông tin chi tiết có trong cột trong bảng
này

<img width="1243" height="659" alt="image" src="https://github.com/user-attachments/assets/95861897-e785-4d9d-b035-de344d6f2ee9" />

ta thấy 3 cột là is_grantable, grantee và role_name trông nó không có gì là liên quan tới username và password nên khả năng bảng này không đúng.

Sau khi kiểm tra lại list table tôi thấy có table khá là khả nghi là  `users_vtwegv` vì đây có thể là bảng lưu trữ thông tin của user

<img width="1400" height="48" alt="image" src="https://github.com/user-attachments/assets/3daeb5c0-8cb4-4bb9-8b00-30b91af4e330" />

Ta tiếp tục dùng câu lệnh `Select column_name from information_schema.tables where table_name = 'users_vtwegv'` để lấy thông tin chi tiết có trong cột trong bảng này

<img width="1238" height="684" alt="image" src="https://github.com/user-attachments/assets/f7b8b158-10f7-415c-915b-197f34266e83" />

đúng như dự đoán ở đây có 2 cột khả năng chứa username và password là `username_txyods` và `password_ybqesh`

Tiếp tục ta sẽ dùng câu lệnh `select username_txyods,password_ybqesh from users_vtwegv` để lấy thông tin chi tiết về username và password có trong bảng này.

<img width="1811" height="102" alt="image" src="https://github.com/user-attachments/assets/7042fb6c-2081-4bdf-a0c9-3148606197e6" />

<img width="1783" height="87" alt="image" src="https://github.com/user-attachments/assets/fc888016-cb14-4477-85fc-34026d52b147" />

<img width="1799" height="87" alt="image" src="https://github.com/user-attachments/assets/608f2836-a65c-4655-a922-5bb09596eb23" />

ta thấy nó trả về 3 cặp username-password khác nhau và khả năng cao là cặp administrator, qlc59dhastfamktgi53q là username-password của adminstrator.

Ta sẽ thử đăng nhập bằng cặp này xem có đúng không

<img width="1246" height="634" alt="image" src="https://github.com/user-attachments/assets/ec70e339-0574-4fe7-8f08-6da762a9437a" />

Và đúng thật cặp administrator, qlc59dhastfamktgi53q là cặp username-password của adminstrator.













