<img width="1528" height="613" alt="image" src="https://github.com/user-attachments/assets/073014ed-bc6e-47d0-bc29-a1360b0b7a6f" />

Bài lab này yêu cầu thực thi được câu lệnh `whoami` và ta biết rằng trang web có thực thi shell command ở phần nhập liệu product và store IDs của user.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/e0a78d63-767c-43a3-a5de-39e467f987a0" />

Sau khi truy cập vào trang web thì ta biết đây là 1 trang bán hàng thì ta thử bấm view details để xem xét kỹ hơn.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/bef3ffc1-0bf9-40f0-8a5c-9112eda43efd" />

ở đây ta phát hiện ra rằng khi bấm vào xem chi tiết sản phẩm thì họ có dùng tới biến `$_GET` để lấy thông tin sản phẩm 

<img width="976" height="154" alt="image" src="https://github.com/user-attachments/assets/d313f97e-6101-4723-b25b-831019c4cc8a" />

Tiếp đến là ta có thể thử check giá ở từng vùng

<img width="1124" height="327" alt="image" src="https://github.com/user-attachments/assets/83fb8f42-568e-4b14-b1be-aa11be7170e1" />

Sau khi ta thử check stock ở London thì ta bắt được 1 gói tin `POST` với 2 giá trị là `productID` và `storeID` mà ta biết rằng ở phần nhập liệu 2 giá trị này có thực thi shell command.

Vậy sẽ ra sao nếu ta chèn thêm 1 shell command vào 2 giá trị này?

Ở đây tôi thử chèn vào `productID` payload `1;whoami` mục đích là để ngắt chuỗi của command đang gọi tới giá trị vốn có của `productID` là 1 sau đó sẽ thực thi câu lệnh tiếp theo là `whoami`

<img width="2045" height="1333" alt="image" src="https://github.com/user-attachments/assets/d7cb63f8-ceb8-4a05-ad6f-5b69af992312" />

Nhìn hình thì thấy ở đây nó đang báo về 2 lỗi là không phát hiện ra biến `$2` và lệnh `whoami` đang thừa biến `1`. Tiếp theo tôi có thử thêm `;` để ngắt chuỗi không bị thừa biến `1` ở lệnh `whoami`

<img width="2040" height="766" alt="image" src="https://github.com/user-attachments/assets/10e4f190-37c2-4d39-8c99-b53930777e00" />

Thì ở đây nó báo lỗi là không phát hiện biến `$2` và `1` not found thì tôi đang nghĩ tới command mà dev sử dụng nó sẽ như này

    command = "/home/peter-PmV9s5/stockreport.sh " + productId + " " + storeId
    system(command); or exec(commmand);

VÌ với payload `1;whoami` thì câu lệnh nó sẽ thành `/home/peter-PmV9s5/stockreport.sh 1;whoami 1` và được thực thi như sau 

* /home/peter-PmV9s5/stockreport.sh 1; --> lỗi thiếu tham số
* whoami 1 --> thừa operand

Còn với payload `1;whoami;` thì câu lệnh nó sẽ thành `/home/peter-PmV9s5/stockreport.sh 1;whoami; 1` và được thực thi như sau:

* /home/peter-PmV9s5/stockreport.sh 1; --> lỗi thiếu tham số
* whoami được thực thi nhưng lại không hiển thị
* 1 thì do không có câu lệnh nào như thế nên nó báo lỗi

Thì với payload thứ 2 tôi có 1 suy đoán là trong linux thì có 2 luồng là `stdourt` và `stderr` thì whoami thực thi đúng sẽ rơi vào `stdout` và 2 lỗi trên sẽ rơi vào `stderr` thì tôi nghĩ rằng backend
sẽ dựa vào command cuối rơi vào đâu sau đó in ra kết quả của luồng đó như trên là do command cuối là `1` bị lỗi rơi vào `stderr` dẫn tới in ra output của `stderr` thế nếu ta cho command cuối được
rơi vào `stdout` thì có lẽ nó sẽ in ra kết quả của `stdout` đúng không?

Thế tôi sẽ thay `;` ở cuối payload 2 thành `||` toán tử OR thì với toán tử này chỉ cần 1 trong 2 câu lệnh đúng thì kết quả đúng sẽ rơi vào `stdout` còn command sai sẽ được bỏ qua dẫn tới là command
cuối sẽ rơi vào `stdout`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/4278c14e-69cd-4f6e-b52f-846be3ce64a5" />

Có thể thấy suy luận của tôi cũng đã đúng.

Hoặc có thể dễ hơn đó là ta chỉ cần chèn câu lệnh vào sau `storeID` là được, câu lệnh được thực thi nó sẽ như này: `/home/peter-PmV9s5/stockreport.sh 1 1;whoami`, và như thế thì câu lệnh đầu tiên sẽ
được thực thi thành công không có lỗi thiếu tham số và câu lệnh thứ 2 cũng được thực thi thành công mà không có lỗi gì

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/342cdbb5-bc19-43e8-929e-c0567e6640dd" />

vậy là ta đã thành công biết được current user.
