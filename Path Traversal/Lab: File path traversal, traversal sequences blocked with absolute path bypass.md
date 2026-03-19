<img width="2505" height="1189" alt="image" src="https://github.com/user-attachments/assets/3b5e5d09-3ab6-4486-9649-7bf2f2692f63" />

hãy mở một hình bất kỳ trong trang này sang tab mới 

<img width="1246" height="576" alt="image" src="https://github.com/user-attachments/assets/2e784031-5079-44a5-b4e1-8c0f886dfeff" />

trước tiên hãy phân tích đoạn gợi ý này có ý gì `The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.`
ta có thể thấy hệ thống đã thiết lập bộ lọc (filter) để chặn các chuỗi duyệt thư mục như ../. Tuy nhiên, lỗ hổng nằm ở việc ứng dụng nối thẳng đầu vào của người dùng vào một thư mục 
mặc định mà không kiểm tra xem đó là đường dẫn tương đối hay tuyệt đối. Do đó, để bypass cơ chế bảo vệ này, ta có thể cung cấp trực tiếp một đường dẫn tuyệt đối là /etc/passwd

thử chèn payload này `/etc/passwd` thay cho value trong tham số filename

<img width="1247" height="470" alt="image" src="https://github.com/user-attachments/assets/89fca881-83de-4438-8ce5-f40709c16009" />

như hình là ta đã lấy được nội dung chứa trong `etc/passwd` và thành công lab này 

