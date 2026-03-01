Mục tiêu bài lab này là trả về chuỗi text chứa version 

Đầu tiên ta phải xác định số cột được truy vấn trả về (chú ý trong Oracle Database thì khi dùng câu lệnh `select` bắt buộc phải select thì 1 bảng vậy nên trong trường hợp này
ta có thể dùng bảng `dual`)

<img width="2559" height="1093" alt="image" src="https://github.com/user-attachments/assets/376c11bd-758f-496f-b53a-e92ac6e5c47b" />

ta thấy ở đây có 2 cột được trả về.

Tiếp đến thì ta sẽ xác định cột nào chứa dữ liệu kiểu text 

<img width="2543" height="1088" alt="image" src="https://github.com/user-attachments/assets/0135b0b1-3774-4efe-a05b-8c02bb78c375" />

sau khi check thì thấy cả 2 cột đều chứa dữ liệu kiểu text.

Cuối cùng ta sẽ chèn câu lệnh truy vấn để check version database thì theo Oracle câu lệnh truy vấn để hiển thị version database là `SELECT banner FROM v$version
/SELECT version FROM v$instance`

<img width="2501" height="1025" alt="image" src="https://github.com/user-attachments/assets/19700de8-93dc-4eec-97fc-0431c642df96" />

