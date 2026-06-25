<img width="1583" height="489" alt="image" src="https://github.com/user-attachments/assets/b8ceac1b-73ea-49b2-9c45-de3858d6fda4" />

Yêu cầu: Chiếm quyền admin và xóa user `Carlos`.

Gợi ý: Lab này sử dụng cơ chế phiên (session) dựa trên quá trình tuần tự hóa (serialization) và do đó tồn tại lỗ hổng leo thang đặc quyền (privilege escalation).

Account được cung cấp: `wiener:peter`.

<img width="2499" height="1443" alt="image" src="https://github.com/user-attachments/assets/cbbf384a-ac59-465a-ba0d-2accf3ec3d3e" />

Hiện tại chưa đăng nhập thì session cookie đang được để trống. 

<img width="2523" height="1348" alt="image" src="https://github.com/user-attachments/assets/9e0813ab-fffb-4a81-afcd-b02058a75101" />

Sau khi đăng nhập bằng account đã cho, session cookie đã được update.

<img width="2563" height="895" alt="image" src="https://github.com/user-attachments/assets/b38cddbb-9437-478d-9ee0-596cca7a87c7" />

Ở đây ta phát hiện ra đoạn session này được mã hóa bằng `base64` sau khi decode ra ta được định dạng PHP Serialization. 

    O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}

Trong định dạng này có 1 class `User`(độ dài 4 ký tự) với 2 attribute:
* `username`(độ dài 8 ký tự):
  * `value`(string) = wiener, độ dài s:6 ký tự
* `admin`(độ dài 5 ký tự)
  * `value`(boolean) = 0(FALSE)

Vậy ở đây ta có thể hiểu rằng phiên đăng nhập này là của user tên `wiener` và không có quyền admin.

Nếu ta sửa giá trị của thuộc tính `admin` thành 1(TRUE) thì liệu user `wiener` có quyền admin hay không?

<img width="1191" height="444" alt="image" src="https://github.com/user-attachments/assets/30e4ede5-7915-4568-bb90-83a68854beae" />

Ta lấy đoạn payload đã được encode base64 thay thế cookie hiện tại

<img width="2510" height="1443" alt="image" src="https://github.com/user-attachments/assets/4b6ee135-75c5-44ef-ad97-7b69b0447ac5" />

Ở trong trang web đã phát hiện thêm 1 feature mới là `Admin panel`

<img width="2501" height="852" alt="image" src="https://github.com/user-attachments/assets/68449f9d-468f-4168-b9e2-de21f0d2b728" />

Ở bên trong này ta có quyền xóa đi các user hiện có, vậy ta sẽ xóa đi user `carlos` theo yêu cầu bài lab.

<img width="2503" height="1057" alt="image" src="https://github.com/user-attachments/assets/0bd395f8-4d52-47bc-808f-1bf801208d8f" />
