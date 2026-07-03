<img width="1606" height="785" alt="image" src="https://github.com/user-attachments/assets/329b9819-de53-4949-b427-e3838cc26ea9" />

Yêu cầu: gọi 1 hàm alert().

Gợi ý: Lab này chứa lỗ hổng DOM-based cross-site scripting trong thanh search. Nó sử dụng document.write để viết dữ liệu ra ngoài page. document.write sử dụng data từ location.search - Nơi mà ta có 
thể kiểm soát. Dữ liệu này được đặt bên trong một phần tử (thẻ) select

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/f3aea167-0bda-4292-8eb5-997441482aea" />

Ở đây ta có thể check giá sẳn phẩm với từng vùng.

<img width="2512" height="1443" alt="image" src="https://github.com/user-attachments/assets/f1487722-be3f-4f68-bd45-97b5fb043f15" />

Ta phát hiện ở đây họ tạo 1 script javascript để tạo 1 cái bảng chọn từng vùng có sử dụng tag `select` thì khoảng giữa tag mở và đóng của `select` này chỉ cho phép 1 số tag hợp lệ như `option`, 
`outgroup`. Vậy mục tiêu ở đây là thoát khỏi tag `select`.

Ở đây ta có thể kiểm soát được giá trị của biến `store` do nó lấy giá trị từ `storeId` mà ta có thể kiểm soát và biến store thì nằm giữa tag `option`.

Ta sẽ chèn thẳng 1 cái tag close `</select>` vào rồi chèn 1 tag mới cho phép thực thi javascript.

Payload:

```
</select><script>alert(origin)</script>
```

<img width="2502" height="1443" alt="image" src="https://github.com/user-attachments/assets/ebb52c65-f425-4cbc-b428-0bec35c4e7a0" />

<img width="2498" height="1443" alt="image" src="https://github.com/user-attachments/assets/a5a679cc-ae49-4958-bfc6-dd35b1d7478b" />

Thế là thành công bật alert lên.
