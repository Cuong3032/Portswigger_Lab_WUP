<img width="2504" height="1437" alt="image" src="https://github.com/user-attachments/assets/4d4b2779-0f5d-411b-af3b-0f8c3f3f526e" /><img width="1522" height="766" alt="image" src="https://github.com/user-attachments/assets/9208ef36-873c-4784-9a6d-189724319bbc" />

Yêu cầu: gọi 1 hàm `alert()`.

Gợi ý: Lab này chứa lỗ hổng `DOM-based cross-site scripting` trong thanh search. Nó sử dụng `document.write` để viết dữ liệu ra ngoài page. `document.write` sử dụng data từ `location.search` - Nơi mà
ta có thể kiểm soát. 

<img width="2504" height="1437" alt="image" src="https://github.com/user-attachments/assets/ed21a068-9fc8-4526-9a89-50b135afb6ec" />

<img width="1504" height="711" alt="image" src="https://github.com/user-attachments/assets/38a7cd7e-c965-4996-8efe-7966395b7839" />

Sau khi search 1 ký tự thì nó sẽ được in ra trong 1 tag `h1`.

Ở đây bắt đầu bằng việc ta sẽ thử bằng 1 tag của html ví dụ như `<h2>`.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/c14511c7-60d1-4d48-ac0e-818d0c3f70e3" />

Ở đây thấy tag `<h2>` vẫn được in ra bình thường và nội dung bên trong tag thì vẫn không đổi kích cỡ vẫn là kích cỡ của tag `<h1>`.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/8c105533-2e98-49d8-b474-83098877fede" />

Bên trong source code đã chỉ ra rõ lí do, đó là vì các dấu `< >` đã bị mã hóa lại khi html render nó không hiểu `&lt;h2&gt;` là 1 tag `<h2>` mà nó chỉ hiểu thành các ký tự đơn lẻ là `< h2 >` và cứ 
thế nó in ra cả đoạn string `<h2>`.

Nhưng tôi phát hiện ra được ở bên dưới có sử dụng tới 2 hàm của javascript là `document.write` và `location.search`.

```html
                    <script>
                        function trackSearch(query) {
                            document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
                        }
                        var query = (new URLSearchParams(window.location.search)).get('search');
                        if(query) {
                            trackSearch(query);
                        }
                    </script>
```
Ở hàm `location.search` lấy nội dung biến, giả sử có 1 url như sau :

```url
https://0aff00c0038ad40181972b580054000d.web-security-academy.net/?search=%3Ch2%3Ea%3C%2Fh2%3E
```

thì nó sẽ lấy phần này ra `?search=%3Ch2%3Ea%3C%2Fh2%3E`, tiếp đến thì khởi tạo 1 đối tượng `URLSearchParams` dùng data là phần dữ liệu của `location.search` tiếp là nó gọi tới hàm `get()` của
`URLSearchParams` để lấy dữ liệu và URL decode cái giá trị của biến `search` ra.


sau đó thì ở hàm `document.write` sẽ lấy cái giá trị mà hàm `get()` của `URLSearchParams` trả về thay thế cho `query` bên trong chuỗi dưới:

```html
<img src="/resources/images/tracker.gif?searchTerms='+query+'">
```

Và thế là ta được nội dung bên trong `document.write` là:

```html
<img src="/resources/images/tracker.gif?searchTerms='<h2>a</h2>'">
```

Mà nhìn ở trên payload của ta vẫn chỉ là 1 phần của giá trị chuỗi được gán cho thuộc tính `src` của tag open `img` vậy trước hết ta phải thoát được cái việc bị gán chuỗi đó trước
đã và phải thoát luôn khỏi tag open của `img` để có thể chèn thêm payload vì ở thẻ mở thì Chỉ dành cho các thuộc tính, còn phần giữa thẻ mở và đóng mới là phần mà ta có thể chèn thêm 1 thẻ mới vào.

vậy giờ ta sẽ thử chèn 1 payload như này vào:

```html
"><h2>abcxyz</h2>
```

<img width="2490" height="1443" alt="image" src="https://github.com/user-attachments/assets/094e16f9-c24d-432d-bb44-d3a285a27839" />

ở đây ta có thể thấy nội dung của thẻ `<h2>` đã được in ra rồi và nó còn in phần thừa ra là phần `">` do nãy tôi đã đóng chuỗi và thẻ dẫn tới đoạn đằng sau payload bị coi là thừa thãi và đối xử như 
Text Node (Nút văn bản thuần túy) và in ra màn hình.

Vậy thì ở đây tôi đã thành công HTML injection. Tiếp theo tôi sẽ thử chèn 1 script javascript để nó bật alert lên xem.

```html
"><script>alert(origin)</script>
```

<img width="2523" height="1443" alt="image" src="https://github.com/user-attachments/assets/d3fe822b-9d95-43ee-979c-df9beb63ee47" />

nó đã thành công bật lên 1 thông báo về origin của trang web này.

<img width="2504" height="1443" alt="image" src="https://github.com/user-attachments/assets/d48fe870-453c-4597-af23-9da97a397211" />
