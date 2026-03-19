Để giải bài lab này thì ta phải truy xuất nội dung `/etc/passwd`

Sau khi chạy lab này ta được url này `https://0a81007a031baf32804212ac00ba00f5.web-security-academy.net/` 

<img width="1245" height="607" alt="image" src="https://github.com/user-attachments/assets/ee6f8f4a-89b7-47af-94f8-fc46051f87a0" />


Sau khi đọc kỹ yêu cầu bảo là có lỗ hổng truyền tải đường dẫn hình ảnh vậy thì thử xem mở hình ảnh ta được url là `https://0a24009e047863d583360bb000eb00c0.web-security-academy.net/image?filename=45.jpg` ở đây có parameter `filename` thì ta thử truyền `../../../etc/passwd` xem có được không

<img width="1248" height="510" alt="image" src="https://github.com/user-attachments/assets/c553eeb8-5267-4739-9a9d-5840f993e348" />

như hình là ta đã truy xuất được nội dung trong `etc/passwd` và thành công giải được bài lab này

<img width="1253" height="624" alt="image" src="https://github.com/user-attachments/assets/4da6ca41-8afe-4c0a-8a6d-e24478d4c25c" />
