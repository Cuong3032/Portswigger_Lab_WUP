<img width="1513" height="468" alt="image" src="https://github.com/user-attachments/assets/9853e78b-7ea1-4237-a819-d1b7f44a80b3" />

Yêu cầu: gọi hàm `alert` khi click vào tên của chủ comment.

Gợi ý: Chứa lỗi `stored cross-site scripting` trong tính năng comment.

<img width="2507" height="1443" alt="image" src="https://github.com/user-attachments/assets/da3f23da-ff68-47b1-8428-b94254189409" />

Ở trên tôi đã thử post 1 comment vào 1 bài post bất kỳ và tôi đã bấm thử vào tên của tôi sau khi post comment thì nó đi tới website tôi vừa post kèm theo

<img width="2497" height="463" alt="image" src="https://github.com/user-attachments/assets/fa8b25f5-1469-4d80-9d62-2ff26a9c7d89" />

<img width="2540" height="1443" alt="image" src="https://github.com/user-attachments/assets/c53876ee-2a41-465c-8e82-51f7f21daaa9" />

Nhìn vào trên thì tên của người vừa comment được gắn với 1 attribute là `href`, cái giá trị này chính là phần website tôi vừa nhập vào. 

Tôi sẽ thực hiện bằng cách đóng lại giá trị của `href` sau đó chèn thêm 1 attribute mới như onclick rồi cho nó bật alert sau khi tôi click vào tên của chủ comment.

Payload:
```html
" onclick="alert(origin)
```

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/27104d50-fd29-4ee9-ba7f-c40c256658cc" />

Thì ở đây tôi phát hiện ra rằng ký tự `"` đã bị mã hóa lại do đó tôi không thể thoát khỏi giá trị của attribute `href`. Thì ở đây tôi nghĩ tới dùng `Pseudo-Protocol`, tôi sẽ dùng giao thức giả của
javascript để thay vì redirect tới 1 trang bên ngoài nó sẽ thực thi câu lệnh javascript.

Payload:
```
javascript:alert(origin)
```

<img width="1434" height="998" alt="image" src="https://github.com/user-attachments/assets/47a5f50b-40fc-4b22-af22-2bd0384f15bf" />

<img width="2497" height="1443" alt="image" src="https://github.com/user-attachments/assets/b745a8e7-48e1-4820-ab93-0b61699f893f" />

