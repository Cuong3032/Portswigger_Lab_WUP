<img width="1642" height="874" alt="image" src="https://github.com/user-attachments/assets/31947805-84aa-47af-9e3b-91ddb5105ff6" />

Đây là yêu cầu cho lab này: yêu cầu ta phải đọc được nội dung trong `/home/carlos/secret` và ta được cung cấp 1 account và ở đây có gợi ý rằng lần này hệ thống không kiểm tra bằng đuôi file nữa, mà
nó sẽ dựa vào nội dung của file để xác định đó là loại file gì.

Đầu tiên ta thử đăng nhập bằng account phía sever đã cung cấp.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/7062e928-1142-4aff-b588-2d6f81d9d9e6" />

Ở đây sau khi đăng nhập thành công ta vào được trang update profile thì ở trang này ta có thể update email và avatar.

<img width="1507" height="495" alt="image" src="https://github.com/user-attachments/assets/9d12a16e-fdba-45f6-9fe7-ac9153a9238d" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/6fedc3ac-4d16-49b0-8f69-4c56a838493e" />

Ở đây sau khi upload thành công email và avatar thì ta có thấy nó hiển thị như trên

<img width="2563" height="1079" alt="image" src="https://github.com/user-attachments/assets/e32e4c39-cfd3-4c1f-9195-925c680d6840" />

Và gói request này cho ta biết được hình ảnh đã được hiển thị.

Thì với tính năng upload file cũng như là hiển thị nội dung file thì liệu ta có thể upload 1 file không phải hình ảnh hay không?

Ở đây tôi bắt đầu bằng việc thử upload 1 file text

<img width="1724" height="426" alt="image" src="https://github.com/user-attachments/assets/ad830c90-5ac7-410e-8e92-f27d2bc44385" />

thì ở đây nó báo rằng đây không phải là 1 file image hợp lệ. Vậy thì sẽ ra sao nếu ta dùng fake extension như `abc.jpg.php` hoặc `abc.php%00.jpg` thì tôi nghĩ đều không được do ở đây có gợi ý phía trên
là nó kiểm tra dựa vào content chứ không phải extension file do đó có thay đổi file extension như nào thì đều không dược.

<img width="2071" height="1256" alt="image" src="https://github.com/user-attachments/assets/7f37a158-4a73-4403-8bdc-49ad11a3c42a" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/dd7dc67e-d192-4a4c-9646-e3d602899322" />

có thể thấy đều không được.

Vậy cơ chế kiểm tra bằng nội dung file là như nào thì đó là có thể kiểm tra thông qua magic byte là dựa những byte đầu của file để xác định đó là loại file gì.

Thì liệu ta có thể fake magic byte để có thể bypass được không?

Được biết magic byte của jpg là `FF D8 FF E0` và nếu được hiển thị dưới dạng text thì nó sẽ khá kỳ lạ đại loại như này: `ÿØÿà`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/fa994fbb-a23d-45fd-85b9-234cf5e5bff6" />

Có vẻ như ở đây nó không lọc theo mỗi magic byte rồi, có lẽ nó còn xác định dựa vào metadata sau khi bóc tách file đó ra thì Nhảy đến các khối dữ liệu (chunks/segments) chứa thông tin của ảnh. 
Trong JPEG, EXIF data thường nằm ở segment có tên là APP1. Bóc tách các trường thông tin trong EXIF (như Chiều rộng, Chiều cao, Tên máy ảnh, Tác giả, Bình luận...). Thì ở đây có các trường tự do
có thể thay đổi như `comment`, `Artist`, `Copyright`. Vậy sẽ ra sao nếu ta thay đổi các trường này bằng 1 đoạn payload nguy hiểm.

Thì ở đây có công cụ để làm được điều đó là `ExifTool`

<img width="1546" height="356" alt="image" src="https://github.com/user-attachments/assets/1f0aeb32-608c-41a7-92c2-d65735b13620" />

Thì ở đây ta thử thay đổi trường `Comment` bằng payload như dưới.

    exiftool -Comment="<?php system('whoami'); ?>" payload.jpg -o payload.php

<img width="1740" height="287" alt="image" src="https://github.com/user-attachments/assets/5b43757f-e494-4da3-8800-3217151a9833" />

giờ ta sẽ lấy file này upload lên xem có được không?

<img width="1740" height="287" alt="image" src="https://github.com/user-attachments/assets/9dcd4fef-8b35-4b8e-b52c-dc95bdd74dcb" />

thì ta thấy rằng ở đây rất khó để đọc được response thì thay vào đó ta sửa payload thành

    exiftool -Comment="<?php echo 'START ' . system('whoami') . ' END'; ?>" payload.jpg -o payload.php

Để lát nữa sau khi ra respone ta có thể dùng search engine để scan cho nhanh

<img width="1017" height="141" alt="image" src="https://github.com/user-attachments/assets/689ac857-9a84-4506-a3d8-3ffba7ec3bbf" />

thì ở đây ta đã thành công đọc được respone của câu lệnh trên, tiếp đến là ta sẽ sửa lại payload 1 chút để đọc nội dung bên trong `/home/carlos/secret`

    exiftool -Comment="<?php echo 'START ' . system('cat /home/carlos/secret') . ' END'; ?>" payload.jpg -o payload.php

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a9199146-e847-41e4-b511-218e3b0829e7" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/02dac79b-ac30-4820-8b8d-85f680f5f018" />
