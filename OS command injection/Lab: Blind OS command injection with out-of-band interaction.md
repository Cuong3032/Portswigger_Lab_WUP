<img width="1547" height="1120" alt="image" src="https://github.com/user-attachments/assets/b3e10b76-7d37-4b93-8b5e-38b531e8b7dd" />

Yêu cầu: thực hiện truy vấn DNS lookup tới Burp Collaborator.
Gợi ý: có thể thực thi shell command dựa vào đầu vào của user, và không có tác dụng gì tới respone, không thể chuyển hướng tới vị trí có thể truy cập nhưng có thể redirect ra một miền ngoài.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/519b2225-c9f9-42dc-bf5d-fd46888110a7" />

Ở đây đầu vào của user chỉ có mỗi phần `submit feedback` nên ta sẽ tập trung vào đây

<img width="2561" height="1443" alt="image" src="https://github.com/user-attachments/assets/a5d73fec-3349-48f8-a118-8ff7897d2340" />

Ở đây ta phát hiện ra rằng ta thử lệnh `sleep` với từng trường nhưng đều không thành công vì không có respone nào bị delay và ta nhớ tới gợi ý rằng mặc dù không có ảnh hưởng hay tác dụng gì tới respone
nhưng command vẫn được thực thi và ta có thể redirect output ra miền ngoài. Vậy ta sẽ chèn thêm hết vào cả 4 trường payload như sau:

    nslookup + location của burp collaborator

<img width="2563" height="1271" alt="image" src="https://github.com/user-attachments/assets/e6385a1b-b6df-4815-a02d-bc7c916d2d62" />

<img width="2563" height="950" alt="image" src="https://github.com/user-attachments/assets/583f859a-49d7-4193-a8b9-acf66fc849f4" />

Và có thể thấy bên Burp Collaborator đã nhận được 2 cái dns lookup

<img width="2562" height="1443" alt="image" src="https://github.com/user-attachments/assets/20633ef6-74ab-4770-ba77-cb109484279b" />
