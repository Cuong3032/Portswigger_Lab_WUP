<img width="1242" height="551" alt="image" src="https://github.com/user-attachments/assets/a613990d-67c9-4f4c-b03c-2b9b44d616b3" />

theo yêu cầu bài lab là `The application validates that the supplied filename ends with the expected file extension.` sẽ xác thực xem chuỗi này có kết thúc đúng phần mở rộng tệp không
vậy thì bypass kiểu j khi mà nếu ta truyền `../../../etc/passwd` thì sẽ không đúng phần mở rộng tệp nhưng nếu truyền như `../../../etc/passwd.png` thì đúng định dạng tệp nhưng hệ thống
sẽ báo lỗi là không có tệp này 

<img width="1236" height="259" alt="image" src="https://github.com/user-attachments/assets/b929b3c5-82b1-440b-90c6-771b7e6f9463" />

<img width="1234" height="319" alt="image" src="https://github.com/user-attachments/assets/b2dafc23-3776-4ff8-8512-a3e363edd9c5" />

vậy thì ở đây ta sẽ sử dụng `null byte` thì đối với họ ngôn ngữ C/C++ thì chuỗi văn bản phải kết thúc bằng `null byte` được biểu diễn là  `\0` trong C hoặc `%00` trong URL
vậy thì ta sẽ truyền `../../../etc/passwd%00.png` vào sau đó nó sẽ lọc thấy payload này có đúng phần mở rộng tệp và cho qua sau đó nó sẽ thực hiện câu lệnh này và khi gặp 
`%00` thì nó sẽ dừng vì hiểu đây là chuỗi đã kết thúc và bỏ qua phần `.png`

<img width="1239" height="539" alt="image" src="https://github.com/user-attachments/assets/ac4c4060-a2f6-434b-bf45-fe8bb4073c7b" />

<img width="1280" height="606" alt="image" src="https://github.com/user-attachments/assets/d8e34851-201b-4e93-8de1-081751133dbf" />

thế là thành công.




