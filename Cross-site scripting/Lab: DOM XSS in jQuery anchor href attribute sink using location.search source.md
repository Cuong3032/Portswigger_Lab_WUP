<img width="1514" height="574" alt="image" src="https://github.com/user-attachments/assets/1d87e008-9d2f-4416-a6b1-f04139fa35cc" />

Yêu cầu: Gọi hàm `alert(document.cookie)` thông qua liên kết "back".

Gợi ý: Lab này chứa lỗ hổng DOM-based cross-site scripting trong trang submit feedback. Nó sử dụng hàm selector `$` của thư viện jQuery để tìm một phần tử anchor (thẻ `<a>`), và thay đổi thuộc tính 
`href` của thẻ đó bằng dữ liệu lấy từ `location.search` - nơi mà ta có thể kiểm soát.

<img width="2495" height="1443" alt="image" src="https://github.com/user-attachments/assets/87ac8003-9259-41bb-a4ab-92a52cf71899" />

Đây là `homne` của lab, theo gợi ý tôi sẽ vào trang submit feedback.

<img width="2487" height="1443" alt="image" src="https://github.com/user-attachments/assets/103bc3e6-f702-4346-8b13-c82ede831458" />

Tôi mở source code của trang submit feedback trên phát hiện ra hàm `selector` của thư viện JQuery đang được sử dụng để ghi thêm 1 attribute `href` vào trong thẻ có id là `backLink`, và nó ghi vào 
attribute này giá trị mà ta có thể kiểm soát là biến `returnPath` trên URL.

<img width="1347" height="332" alt="image" src="https://github.com/user-attachments/assets/5180aec6-64fb-4467-8de9-ebb0e8a06998" />

Và được biết là ta có thể dùng **Pseudo-protocol (Giao thức giả)** để thực thi javascript thông qua attribute `href`.

Vậy nếu tôi truyền vào `returnPath` giá trị là 

```javascript
javascript:alert(document.cookie)
```

Thì lúc này thẻ có id là `backLink` sẽ thành 

```javascript
<a id="backLink" href="javascript:alert(document.cookie)">Back</a>
```

Khi ta bấm vào function `Back` thì thay vì trở về home nó sẽ bật thông báo về cookie lên.

<img width="2491" height="1443" alt="image" src="https://github.com/user-attachments/assets/96b8bbb3-2ea1-459d-9a08-2c2a8ec05755" />
