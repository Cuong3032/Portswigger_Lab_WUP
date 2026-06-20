<img width="1532" height="520" alt="image" src="https://github.com/user-attachments/assets/a4ad6e11-77dc-4f2b-90fc-368f8872d0cb" />

Yêu cầu bài lab này là thực hiện blind OS command để khiến sever bị delay 10s, và được biết rằng là các input của user có chứa shell command. và kết quả của các lệnh không được hiển thị ra.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/969296af-3f35-4687-bd1b-c4dd4cf4261c" />

Thì đây là 1 trang bán hàng có chức năng home, submit feedback, với xem chi tiết sản phẩm.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/c832c47f-623e-47a5-926b-da606131e6e4" />

Với việc xem chi tiết sản phẩm thì ta có thể thay đổi ở phần `productID`.

Tiếp đến phần `home` thì nó chỉ trả về trang chứa các sản phẩm như trên.

Còn với phần `submit feedback` thì ta có thể nhập feedback của ta vào kèm theo 1 vài thông tin như `tên, email, subject`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/51a8a8db-a422-49fc-9bd3-3f5cf1dedfbe" />

Ở đây ta thử gửi 1 cái feedback xem như nào.

<img width="2044" height="568" alt="image" src="https://github.com/user-attachments/assets/0b784fa6-1a69-4ddc-bba7-13df4444d725" />

nó trả về 1 cái gói tin POST như trên.

Thì ở đây ta thử thay đổi phần `productID` xem nó có dính lỗi OS command injection không?

<img width="2045" height="821" alt="image" src="https://github.com/user-attachments/assets/2c855d3e-7e39-4b64-9216-2062e94976d5" />

Ở đây nó chỉ trả về mỗi Bad Request với lỗi như dưới

<img width="2030" height="698" alt="image" src="https://github.com/user-attachments/assets/6188d377-47c0-4e90-8ad4-db9ad553a26f" />

Tôi nghĩ chắc nó làm cách nào đó không hiển thị respone thôi nên tôi thử dùng sleep để khiến sever delay thì thấy nó trả về respone rất nhanh

<img width="2045" height="821" alt="image" src="https://github.com/user-attachments/assets/f1a1b685-05a3-4a40-bb8b-37d9e52192ab" />

Vậy nên tôi nghĩ ở đây khả năng không có dính lỗi OS command.

Quay lại với phần `submit feedback`.

