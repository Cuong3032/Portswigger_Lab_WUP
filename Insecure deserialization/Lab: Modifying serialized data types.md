<img width="1528" height="488" alt="image" src="https://github.com/user-attachments/assets/ca30eb0f-cfab-4057-a121-2d236d195867" />

Yêu cầu: đâng nhập vào account `administrator` và xóa đi được user `Carlos`

Gợi ý: Lab này sử dụng cơ chế phiên (session) dựa trên quá trình tuần tự hóa (serialization) và do đó tồn tại lỗ hổng leo thang đặc quyền (privilege escalation).

Hint: Để đăng nhập đươc vào user khác thì cần khai thác 1 cái điểm kỳ lạ trong cách PHP so sánh 2 loại dữ liệu khác nhau. Và việc so sánh ở 2 kiểu dữ liệu khác nhau ở mỗi version của PHP là khác nhau.
Phiên bản lab này đang sử dụng là PHP 7.x trở về trước.

<img width="1452" height="365" alt="image" src="https://github.com/user-attachments/assets/54adf701-f352-44b5-a18d-6162026e7d33" />

<img width="2510" height="1443" alt="image" src="https://github.com/user-attachments/assets/f6a3337b-9cb2-43d2-9746-32e87470847d" />

Hiện tại chưa đăng nhập thì session cookie đang được để trống.

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/0205882e-59c7-4e86-8bad-fe443a9d0fdf" />

Sau khi đăng nhập bằng account đã cho, session cookie đã được update.

<img width="2563" height="965" alt="image" src="https://github.com/user-attachments/assets/2568be91-1353-46ae-a922-979df0e598ee" />

Ở đây ta phát hiện ra đoạn session này được mã hóa bằng base64 sau khi decode ra ta được định dạng PHP Serialization.

    O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"c85lbtg8rrwc43pzn7zh6y9bayujjsvz";}

Trong định dạng này có 1 class `User`(độ dài 4 ký tự) với 2 attribute:
* `username`(độ dài 8 ký tự):
  * `value`(string) = wiener, độ dài s:6 ký tự
* `access_token`(độ dài 12 ký tự)
  * `value`(string) = 'c85lbtg8rrwc43pzn7zh6y9bayujjsvz'

Thì ở đây nó sẽ lấy username từ `username` này sẽ lấy `access_token` trong database ròi thực hiện so sánh với token trong cookie nếu đúng thì cho đăng nhập.

Ở đây để kiểm chứng xem nếu sai `access_token` thì có đăng nhập được hay không ta sẽ thử thay đổi giá trị của username thành `administrator`.

<img width="2259" height="550" alt="image" src="https://github.com/user-attachments/assets/79553bf3-e378-4477-ac1f-8ad3042f02b6" />

Ta lấy payload đã mã hóa kia thay thế cho cookie hiện tại.

<img width="2495" height="1443" alt="image" src="https://github.com/user-attachments/assets/c3b902a8-de89-4e64-b324-00d39441ae94" />

Ở đây nó có trả về lỗi như trên là access token không khớp. Vậy ta sẽ phải làm cách nào đó để bypass được cái access token này. Dựa vào hint đề bài cung cấp là so sánh 2 kiểu dữ liệu khác nhau của 
PHP.

<img width="1624" height="771" alt="image" src="https://github.com/user-attachments/assets/e858974c-5e3a-4f8e-aaba-0e248ac252ac" />

Ở đây có nói rằng khi so sánh lỏng lẻo `Loose comparisons with ==` thì khi so sánh 0 với 1 chuỗi bắt đầu bằng ký tự khác 0 và không phải số thì sẽ trả về là `true` với các version trước PHP 8.0.0
Và với các phiên bản sau đó sẽ là `false`.

Vậy ta đặt giả thuyết ở đây rằng dev bài lab này sẽ sử dụng dấu `==` để so sánh access token cookie với database thì nếu ta đổi value `access_token` trong định dạng trên thành `0` thì khi so sánh
nó sẽ thành như này:

    0 == 'acces_token_database'

Mà với version PHP đang sử dụng trong bài lab này đang là PHP 7.x trở về trước thì phép so sánh kia sẽ trả về `TRUE` dẫn tới ta có thể chui qua được bước xác thực `access_token`.

<img width="2563" height="654" alt="image" src="https://github.com/user-attachments/assets/78f295f9-7faf-48bf-b09b-134301271835" />

Ta sẽ dùng payload đã mã hóa trên để thay thế cho cookie hiện tại.

<img width="2512" height="1443" alt="image" src="https://github.com/user-attachments/assets/721e6f9f-1ca0-4fab-b32d-04b0bf750e56" />

Đã đăng nhập thành công vào account `administrator` và giờ ta chỉ cần xóa user `Carlos`.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/dac51f59-d182-4f02-b22b-ee1b2a9a9f30" />
