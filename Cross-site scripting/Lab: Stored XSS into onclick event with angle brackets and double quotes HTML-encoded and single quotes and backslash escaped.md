<img width="1544" height="654" alt="image" src="https://github.com/user-attachments/assets/12929a28-9b7b-4f45-bf4d-a71c62cdd4c1" />

Yêu cầu: kích hoạt `alert` khi bấm vào tên tác giả commnent.

Gợi ý: Có lỗi `stored cross-site scripting` ở tính năng comment, các dấu ngoặc nhọn, nháy đơn, nháy kép, backslash đều bị mã hóa.

<img width="2514" height="1443" alt="image" src="https://github.com/user-attachments/assets/8b24ceb7-8e5b-46ca-b7eb-0f28ff1ced51" />

Tôi sẽ thử post 1 comment như này để xem bên phía server sẽ mã hóa các ký tự trên như nào theo đề bài.

<img width="2563" height="186" alt="image" src="https://github.com/user-attachments/assets/84f15826-c949-425e-bea2-b6a74d7a46a2" />

Thấy được rằng các ký tự như nháy kép, ngoặc nhọn thì bị mã hóa thành các HTML entities còn nháy đơn và backslash thì bị escape.

Và ở đây `onclick` đang lấy cái link ta post lên sau khi được mã hóa để cho vào hàm `track`. Mục tiêu của ta ở đây là làm nào để thoát khỏi chuỗi trong hàm `track` này rồi kích hoạt `alert`.

Thì ở đây do các ký tự như đã nói ở trên đã bị mã hóa rồi nên khi ta truyền trực tiếp nháy đơn vào hay có thêm backslash cho nháy đơn thì đều không khiến ta thoát được khỏi chuỗi. Nhưng được biết
rằng cơ chế hoạt động của thuộc tính là nó sẽ decode các HTML entities trong giá trị được gán vào trước để lưu vào cây DOM, sau đó tùy thuộc vào loại thuộc tính thì nó sẽ render hay đẩy code đó cho 
JS engine.

Vậy thì tôi thay vì đóng chuỗi bằng cách trực tiếp truyền vào dấu nháy đơn để bị escape thì tôi sẽ truyền vào là giá trị HTML của nó là `&apos;` (hoặc `&#39;`).

Payload:

```html
https:a.com/&apos;-alert(1)-&apos;
```

Lúc này nó khi qua backend xử lí nó sẽ không thấy bất kỳ ký tự nào đã bị mã hóa ở trên và cho qua và nó sẽ hiển thị link ở trong `onclick` như sau:

<img width="2466" height="96" alt="image" src="https://github.com/user-attachments/assets/aa2df037-ead1-4b8e-930e-eb01dafe3da4" />

```html
<a id="author" href="https:a.com/&apos;-alert(1)-&apos;" onclick="var tracker={track(){}};tracker.track('https:a.com/&apos;-alert(1)-&apos;');">a</a>
```

Ngay khi trình duyệt tải trang và phân tích cú pháp HTML, nó đã tự động decode các HTML entities trong thuộc tính này và lưu vào cây DOM. Tuy nhiên, cần lưu ý: mặc dù HTML entity đã được decode thành ký tự gốc (như dấu nháy đơn), nó chỉ có thể đóng/mở chuỗi đối với các thuộc tính sự kiện (event handlers). Còn với các thuộc tính HTML thông thường, trình duyệt chỉ coi đó là một ký tự dữ liệu đơn thuần, hoàn toàn không có khả năng phá vỡ cấu trúc thẻ. Do đó, khi người dùng kích hoạt thuộc tính `onclick` (bằng cách click chuột), trình duyệt sẽ lấy chuỗi đã được giải mã sẵn trong DOM đẩy cho JS engine thực thi:

```js
var tracker={track(){}};tracker.track('https:a.com/'-alert(1)-'');
```

Lúc này, ta đã ép JS engine phải thực hiện một biểu thức toán học (phép trừ). Để hoàn thành phép tính đó, JS bắt buộc phải lấy được giá trị trả về của `alert(1)`, và thế là hàm `alert` được kích hoạt
thành công.

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/3dc5288b-6b56-4c67-8242-28e4a3f0bae1" />

<img width="2499" height="1443" alt="image" src="https://github.com/user-attachments/assets/e3649360-efda-4ef2-bfa4-c64596f6a723" />
