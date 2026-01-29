Để giải bài lab, xác định số cột trả về của truy vấn bằng cách thực hiện một cuộc tấn công SQL injection UNION để trả về một hàng bổ sung chứa các giá trị null.

Vậy thì đầu tiên ta thử chèn 1 câu lệnh `union select null --` vào thì nó sẽ trả về lỗi do ch khớp số cột sau đó ta sẽ tiếp tục chèn thêm cột null cho đến khi không còn trả về lỗi nữa
thì số null được chèn vào sẽ là số cột

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/488fa618-da1b-4e3b-920e-45342e059164" />

như ở đây là có 3 cột
