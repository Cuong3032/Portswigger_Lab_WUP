<img width="1493" height="662" alt="image" src="https://github.com/user-attachments/assets/47553a30-46ce-4fe0-b205-1f2605f167ba" />

Yêu cầu: kích hoạt `alert` bên trong template string.

Gợi ý: Có lỗi reflected  cross-site scripting ở tính năng search, các dấu ngoặc nhọn, nháy đơn, nháy kép, đều bị HTML encode còn các dấu backtick đã bị escape.

<img width="2511" height="1443" alt="image" src="https://github.com/user-attachments/assets/0a1538ae-ef69-4c87-bb29-45a94162f2d2" />

Ở đây tôi đã thử nhập vào các ký mà phía đề bài đã bảo là đã bị mã hóa thì các giá trị đã thành dạng Unicode escape sequence.

<img width="2506" height="1443" alt="image" src="https://github.com/user-attachments/assets/910be81d-bd06-4b0d-80bf-4f2a2fe593bf" />

Thì ở đây tôi đã thử với chữ cái và các ký tự đặc biệt khác thì chùng vẫn bình thường chỉ có các ký tự trên mới bị mã hóa thành như vậy.

<img width="2503" height="1443" alt="image" src="https://github.com/user-attachments/assets/279808e6-deae-4ec1-9da8-4144a201e64f" />

Và với việc mã hóa như vậy thì tôi không thể đóng chuỗi đó được. Nhưng ở đây họ lại đang gán biến message bằng cặp dấu backtick thì đây được gọi là `template literal`. Bên trong `template literal`
tôi có thể nhúng và thực thi trực tiếp 1 đoạn Javascript thông qua `${}`(String Interpolation (nội suy chuỗi)).

Và bây giờ tôi sẽ kiểm tra lại xem các ký tự `${}` có bị lọc và mã hóa thành dạng Unicode escape sequence không.

<img width="2513" height="1443" alt="image" src="https://github.com/user-attachments/assets/3eb3ab7a-04a2-4acd-994e-1561483e1b14" />

Và có thể thấy các ký tự `${}` không bị mã hóa, thì giờ tôi sẽ nhúng trực tiếp hàm `alert` vào bên trong.

```js
${alert(1)}
```

<img width="2496" height="1443" alt="image" src="https://github.com/user-attachments/assets/4e177f67-0265-4c52-b1ed-50413235b524" />

<img width="2491" height="1443" alt="image" src="https://github.com/user-attachments/assets/946e7ec4-cea8-43db-96f2-03e0e740c5b7" />
