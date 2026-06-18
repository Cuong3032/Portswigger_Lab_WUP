<img width="1509" height="563" alt="image" src="https://github.com/user-attachments/assets/d88f5309-1992-4d8e-b56b-2cea60044678" />

Đây là yêu cầu cho lab này: yêu cầu ta phải đọc được nội dung trong `/home/carlos/secret` và ta được cung cấp 1 account và ở đây có gợi ý rằng server được config để chặn các file thực thi từ phía user,
nhưng có thể exploit bằng lỗ hổng thứ 2.

Đầu tiên ta thử đăng nhập bằng account phía sever đã cung cấp.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/76072e36-686b-4d15-8791-9893b7501d89" />

Ở đây sau khi đăng nhập thành công ta vào được trang update profile thì ở trang này ta có thể update email và avatar.

<img width="1499" height="320" alt="image" src="https://github.com/user-attachments/assets/2a035bba-cced-4e96-b038-3314e58751d5" />

<img width="1416" height="990" alt="image" src="https://github.com/user-attachments/assets/ac5f39c3-715d-489e-ad3a-e686041b8985" />

Ở đây sau khi upload thành công email và avatar thì ta có thấy nó hiển thị như trên

<img width="2563" height="1207" alt="image" src="https://github.com/user-attachments/assets/a28af153-9143-4b6a-a955-6e146b4c6631" />

Và gói request này cho ta biết được hình ảnh đã được hiển thị.

Thì với tính năng upload file cũng như là hiển thị nội dung file thì liệu ta có thể upload 1 file không phải hình ảnh hay không?

Ở đây tôi bắt đầu bằng việc thử upload 1 file text

<img width="1367" height="335" alt="image" src="https://github.com/user-attachments/assets/db7ef82c-b56c-4cdf-89ba-cd16b27395af" />

Thì file text ở đây đã thành công được upload lên và như dưới thì nội dung của nó cũng đã được hiển thị

<img width="2288" height="775" alt="image" src="https://github.com/user-attachments/assets/0be49642-3d72-43a4-a851-709c58a04ba7" />

Vậy nếu ta đã có thể upload 1 file text thì liệu ta có thể upload 1 flie webshell hay không?

<img width="2039" height="1329" alt="image" src="https://github.com/user-attachments/assets/74606dd2-9ab6-4dcd-a2f3-df9a8f71a5f4" />

Có thể thấy ta đã thành công upload 1 file webshell lên. Nhưng nhớ lại gợi ý ở trên thì họ có nói rằng đã ngăn chặn các file có thể thực thi ở đây, Vậy hãy xem thử xem webshell của ta có được thực 
thi hay không?

<img width="2069" height="944" alt="image" src="https://github.com/user-attachments/assets/975b9ad7-1340-47ca-a81e-a8c0adef1890" />

Có vẻ như họ đã config gì đó khiến cho code php bên trong webshell bị đối xử như 1 đoạn text.

Ở đây tôi đặt ra giả thuyết rằng là web sever đang conf bằng apache và thường thì có 1 file là apache2.conf sẽ config cho web sever và thường thì tôi thấy họ hay config cho từng thư mục các quyền
khác nhau, vậy rất có khả năng ở thư mục chứa file upload đã bị config sao cho tất cả các file thực thi sẽ được đối xử như 1 file text. Vậy sẽ ra sao nếu tôi đi về các thư mục phía trước, nơi không
có config chống việc thực thi file?

<img width="1016" height="984" alt="image" src="https://github.com/user-attachments/assets/e5f714ee-cc09-45c9-abc5-a6dfa7588e61" />

Như ở đây tôi thử thêm vào `filename` 1 đoạn như này `../` để có thể upload nó lên vào thư mục phía trước thư mục upload.

<img width="2056" height="1060" alt="image" src="https://github.com/user-attachments/assets/eed613f2-3d5b-44f6-a509-90c78376b0d3" />

Thì ở đây đã có lớp filter nào đó chặn các ký tự như `../` dẫn tới filename vẫn là `python.php`, vậy ta thử encode xem có được không?

<img width="2296" height="449" alt="image" src="https://github.com/user-attachments/assets/55c66ff1-43fa-4a52-8f8f-b822b68c81f9" />

<img width="2053" height="1170" alt="image" src="https://github.com/user-attachments/assets/a5ce90ad-3eb4-4030-ab77-f9d754399b34" />

có thể thấy sau khi encode thì ta đã thành công upload 1 file kèm theo lỗi path traversal.

<img width="2563" height="1166" alt="image" src="https://github.com/user-attachments/assets/6f4e1edb-a3f0-433f-9d22-1df5faf89400" />

Thì nhìn vào payload kia thì đây có khả năng là do url đang hiểu nhầm filename và nó tưởng tất cả đoạn `%2e%2e%2fpython.php` là 1 filename bình thường mà bên trong thư mục chứa file upload đâu
có file nào tên như vậy dẫn tới là `NOT FOUND`

Thì lúc này nếu giả thuyết của ta đúng thì hiện tại file của chúng ta đã đuọc lùi 1 thư mục là ở thư mục `files`, giờ ta thử truy cập vào path `/files/ppython.php` xem file của chúng ta đã 
được thực thi chưa?

<img width="2509" height="1443" alt="image" src="https://github.com/user-attachments/assets/98421683-e69a-4807-bcee-1fdd9c431716" />

Thì có thể thấy file webshell ta upload đã được thực thi, tiếp đến là phần đọc nội dung bên trong `/home/carlos/secret`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/2a5f1933-3793-4604-bba8-8a7a353b3f20" />

<img width="1377" height="514" alt="image" src="https://github.com/user-attachments/assets/c141fbc4-3332-4010-89ea-47776211b070" />
