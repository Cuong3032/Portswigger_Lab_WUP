<img width="1617" height="743" alt="image" src="https://github.com/user-attachments/assets/33263345-ee4f-4e17-ac50-5a560bfe5583" />

Đây là yêu cầu cho lab này: yêu cầu ta phải đọc được nội dung trong `/home/carlos/secret` và ta được cung cấp 1 account và gợi ý cho bài này là có thể bypass các file extension trong blacklist bằng 
cách làm kỹ thuật obfuscation.

Đầu tiên ta thử đăng nhập bằng account phía sever đã cung cấp.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/9929c1bf-7f71-40de-834d-fa720dcc9b92" />

Ở đây sau khi đăng nhập thành công ta vào được trang update profile thì ở trang này ta có thể update email và avatar.

<img width="1481" height="530" alt="image" src="https://github.com/user-attachments/assets/ad72a2c9-e63a-4287-96da-cfe0296f0f0a" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/89ea31ee-dbef-4d40-ae99-ef152c9ec56b" />

Ở đây sau khi upload thành công email và avatar thì ta có thấy nó hiển thị như trên

<img width="2563" height="1186" alt="image" src="https://github.com/user-attachments/assets/799e2e35-387f-45df-ad51-b37338435a4f" />

Và gói request này cho ta biết được hình ảnh đã được hiển thị.

Thì với tính năng upload file cũng như là hiển thị nội dung file thì liệu ta có thể upload 1 file không phải hình ảnh hay không?

Ở đây tôi bắt đầu bằng việc thử upload 1 file text

<img width="1563" height="369" alt="image" src="https://github.com/user-attachments/assets/e90f5716-b517-4d04-a799-5c951f3150c3" />

Thì ở đây sau khi upload 1 file text thì nó báo về lỗi và chỉ cho phép upload file có đuôi là `jpg` hoặc `png`. Vậy sẽ ra sao nếu ta thử fake extension như này `python.jpg.php`

<img width="2068" height="1201" alt="image" src="https://github.com/user-attachments/assets/8cf64a73-a050-40e5-9688-e6dd1e8e35bf" />

Thì ở đây đã không thành công. Tiếp theo thì nếu ta chèn vào trong nội dung của file image 1 đoạn code php thì liệu code php bên trong có được thực thi? Được biết rằng trình thông dịch php
sẽ thực thi code php khi nó phát hiện ra tag `<?php & ?>`.

<img width="2563" height="1381" alt="image" src="https://github.com/user-attachments/assets/0e2034aa-9aae-4107-b223-ec1247f836d4" />

có thể thấy ở đây ta đã thành công upload 1 file jpg nhưng có chứa code php. Nhưng quan trọng ở đây là liệu code php của ta có được thực thi hay không?

<img width="2547" height="1443" alt="image" src="https://github.com/user-attachments/assets/232c87b0-7aee-43c1-8955-aceb5f358d35" />

kết quả rất đáng tiếc là không.

Ở đây ta nhớ lại fake extension khi mà ta vẫn chưa test trường hợp nếu giả sử ta chèn 1 null byte vào thì sao như này `python.php%00.jpg` thì khi hệ thống xử lí file xuống ổ cứng hàm hệ thống
quét tới null byte là sẽ end luôn và không đọc tới đuôi jpg dẫn tới httpd sẽ coi đây như là 1 file php.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/74a8297c-b6de-4613-937d-da00547f094f" />

Và giả thuyết này có vẻ đúng khi mà ta đã thành công upload 1 file php lên và nhìn kỹ vào đuôi file thì nó không phải là `php%00` mà là `php` có thể nói là đúng như ta dự đoán khi mà server đọc tới
`null byte` nó sẽ coi như đây là end và loại bỏ cả tất cả chuỗi từ null byte trở đi

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/39a0ad0f-7ed8-43fd-b8c0-abc0242cedfa" />

Có thể thấy kết quả ở đây.

Tiếp đến ta sẽ lấy nội dung bên trong `/home/carlos/secret`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/4a40bd83-1d09-4221-984d-38e2c33d6ba7" />

<img width="1496" height="584" alt="image" src="https://github.com/user-attachments/assets/07d390d9-a103-4222-adf8-b83ca8a28ce2" />

<img width="2563" height="666" alt="image" src="https://github.com/user-attachments/assets/ce8ef01d-8470-4a23-af76-671f3a89c9bd" />
