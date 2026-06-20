<img width="1515" height="936" alt="image" src="https://github.com/user-attachments/assets/aca53e11-d4f3-4d30-b4a3-10f50ba4057b" />

Yêu cầu: trích xuất output của `whoami` ra bên ngoài.

Gợi ý: có thể thực thi shell command dựa vào đầu vào của user, và không có tác dụng gì tới respone, không thể chuyển hướng tới vị trí có thể truy cập nhưng có thể redirect ra một miền ngoài.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/6a076d3d-422a-43f6-b059-b0109178b173" />

Ở đây đầu vào của user chỉ có mỗi phần submit feedback nên ta sẽ tập trung vào đây

<img width="2560" height="1443" alt="image" src="https://github.com/user-attachments/assets/2e3f46e1-d2bb-402a-bf6c-19a869565669" />

Ở đây ta phát hiện ra rằng ta thử lệnh sleep với từng trường nhưng đều không thành công vì không có respone nào bị delay. 

<img width="2034" height="786" alt="image" src="https://github.com/user-attachments/assets/49ed89e2-4964-4ac1-8ef2-8a0bdaefcace" />

Ta nhớ tới gợi ý rằng mặc dù không có ảnh hưởng hay tác dụng gì tới respone nhưng command vẫn được thực thi và ta có thể redirect output ra miền ngoài. 
Vậy ta sẽ chèn thêm hết vào cả 4 trường payload như sau:

<img width="2060" height="957" alt="image" src="https://github.com/user-attachments/assets/e1b2d642-681d-4ad3-940a-cf6869820b03" />

Thì ở đây do name không được quá 64 ký tự nên ta tạm thời với 3 trường còn lại trước.

<img width="2051" height="1123" alt="image" src="https://github.com/user-attachments/assets/b414db3c-0cfc-40f2-b0a6-35e9570e21e0" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/543a931c-867b-4449-9aa8-d0e8881fb13d" />

Ở đây ta đã thành công trích xuất thành công current user mà không cần tới trường name.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/0f22e6b7-726e-429a-83b4-b3ef92353312" />
