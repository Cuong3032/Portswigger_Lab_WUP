<img width="1487" height="490" alt="image" src="https://github.com/user-attachments/assets/97f0b8e0-04fe-4858-b856-69bdc54d1185" />

Yêu cầu:  Gọi hàm `alert()`

Gợi ý: lỗi `stored DOM vulnerability` có trong tính năng comment blog.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/85c46d22-a99e-4814-a402-1ac2e9245d2a" />

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/f192d68b-8ef8-47c3-a5a6-cd8298f61f7a" />

Nhìn vào đây thì thay vì render (kết xuất) trực tiếp kết quả tìm kiếm vào thẳng mã nguồn HTML ban đầu, lập trình viên đã thiết kế trang web tải dữ liệu động thông qua cơ chế AJAX bằng cách gọi hàm 
`loadComments()`.

Ở phía dưới là toàn bộ đoạn code của `loadCommentsWithVulnerableEscapeHtml.js` bao gồm `loadComments()`, `escapeHTML()` và `displayComments()`.

```js
function loadComments(postCommentPath) {
    let xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            let comments = JSON.parse(this.responseText);
            displayComments(comments);
        }
    };
    xhr.open("GET", postCommentPath + window.location.search);
    xhr.send();

    function escapeHTML(html) {
        return html.replace('<', '&lt;').replace('>', '&gt;');
    }

    function displayComments(comments) {
        let userComments = document.getElementById("user-comments");

        for (let i = 0; i < comments.length; ++i)
        {
            comment = comments[i];
            let commentSection = document.createElement("section");
            commentSection.setAttribute("class", "comment");

            let firstPElement = document.createElement("p");

            let avatarImgElement = document.createElement("img");
            avatarImgElement.setAttribute("class", "avatar");
            avatarImgElement.setAttribute("src", comment.avatar ? escapeHTML(comment.avatar) : "/resources/images/avatarDefault.svg");

            if (comment.author) {
                if (comment.website) {
                    let websiteElement = document.createElement("a");
                    websiteElement.setAttribute("id", "author");
                    websiteElement.setAttribute("href", comment.website);
                    firstPElement.appendChild(websiteElement)
                }

                let newInnerHtml = firstPElement.innerHTML + escapeHTML(comment.author)
                firstPElement.innerHTML = newInnerHtml
            }

            if (comment.date) {
                let dateObj = new Date(comment.date)
                let month = '' + (dateObj.getMonth() + 1);
                let day = '' + dateObj.getDate();
                let year = dateObj.getFullYear();

                if (month.length < 2)
                    month = '0' + month;
                if (day.length < 2)
                    day = '0' + day;

                dateStr = [day, month, year].join('-');

                let newInnerHtml = firstPElement.innerHTML + " | " + dateStr
                firstPElement.innerHTML = newInnerHtml
            }

            firstPElement.appendChild(avatarImgElement);

            commentSection.appendChild(firstPElement);

            if (comment.body) {
                let commentBodyPElement = document.createElement("p");
                commentBodyPElement.innerHTML = escapeHTML(comment.body);

                commentSection.appendChild(commentBodyPElement);
            }
            commentSection.appendChild(document.createElement("p"));

            userComments.appendChild(commentSection);
        }
    }
};

```

Hàm `loadComments` đang sử dụng tới các dữ liệu ta nhập vào thông qua endpoint `/post/comment` và với biến lấy được trên URL, kết quả trả về là `json data` sau đó được cho vào `JSON.parse` để biến nó
thành các Object.

Tiếp đến là `displayComments` sẽ dùng tới 2 thứ là dữ liệu ta nhập vào sau khi qua `JSON.parse` và hàm `escapeHTML`.

Thì với hàm `escapeHTML` đang dùng hàm replace để thay thế `< >` thành `&lt;` và `&gt;` nhưng với hàm này ta sễ chỉ thay thế ký tự đầu tiên nó gặp.

Và với dòng này:

```js
commentBodyPElement.innerHTML = escapeHTML(comment.body);
```

Dòng này chính là điểm kích hoạt lỗ hổng (Sink). Thuộc tính `innerHTML` sẽ biên dịch chuỗi được truyền vào thành HTML thực sự bên trong thẻ `<p>`. Do hàm `escapeHTML` sử dụng `replace()` mà không có 
cờ `global`, nó chỉ encode được cặp dấu `<>` đầu tiên. Lợi dụng điều này, ta chèn payload `<><img src=x onerror=alert(origin)>.` Khi đó, cặp ngoặc rác `<>` ở đầu sẽ biến thành `&lt;&gt;`, đóng vai trò 
làm vật tế thần (bypass filter), giúp cho đoạn mã `<img src=x onerror=alert(origin)>` phía sau giữ nguyên hình hài, được `innerHTML` render thành thẻ ảnh thật và thực thi JavaScript thông qua sự kiện 
`onerror`.

Payload:
```HTML
<><img src=x onerror=alert(origin)>
```

Ta chỉ cần post comment giá trị trên thì sau khi qua `JSON.parse` nó sẽ được giữ nguyên rồi khi qua `escapeHTML` thì 2 ký tự đầu sẽ thành `&lt;&gt;` nhưng các ký tự phía sau không ảnh hưởng khiến cho 
tag `img` vẫn được render bình thường dẫn tới `alert`.

<img width="2499" height="1443" alt="image" src="https://github.com/user-attachments/assets/111eda25-e47d-47ca-a0be-490cc0fa94c8" />

<img width="2506" height="1443" alt="image" src="https://github.com/user-attachments/assets/03ce947c-5713-42cc-a5d4-2f2fa27dd6b1" />
