<img width="1512" height="489" alt="image" src="https://github.com/user-attachments/assets/564e5a80-29dd-409b-95ec-e11db65bda54" />

Yêu cầu: Chèn vào 1 thuộc tính và gọi hàm `alert()`.

Gợi ý: Bài lab này chứa một lỗ hổng Reflected Cross-Site Scripting (XSS phản xạ) trong chức năng tìm kiếm blog, nơi mà các dấu ngoặc nhọn (< và >) đã bị mã hóa theo chuẩn HTML.

<img width="2504" height="1443" alt="image" src="https://github.com/user-attachments/assets/51140045-f863-4a90-b020-d1324ee0f488" />

Bắt đầu bằng việc thử gõ vào ô tìm kiếm.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/80422298-74cb-4175-8bc4-05d699b135c3" />

<img width="2500" height="1443" alt="image" src="https://github.com/user-attachments/assets/9989140e-2633-49af-ba7e-bdc5240eea4a" />

Thì ở đây ta thấy phần ta nhập vào tìm kiếm nó xuất hiện ở 2 nơi 1 là trong văn bản tự do của tag `h1` thứ 2 là trong giá trị của thuộc tính value của tag `input`.

Tiếp theo ta sẽ thử chèn vào 1 tag bất kỳ.

<img width="2499" height="1443" alt="image" src="https://github.com/user-attachments/assets/e9be4ed5-919d-41b4-b940-a3c88b428c0c" />

<img width="2509" height="1443" alt="image" src="https://github.com/user-attachments/assets/c37fd7ae-42a8-4f8a-96be-4ebdaf5b3be0" />

Có thể thấy các ký tự (< và >) đã bị mã hóa theo chuẩn HTML, do đó ta không thể lợi dụng thông qua các tag nữa, nhưng ở đây ta có thể lợi dung qua attribute khi mà giá trị của attribute `value` của
tag `input` phụ thuộc vào giá trị ta nhập vào thanh tìm kiếm.

```html
<input type=text placeholder='Search the blog...' name=search value="&lt;h1&gt;a&lt;/h1&gt;">
```

Ở đây tôi sẽ tìm cách thoát khỏi attribute `value` và chèn thêm 1 event handler vào để bật `alert()` lên.

Payload:

```html
" [event handler] = "alert(origin)
```

Thì event handler ở đây tôi có thể sử dụng nhiều loại tùy vào cách bạn muốn sử dụng, như ở đây tôi có thể dùng `onfocus` kết hợp với `autofocus` để nó tự động trỏ chuột vào ô input rồi `onfocus` được
tự động kích hoạt dẫn tới nổ `alert()`. Hoặc có thể dùng `onmouseover` kích hoạt khi con trỏ chuột lướt vào phạm vi của ô input, `onmouseout` kích hoạt khi con trỏ chuột rời khỏi ô input, `onclick` 
kích hoạt khi người dùng click chuột trái vào ô input để chuẩn bị gõ chữ.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/d4699b4f-ad59-4663-942d-11c111fbfc60" />
