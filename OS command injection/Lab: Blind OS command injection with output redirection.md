<img width="1542" height="764" alt="image" src="https://github.com/user-attachments/assets/c3e9132d-d557-4948-9165-53c514b29b88" />

Yêu cầu Lab: thực thi `whoami` và lấy được output
Gợi ý: có thể thực thi shell command trong phần nhập liệu của user, output không được in ra, có thể redirect output vào folder `/var/www/images/`(folder này có thể ghi vào)

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/aa67c10c-eec0-4f33-9ab6-8eb79d8f0cdb" />

Đối với bài này ta sẽ tập trung vào phần nhập liệu bên trong `submit feedback` 

<img width="2560" height="1443" alt="image" src="https://github.com/user-attachments/assets/f65a7704-1476-4516-8b5c-07c72ef27bab" />

Thì ở đây có 4 trường và sau khi thử thì tôi phát hiện ra ở trường `email` có thể thực thi được OS command vì khi tôi thử chèn command `sleep` thì respone đã bị delay.

Vậy để tiếp cận mục tiêu là thực thi lệnh `whoami` và lấy được output thì ta sẽ ghi output vào folder `/var/www/images/` mà đã được gợi ý là có thể ghi vào.

<img width="2055" height="965" alt="image" src="https://github.com/user-attachments/assets/47d09e31-5a8e-4692-8da7-15f8b266c2b2" />

<img width="2058" height="759" alt="image" src="https://github.com/user-attachments/assets/a769d969-096a-4dea-aa71-38af8499d42e" />

Thế là đã thành công đọc được nội dung output của `whoami`

<img width="2469" height="1443" alt="image" src="https://github.com/user-attachments/assets/42582aee-a88d-43ad-ad0d-29cab346d4e7" />
