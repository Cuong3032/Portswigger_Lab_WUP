Mục tiêu bài này là đăng nhập thành công vào hệ thống dưới quyền tài khoản administrator nhưng là trên Oracle database.

Đầu tiên là kiểm tra số cột

<img width="1262" height="657" alt="image" src="https://github.com/user-attachments/assets/e06f60a3-746e-4de5-a510-f6cc00dc7d9f" />

như hình là có 2 cột.

Tiếp đến là kiểm tra xem 2 cột này có cột nào chứa text data không

<img width="1242" height="683" alt="image" src="https://github.com/user-attachments/assets/ca8313a4-0c25-4bd1-98a3-a54664c1bc96" />

như hình là cả 2 cột đều chứa text data.

Sau đó ta liệt kê ra list table bằng câu lệnh `Select table_name from all_tables`

<img width="1243" height="689" alt="image" src="https://github.com/user-attachments/assets/73750693-c8d8-4855-baa7-4b29d4a2dc22" />

ta thấy có bảng `USERS_TIYAJA` nghi vấn là có chứa dữ liệu về user, nên tiếp theo ta sẽ liệt kê các cột có trong bảng này ra bằng câu lệnh 
`SELECT column_name FROM all_tab_columns WHERE table_name = 'USERS_TIYAJA'`

<img width="1249" height="693" alt="image" src="https://github.com/user-attachments/assets/25f0e154-ac3b-4b81-afcd-dc28d24ee2d5" />

như hình ta thấy 2 cột khá là khả nghi có khả năng chứa username và password của user là `USERNAME_SBJQPZ` và `PASSWORD_ALKQAO`,
nên ta sẽ lấy hết thông tin chi tiết về username và password có trong bảng này bằng câu lệnh `select USERNAME_SBJQPZ,PASSWORD_ALKQAO from USERS_TIYAJA`

<img width="1248" height="686" alt="image" src="https://github.com/user-attachments/assets/7a780b62-b52f-4ad3-850c-ab6808f9f3f9" />

ta thấy có cặp username-password này khả năng cao là của adminstrator 

<img width="378" height="92" alt="image" src="https://github.com/user-attachments/assets/8b2b06f0-757f-4be6-a1be-de5d44f082a8" />

nên ta sẽ thử xem nó có đúng không

<img width="1237" height="515" alt="image" src="https://github.com/user-attachments/assets/eb6271bf-8a1b-4db8-87e3-aaed211947db" />

Và ta đã thành công lấy được thông tin đăng nhập của adminstrator.






