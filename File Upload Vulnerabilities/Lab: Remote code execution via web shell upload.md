<img width="1684" height="821" alt="image" src="https://github.com/user-attachments/assets/6169ff0c-4abe-414c-b42a-a548d88e4fcb" />

Ở đây ta có yêu cầu bài lab như này, thì nó yêu cầu chúng ta lấy được content trong `/home/carlos/secret` và ở đây ta được cấp sẵn tài khoản là `wiener:peter`.

Ta thử đăng nhập bằng account đã cho.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/e0b69f3b-c419-4dd6-8603-9b1e2209dc0d" />

Ở đây ta có thể update profile của account này với update email và avatar, thử điền 1 email kèm theo upload 1 hình ảnh xem sao.

<img width="1526" height="591" alt="image" src="https://github.com/user-attachments/assets/2813e720-d255-4982-b35b-7d83525850ec" />

Ở đây ta đã thành công upload avatar.

Thì ở đây có thể thấy sau khi upload xong ảnh nó sẽ có 1 gói request get tới cái ảnh ta đã upload như dưới

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/8c09f94c-4fed-49dd-b83d-6c32e7d10a1b" />

Vậy ở đây có nhất thiết phải upload 1 hình ảnh hay không? Hoặc nói là tôi có thể upload 1 file không phải ảnh hay không?

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/81b315b7-33e0-4863-a8f6-7e3770fd5e33" />

Ở đây tôi đang thử upload 1 file text.

<img width="1595" height="431" alt="image" src="https://github.com/user-attachments/assets/da70329a-3fdd-4361-9217-26fde56cefec" />

Có thể tháy ở đây tôi đã upload thành công file text trên

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/96e65f83-3f8d-42c5-be2e-180bd5ecf313" />

Sau khi back lại trang update profile thì lại có 1 gói request hiện thị nội dung của file ta vừa upload, và như trên ta có thể thấy được nội dung của file ta vừa upload.

Nếu ta đã có thể upload 1 file bất kỳ và hiển thị nội dung của file đó, thì sẽ ra sao nếu tôi thử upload web shell lên xem?

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/ad550c81-ed9e-489f-979e-433f073ed67c" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/7bac9cf4-950b-4b9c-93b6-7fc03f2f7694" />

Như vậy tôi đã thành công upload 1 web shell và hiển thị ra được nội dung của file đó. Giờ ta muốn lấy thông tin của `/home/carlos/secret` thì chỉ cần sửa lại command trong webshell là được

<img width="2563" height="1357" alt="image" src="https://github.com/user-attachments/assets/0126b023-57ea-4cd8-85c7-b7871a5c1ef7" />

<img width="2563" height="834" alt="image" src="https://github.com/user-attachments/assets/f74eb3f9-294b-4cb0-bdb9-030e4ed4881b" />

Vậy là ta đã lấy được nội dung bên trong `/home/carlos/secret` --> `dlg1MtQtJxRLj03s2ChYQ4rowXaVI8Tz`

<img width="2563" height="642" alt="image" src="https://github.com/user-attachments/assets/de3c0c77-b3f8-45fe-8534-0eb77cc35243" />
