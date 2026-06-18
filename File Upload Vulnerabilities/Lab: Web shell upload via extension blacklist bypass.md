<img width="1498" height="541" alt="image" src="https://github.com/user-attachments/assets/3b639a48-9493-476f-a029-13f1dee57d2f" />

Đối với bài này thì yêu cầu ta phải đọc được thông tin trong `/home/carlos/secret` và dựa vào tài khoản đã đươc cấp sẵn. Gợi ý ở đây là đuôi file đã được cho vào blacklist nhưng vẫn có thể bypass vì lỗi
cơ bản trong việc cấu hình.

Đầu tiên hãy thử đăng nhập bằng account đã cho

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/1a1f9422-8d54-4748-8df0-877f24211592" />

Ở đây sau khi đăng nhập thành công ta vào được trang update profile thì ở trang này ta có thể update email và avatar.

<img width="1719" height="364" alt="image" src="https://github.com/user-attachments/assets/a162321b-ec63-49d1-a72f-2278e7b0b598" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/924f0719-bee1-4fe0-9471-4fc94e564dbd" />

Ở đây sau khi upload thành công email và avatar thì ta có thấy nó hiển thị như trên

<img width="2563" height="1193" alt="image" src="https://github.com/user-attachments/assets/49a86bfb-fe23-4ae6-96f9-02514f1de516" />

Và gói request này cho ta biết được hình ảnh đã được hiển thị.

Thì với tính năng upload file cũng như là hiển thị nội dung file thì liệu ta có thể upload 1 file không phải hình ảnh hay không?

Ở đây tôi bắt đầu bằng việc thử upload 1 file text

<img width="1791" height="456" alt="image" src="https://github.com/user-attachments/assets/d61683f7-bb13-4331-a291-56f206c56d35" />

Thì file text ở đây đã thành công được upload lên và như dưới thì nội dung của nó cũng đã được hiển thị

<img width="2562" height="1069" alt="image" src="https://github.com/user-attachments/assets/11d87b7a-6368-4c95-852d-8eabc042768a" />

Vậy nếu ta đã có thể upload 1 file text thì liệu ta có thể upload 1 flie webshell hay không?

<img width="2046" height="1101" alt="image" src="https://github.com/user-attachments/assets/b57ced3b-811c-49de-84f7-e77bfae9e34b" />

Ở đây mặc dù ta đã có thể upload 1 file text, nhưng ta lại không thể upload 1 file php lên.

Tôi đặt ra giả thuyết rằng, liệu ở đây config chỉ cấm mỗi file có đuôi php, thì liệu là nếu tôi dùng đuôi giả thì nó có thể qua được hay không ví dụ như `python.xyz.php`

<img width="2055" height="1146" alt="image" src="https://github.com/user-attachments/assets/b246e6b2-2811-4373-b678-64fd980f98ba" />

Thì rõ ràng là không được, vậy các đuôi file khác nhưng vẫn có thể thực thi được code php thì sao?

<img width="2563" height="1441" alt="image" src="https://github.com/user-attachments/assets/d3b9b4c3-605f-458d-b5d6-cd05eb35c04f" />

Như ở đây tôi đã thành công upload 1 file có extension có thể thực thi code php là `.phar`, thì để chắc chắn hãy kiểm tra xem code có thực sự được thực thi hay không?

<img width="2491" height="1443" alt="image" src="https://github.com/user-attachments/assets/ae559616-8418-4fb1-9729-72dd141c9746" />

Như hình trên thì đoạn command `phpinfo()` đã được thực thi thành công

<img width="2563" height="1370" alt="image" src="https://github.com/user-attachments/assets/4b85ec45-0abc-4c38-8b08-8e76c8882ae8" />

Tôi đổi nội dung của file để có thể lấy được nội dung bên trong `/home/carlos/secret`

<img width="1673" height="414" alt="image" src="https://github.com/user-attachments/assets/b220a90f-bb01-4041-97f5-a93b2dc4840d" />

<img width="2563" height="530" alt="image" src="https://github.com/user-attachments/assets/4a36592e-d3cb-4dc2-8e9c-a252e3134bea" />
