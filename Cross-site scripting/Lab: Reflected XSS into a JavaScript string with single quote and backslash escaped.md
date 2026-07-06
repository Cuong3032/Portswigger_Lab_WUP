<img width="1511" height="652" alt="image" src="https://github.com/user-attachments/assets/964bb5d0-d847-4c6b-9b9a-622d58998229" />

Yêu cầu: Thực hiện `cross-site scripting attack` thoát khỏi Javascript string và gọi hàm `alert()`.

Gợi ý: Lab chứa lỗi `reflected cross-site scripting` ở tính năng theo dõi query search. Reflect xảy ra bên trong Javascript string và nháy đơn và backslash đã bị escape.

<img width="2495" height="1443" alt="image" src="https://github.com/user-attachments/assets/7d0dd722-87e3-47d2-a22a-f21cb24a0223" />

Theo đề bài thì 2 ký tự `'` và `\` đã bị escape bằng chứng có thể nhìn ở trên hình khi tôi nhập vào cả 2 ký tự thì backslash thừa thêm 1 ký tự và `'` thì được in ra bình thường. Để nhìn rõ hơn ta sẽ
xem source code.

<img width="2498" height="1443" alt="image" src="https://github.com/user-attachments/assets/75523c1e-8a07-4d86-bfd2-64214032441a" />

Có thể thấy rõ ở biến `searchTerms`, vậy nếu không thể thoát khỏi chuỗi thì làm sao để bypass đây. Thì ở đây đề bài chỉ nói là đã escape nháy đơn và backslash không có nói đến các ký tự khác.

Vậy ở đây tôi đặt ra giả thuyết là nếu như tôi có thể truyền 1 thẻ HTML không bị mã hóa/lọc vào `searchTerms` thì sao? Tôi sẽ có thể đóng được cái thẻ `script` này và khởi tạo 1 thẻ mới giúp tôi gọi 
hàm `alert()`. Và yêu cầu đề bài cũng đã nhắc nhở ta phải Thoát khỏi ngữ cảnh thực thi của JavaScript. Nên tôi sẽ thử nhập vào closing tag của `script` để đóng cái khối lệnh JavaScript hiện tại thử xem.

<img width="2504" height="1443" alt="image" src="https://github.com/user-attachments/assets/6b7bd76b-825a-48aa-b63b-8d6adacdb91f" />

<img width="2506" height="1443" alt="image" src="https://github.com/user-attachments/assets/42318efd-f4f5-4d32-b301-c5a8e334ce07" />

Có thể thấy ở đây thẻ `script` đã bị đóng, vậy tôi sẽ thêm 1 thẻ mới nữa để kích hoạt `alert()`

Payload:

```html
</script><script>alert(1)</script>
```

<img width="2497" height="1443" alt="image" src="https://github.com/user-attachments/assets/7d318566-ef36-4cac-9908-677277425d78" />

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/34675fb2-dc91-4529-a406-29305392421c" />
