<img width="1583" height="558" alt="image" src="https://github.com/user-attachments/assets/bfdd9cac-817d-403a-9a86-f8a51ea9caf9" />

Đây là yêu cầu đối với bài lab này. Ở bài lab này có cung cấp cho ta 1 account thử đăng nhập vào xem.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/7200fe1c-2d23-499c-ae7c-41f2d22cbbdf" />

Ở đây sau khi đăng nhập xong ta vào được trang upadate profile, ở trang này thì ta có thể update avatar và email.

Ta thử chèn 1 hình ảnh và thay đổi email bất kỳ.

<img width="2359" height="599" alt="image" src="https://github.com/user-attachments/assets/9e3ad762-6357-47cc-bc0a-c24fa99eca5f" />

Ở đây ta đã thành công upload 1 hình ảnh lên

<img width="2496" height="1443" alt="image" src="https://github.com/user-attachments/assets/5ef94ac6-a54c-46d8-90a1-7f76f57e9183" />

Câu hỏi đặt ra là ta có thể upload 1 file nào đó khác ngoài hình ảnh hay không?

<img width="2561" height="1443" alt="image" src="https://github.com/user-attachments/assets/25f32a19-3104-467b-810f-3f3623598757" />

Ở đây tôi thử upload 1 file text.

<img width="1765" height="387" alt="image" src="https://github.com/user-attachments/assets/3b0b07ad-64e3-4849-b9ff-96c45954f8ce" />

Ở đây nó đã báo lại rằng bạn không thể upload 1 file có Content-Type là `text/plain` mà chỉ có thể là `image/jpeg` và `image/png`.

Vậy nếu ta sửa Content-Type này thành `image/png` hoặc `image/jpeg` thì liệu có thành công hay không?

<img width="2055" height="1384" alt="image" src="https://github.com/user-attachments/assets/2f61edb3-3c4c-47c2-99de-dd9241289cda" />

có thể thấy ta đã thành công upload 1 file khác ngoài ảnh. Lí do ta có thể upload là do sever chỉ kiểm tra mỗi cái header của file đó chứ không kiểm tra bản chất của file đó,
nó giống như bạn nhận hàng nhưng bạn chỉ kiểm tra cái nhãn dán ngoài cùng của món hàng đó, còn nội dung thực chất bên trong thì không kiểm tra dẫn tới người có ý đồ xấu có thể mạo danh món hàng
đó của bạn.

<img width="2239" height="1114" alt="image" src="https://github.com/user-attachments/assets/2c996745-b11c-4984-8420-1d554b1815c5" />

<img width="1905" height="581" alt="image" src="https://github.com/user-attachments/assets/47a8a5c5-d4f1-407f-adbc-2923055e94fd" />

Có thể thấy rõ được nội dung của file text mà ta đã upload, Vậy sẽ ra sao nếu ta làm tương tự nhưng là với 1 file webshell.

<img width="2560" height="1443" alt="image" src="https://github.com/user-attachments/assets/d1b9950e-9b82-4fb8-96e4-fc427362760d" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/9031637d-145e-4ad2-b36b-7362aed4a35e" />

Ở đây tôi đã thành công upload 1 file webshell lên giờ tôi chỉ cần sửa tí là có thể lấy được thông tin bên trong `/home/carlos/secret`

<img width="2540" height="424" alt="image" src="https://github.com/user-attachments/assets/ed3a518b-7099-4cca-ab98-8bdee5b19a27" />

<img width="2561" height="540" alt="image" src="https://github.com/user-attachments/assets/a2f1de6b-8c0c-4d5c-8dd1-f1bca1aa5fad" />
