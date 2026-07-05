<img width="2495" height="1035" alt="image" src="https://github.com/user-attachments/assets/163c4a36-e3c6-4019-85b4-b30b7929a076" /><img width="1482" height="617" alt="image" src="https://github.com/user-attachments/assets/94b15e3c-8193-406a-9251-eaff3fc5ec2f" />

Yêu cầu: Tự động alert `document.cookie`.

Gợi ý: Lab này đã chặn hết các tag trừ tag `custom`.

<img width="2497" height="1443" alt="image" src="https://github.com/user-attachments/assets/ff041827-8f90-4d7f-9906-aa61a14e9bd1" />

Dựa vào gợi ý của bài lab này thì tôi không thể sử dụng các tag bình thường của HTML mà phải tự custom 1 tag thì ở đây tôi sẽ thử chèn 1 tag do tôi tạo ra và 1 tag có sẵn của HTML.

<img width="1789" height="363" alt="image" src="https://github.com/user-attachments/assets/da5f1414-08ef-4b4f-a0f4-a4b3e5f3cd40" />

Như trên thì tag `body` cũng đã bị chặn

<img width="2495" height="1035" alt="image" src="https://github.com/user-attachments/assets/20c31684-58c1-47dc-ac50-6f6b61679614" />

Nhưng tag `abc` của tôi vẫn qua dễ dàng và nó được chèn vào bên trong tag `h1` để hiển thị kết quả ra màn hình

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/a6fa53e5-1641-4146-b3fa-2096b68eae43" />

Vậy nếu đã dùng được tag custom thì tiếp theo tôi sẽ thêm các event handler để nó có thể tự động alert 1 cái thông báo bất kỳ.

Ở đây tôi dùng tới `onfocus` cho phép khi vào trạng thái `focus` thì nó sẽ kích hoạt thuộc tính này và để kết hợp với thuộc tính này để nó tự động alert thì tôi dùng thêm thuộc tính `autofocus` cho
phép trang sau khi load xong sẽ tự động `focus` tới thẻ này. 

```html
<abc autofocus onfocus=alert(1)>
```

Nhưng có 1 vấn đề là thẻ custom là `Non-interactive Elements` chỉ có thể dùng để nhìn chứ không thể tương tác dẫn tới là không thể `focus` thì khi load lại trang thuộc tính `autofocus` sẽ tự động 
bỏ qua. Vậy giải pháp ở đây là tôi dùng `tabindex` để nó cho phép thẻ này có thể được tab tới dẫn tới là nó có thể `focus`.

```html
<abc autofocus tabindex=0 onfocus=alert(1)>
```

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/9d6fe2e0-2983-4904-99cf-28fbefb1230f" />

Sau khi gửi payload thì trang đã tự động hiển thị ra thông báo.

Tiếp theo là bước gửi cho victim.

Payload:
```html
<script>
location='https://0af1006c040228f4804126e100890015.web-security-academy.net/?search=%3Cabc+autofocus+tabindex%3D0+onfocus%3Dalert%28document.cookie%29%3E';
</script>
```

<img width="2497" height="1443" alt="image" src="https://github.com/user-attachments/assets/413361f9-29f7-4c22-bc6d-d960d106627d" />
