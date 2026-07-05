<img width="1582" height="580" alt="image" src="https://github.com/user-attachments/assets/55dbf595-c345-4296-bc59-39fb4474425d" />

Yêu cầu: Gọi hàm `alert`

Gợi ý: Lab này có lỗi `reflected XSS` nhưng site đã chặn hầu hết các tag thường thấy nhưng vẫn có thể dùng các thẻ `svg` và event.

<img width="2509" height="1443" alt="image" src="https://github.com/user-attachments/assets/e9418008-8856-4ec9-9288-f511885afaaa" />

Dựa vào gợi ý tôi sẽ thử dùng tag `svg`

<img width="2496" height="1443" alt="image" src="https://github.com/user-attachments/assets/72f39d61-823d-4106-8ff3-61ec1a4834fb" />

<img width="648" height="131" alt="image" src="https://github.com/user-attachments/assets/56ebb7cf-fed3-4f38-b515-60dd1f4f7fb1" />

Có thể thấy tag này đã được xử lí.

Tiếp theo tôi sẽ fuzzing để xem event nào dùng được trong lab này bằng `Intruder` của `BurpSuite`.

<img width="2559" height="1443" alt="image" src="https://github.com/user-attachments/assets/4e6d9626-a593-404b-bd94-05e2eda60bdd" />

Ở đây tôi phát hiện ra chỉ dùng được event `onbegin`. Vì onbegin là sự kiện đặc thù chỉ tự động kích hoạt bên trong các thẻ con xử lý ảnh động của `<svg>`, nên phạm vi payload được thu hẹp lại trong 
các thẻ: `<animate>`, `<animateTransform>`, `<animateMotion>` hoặc `<set>`.

Vậy payload sẽ trông như này

```html
<svg><animate onbegin=alert(1) attributeName=x>
```

Ở đây, việc bổ sung `attributeName=x` là bắt buộc. Nguyên nhân là do bản chất của các thẻ animation kể trên là dùng để làm thay đổi một thuộc tính cụ thể nào đó. Nếu thiếu `attributeName`, trình duyệt 
sẽ đánh giá đây là một thẻ lỗi cú pháp và từ chối khởi tạo hiệu ứng, dẫn đến việc sự kiện `onbegin` không bao giờ được kích hoạt.

Thì sau khi thử cả 4 tag con kể trên tôi phát hiện ra chỉ có tag `<animateTransform>` là nằm trong `whitelist`.

--> Payload:

```html
<svg><animateTransform onbegin=alert(1) attributeName=x>
```

<img width="2508" height="1443" alt="image" src="https://github.com/user-attachments/assets/2da0a341-5c1d-4963-ad8a-124d58cb6338" />

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/488991c6-2fcc-4613-9b76-518c6dcee9fa" />
