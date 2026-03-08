Mục tiêu bài lab này là lấy ra toàn bộ danh sách username và password từ bảng users và đăng nhập thành công administrator.

Đầu tiên ta phải xác định số cột được trả về từ truy vấn

<img width="1247" height="631" alt="image" src="https://github.com/user-attachments/assets/ba6a20e3-d8b3-44c8-8349-f86700ed792d" />

ta thấy được có 2 cột được trả về.

Tiếp đến ta xác định xem cột nào trong 2 cột có kiểu text data

<img width="1244" height="452" alt="image" src="https://github.com/user-attachments/assets/97aa6581-2292-4e44-91af-f4eec560cb13" />

ở cột thứ nhất thì không chứa text data tiếp tục thử với cột thứ 2

<img width="1249" height="647" alt="image" src="https://github.com/user-attachments/assets/ca68e578-dbca-41ce-b6fc-67fdf20b15a9" />

như vậy là chỉ có mỗi cột thứ 2 là chứa text data.

<img width="1246" height="431" alt="image" src="https://github.com/user-attachments/assets/4f333c19-9bec-4aa5-9209-7516f430b628" />

do chỉ có một cột là chứa text data nên khi ta cố ép nó nhận giá trị của cột username có thể dẫn đến lỗi Type Conversion Error do đó ta nên 
thay cột thứ nhất thành `null` và hiển thị username và password trên cùng cột thứ 2

<img width="1238" height="669" alt="image" src="https://github.com/user-attachments/assets/d2e868b1-9bc9-49a2-b915-18830c405acb" />

như vậy là ta đã hiện thỉ được hết username và password trên cùng 1 cột bằng String concatenation.

Tiếp đến chỉ cần đăng nhập bằng administrator là ta sẽ xong bài lab này

<img width="1245" height="537" alt="image" src="https://github.com/user-attachments/assets/f42367af-4741-4673-a87b-9772af12c5d2" />

