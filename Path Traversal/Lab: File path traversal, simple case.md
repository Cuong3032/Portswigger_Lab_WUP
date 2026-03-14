Để giải bài lab này thì ta phải truy xuất nội dung `/etc/passwd`

Sau khi chạy lab này ta được url này `https://0a81007a031baf32804212ac00ba00f5.web-security-academy.net/` thử chèn đoạn path này xem có lấy được nội dung trong `/etc/passwd`

Nó trả về `Not Found` là nghĩa là không có thư mục con như kia

Sau khi đọc lại kỹ đề bảo là có lỗ hổng truyền tải đường dẫn hình ảnh vậy thì thử xem mở hình ảnh ta được url là `https://0a81007a031baf32804212ac00ba00f5.web-security-academy.net/image?filename=67.jpg` ở đây có parameter `filename` thì ta thử truyền `../../../etc/passwd` xem có được không

Giải thích tại sao lại truyền đoạn path kia vào là vì thư mục hiện 
