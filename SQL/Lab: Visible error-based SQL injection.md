<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/2ae5b46d-ccdd-4237-a465-f495f519552e" /><img width="1509" height="626" alt="image" src="https://github.com/user-attachments/assets/20440017-7f60-4790-a90a-eac0edd9d5a6" />

Yêu cầu: login bằng administrator account từ bảng users

Gợi ý: Bài lab này có lỗ hổng SQLi ở phần tracking cookie. Kết quả của lệnh SQL sẽ không được trả về.

<img width="2557" height="1443" alt="image" src="https://github.com/user-attachments/assets/91f61ddb-ccf8-4355-a408-a29ce01e44f2" />

Ta sẽ mở devtools lên và thử thay đổi Tracking Cookie bằng cách chèn thêm payload vào như nào. Bắt đầu bằng payload:

    ' AND '1' = '1

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a653049c-09d6-4256-8ccc-f58a5dc028be" />

Ta thấy khi ta chèn 1 payload chuẩn thì nó không có hiển thị thông báo hay có sự thay đổi gì. Tiếp đến nếu payload sai thì sao, nó có in ra lỗi hay gì đó không?

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/c8717fc6-936d-471a-a5e7-87161ad4834b" />

Có thể thấy ở đây nó đã in ra rõ răng lỗi của query đó như nào. Ta có thể dựa vào hiện thị lỗi này để khai thác không?

Trước tiên thử nối chuỗi với phép chia `1/0` xem nó hiển thị lỗi gì.

<img width="2501" height="1324" alt="image" src="https://github.com/user-attachments/assets/630f82a3-07ff-444f-aa6d-3856f18a570a" />

Ở đây có thể thấy nó hiển thị về lỗi `division by zero`. Vậy thì nếu đã có thể nhìn thấy chi tiết respone error rồi thì sao ta không lợi dụng nó để nó leak kểt quả mà ta muốn vào chính cái respone
error đó. Nhưng gặp 1 vấn đề là ta không rõ server đang sử dụng đến database nào.

Với Oracle thì bắt buộc phải có `FROM` tới bảng bất kỳ tôi sẽ thử không có `FROM` tới bảng nào xem:

    ' || (SELECT 1) || '

<img width="2084" height="1219" alt="image" src="https://github.com/user-attachments/assets/88c4ef45-7414-42a4-ba9d-d9c105a86cc2" />

Ở đây nó vẫn bình thường vậy không phải Oracle. Tiếp với Microsoft thì chỉ có Microsoft dùng hàm `LEN()` để tính độ dài trong khi các loại cơ sở dữ liệu còn lại đều dùng `LENGTH()`:

    ' || (LEN('a') || '

thì ở đây nó trả về lỗi và nhìn vào respone error có đủ `error`, `hint`, `position` thì đây là respone error của `PostgreSQL` do đó có thể xác định được đây là `PostgreSQL`.

Tiếp theo là query để khai thác dựa vào trình ép kiểu dữ liệu, nếu mà tôi ép 1 kiểu dữ liệu không phải interger về interger thì nó sẽ in ra lỗi `Type Casting Error` kèm theo data bị ép kiểu.

Payload:

    '||CAST((SELECT username FROM users LIMIT 1) AS int)||'

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a0798333-ba89-4d3e-9343-2660c6f104e7" />

Thì ở đây ta phát hiện ra rằng username dầu tiên trong bảng là `administrator` vậy tương ứng với nó thì password đầu tiên sẽ là password của `administrator` do đó query ta thay username thành 
password.

    '||CAST((SELECT password FROM users LIMIT 1) AS int)||'

<img width="2563" height="1372" alt="image" src="https://github.com/user-attachments/assets/66a15c18-7f1d-43cd-abc5-5b86770bb060" />

thế là ta được password là: `yukxcmwfijw65vx4mvmt`.

<img width="2563" height="1341" alt="image" src="https://github.com/user-attachments/assets/d3871a68-0d99-435c-a3b6-46339735a6ba" />
