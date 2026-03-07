Tương tự bài lab yêu cầu hiển thị version Oracle thì lần này là Microsoft và MySQL

Đầu tiên ta check xem số lượng cột được trả về

<img width="1280" height="685" alt="image" src="https://github.com/user-attachments/assets/35afe775-4f66-4e24-9f95-6b477e6c444a" />

như theo hình thì ta xác định đc 2 cột (chú ý đây là comment của MySQL nên là -- phải có space(%20)).

Tiếp theo ta sẽ check xem data trả về ở mỗi cột có phải text hay không

<img width="1277" height="650" alt="image" src="https://github.com/user-attachments/assets/512a0fc4-2762-447c-bffa-d1925911553a" />

như hình là cả 2 cột đều trả về kiểu dữ liệu là text.

Cuối cùng là ta dùng câu lệnh là `select @@version` để hiển thị version của MySQL và Microsoft

<img width="1246" height="660" alt="image" src="https://github.com/user-attachments/assets/40ae53ba-d66b-4ca8-aded-26095103dbdc" />

