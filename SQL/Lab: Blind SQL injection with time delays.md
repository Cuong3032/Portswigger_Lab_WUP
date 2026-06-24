<img width="1550" height="791" alt="image" src="https://github.com/user-attachments/assets/89260aeb-d2c2-493c-92c4-f28c2f10cea0" />

Yêu cầu: làm server delay 10s

Gợi ý: có thể thực thi được SQL query ở Tracking Cookie, kết quả query sẽ không được trả về và server cũng sẽ không thay đổi gì dù query có trả về bất kỳ row nào hoặc là gây ra lỗi gì đó.

Vậy nếu biết là tracking cookie có thể thực thi được query rồi vậy thì ta có thể làm nó bị delay để xác định chính xác nó có thực thi được query hay không. 

Thì sau khi thử từng query của từng loại database thì ta thấy query này của PostgreSQL đã thành công khiến sever delay 10s:

    '||(SELECT pg_sleep(10))||'

<img width="2061" height="1349" alt="image" src="https://github.com/user-attachments/assets/267ada53-d97b-4d09-8469-35a7f2f10801" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/2afd2e8c-db38-4039-86fe-a8235c353eb1" />
