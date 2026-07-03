<img width="1509" height="612" alt="image" src="https://github.com/user-attachments/assets/53d5b1c1-a925-4014-ac48-d20120807853" />

Yêu cầu: hãy thực hiện một cuộc tấn công XSS phá vỡ (thoát khỏi) chuỗi JavaScript đó và gọi hàm `alert`

Gợi ý: Bài lab này chứa một lỗ hổng cross-site scripting (XSS) phản xạ trong chức năng theo dõi truy vấn tìm kiếm, nơi mà các dấu ngoặc nhọn (< và >) đã bị mã hóa. Sự phản xạ dữ liệu xảy ra bên 
trong một chuỗi JavaScript.

Tôi bắt đầu bằng cách thử nhập vào 1 tag `h1` xem có đúng là nó đã bị mã hóa hay không?

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/93e86421-7a94-401c-bc40-dbdf19bbbc90" />

<img width="2510" height="1443" alt="image" src="https://github.com/user-attachments/assets/c7ce63f0-be19-4f9f-bcca-03c7a1fa9642" />

Nhìn vào hình thì đúng thật nó đã bị mã hòa. Nhưng nhìn kỹ vào đoạn này:

```html
                   <section class=blog-header>
                        <h1>0 search results for '&lt;h1&gt;a&lt;/h1&gt;'</h1>
                        <hr>
                    </section>
                    <section class=search>
                        <form action=/ method=GET>
                            <input type=text placeholder='Search the blog...' name=search>
                            <button type=submit class=button>Search</button>
                        </form>
                    </section>
                    <script>
                        var searchTerms = '&lt;h1&gt;a&lt;/h1&gt;';
                        document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
                    </script>
```

Thì ở đây ta phát hiện giá trị ta nhập được chèn vào biến `searchTerm` đước dùng cho attribute `src` của tag `img` và ở đây đang sử dụng tới `document.write`  để viết dữ liệu ra ngoài page. Vậy mặc dù
ta không thể chèn các ký tự như `< >` nhưng ta có thể chèn thêm 1 attribute mới.

Chú ý rằng ở đây nó đang encode URL giá trị `searchTerms` thế thì tôi không thể dùng được ký tự `"` vì nó đã bị mã hóa rồi sau khi mã hóa xong thì nó lại không thể đóng chuỗi dẫn tới nó vẫn chỉ là 1
chuỗi bình thường.

Nhưng tôi lại chú ý tới 1 điểm khác đó là bước khai báo biến, khi khai báo thì nó nhận thuần dữ liệu do tôi nhập vào, vậy tôi có thể thoát khỏi bước đặt biến này rồi chạy alert luôn không?

Payload:

```javascript
'-alert(origin)-'
```

Thì lúc này câu lệnh khai báo biến từ việc gán 1 chuỗi thành 1 phép toán trừ.

```javascript
var searchTerms = '' - alert(origin) - ''
```

Và để thực hiện được phép trừ thì nó bắt buộc phải chạy alert để lấy kết quả và thế là ta đã thành công bật được `alert()`.

<img width="2491" height="1443" alt="image" src="https://github.com/user-attachments/assets/fb71307b-09d7-4321-a401-e034afae8da8" />

<img width="2503" height="1049" alt="image" src="https://github.com/user-attachments/assets/b9a253a8-6b62-43de-a99d-9db990930f4c" />
