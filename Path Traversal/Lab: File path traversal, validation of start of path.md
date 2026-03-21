<img width="1246" height="585" alt="image" src="https://github.com/user-attachments/assets/5f5e4638-070b-48bc-9fac-79efa717e7aa" />

có thể thấy rằng là lần này tham số được truyền vào là cả 1 đường dẫn tuyệt đối là `/var/www/images/3.jpg` và đối chiếu với gợi ý của bài lab là đường dẫn được cung cấp phải bắt đầu
bằng thư mục dự kiến nhưng nó không có kiểm tra phần đuôi vì vậy chỉ cần truyền `../` để lùi từng thư mục đến thư mục gốc rồi đi thẳng vào `etc/passwd` là được như vậy ta sẽ truyền 
`/var/www/images/../../../etc/passwd` vào

<img width="1219" height="490" alt="image" src="https://github.com/user-attachments/assets/afdb1ca3-e414-4f02-a558-2d74329d2987" />

<img width="1280" height="612" alt="image" src="https://github.com/user-attachments/assets/13849e68-1033-4f7d-b04d-ddf001529bfa" />

vậy là xong lab này

