<img width="1518" height="514" alt="image" src="https://github.com/user-attachments/assets/28f21be8-380e-421b-9d2a-f523bcb0e69d" />

Yêu cầu: gọi 1 hàm alert().

Gợi ý: Lab này chứa lỗ hổng reflected cross-site scripting trong phần comment.

Ở đây tôi có thử bằng tag `h1` và post bình thường thì nhìn bằng mắt thường cũng thấy sự khác biệt giữa 2 comment

<img width="1327" height="452" alt="image" src="https://github.com/user-attachments/assets/2980268a-a5d0-45cc-b788-e0b219860bbd" />

vào soucre code để thấy rõ hơn.

<img width="1732" height="355" alt="image" src="https://github.com/user-attachments/assets/186109c7-08c0-49bc-bd69-b504346d771b" />

Ở đây tag `h1` được đối xử như 1 tag chứ không phải 1 string, vậy ở đây ta đã thành công thực hiện HTML injection. Vậy ở đây ta sẽ thử chèn 1 script javascript để nó hiện alert xem.

<img width="2508" height="1443" alt="image" src="https://github.com/user-attachments/assets/45171ed1-a89b-425a-a8cf-18cde7bb17dc" />

Ở đây tôi đã thành công aler(1).

<img width="1927" height="178" alt="image" src="https://github.com/user-attachments/assets/c3ef1fa0-682f-45e5-9b20-482baaf41a50" />

<img width="2508" height="1443" alt="image" src="https://github.com/user-attachments/assets/edc22224-aca8-460f-b917-fa410c05d943" />
