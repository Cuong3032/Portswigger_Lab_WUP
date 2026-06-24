<img width="1478" height="782" alt="image" src="https://github.com/user-attachments/assets/f82da865-90b5-42db-8420-9f57160fd71c" />

Yêu cầu: login bằng administrator account từ bảng users

Gợi ý: Bài lab này có lỗ hổng SQLi ở phần tracking cookie. Kết quả của lệnh SQL sẽ không được trả về và không có bất kỳ lỗi nào được trả về. 
Nhưng nếu có lỗi thì sẽ trả về 1 cái error message được custom.

Hint: Database sử dụng là Oracle.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/e1a9ec83-7724-47e9-bff2-be85bf696f8e" />

Đây là giao diện của bài lab này, có home, my account, catagories với có thể xem chi tiết sản phẩm trong từng loại sản phẩm.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/56db8a91-c614-40d4-bcaf-3b19a769d924" />

Với tính năng my account thì sau khi nhấp vào sẽ bắt ta phải đăng nhập. Thì nhớ lại ở gợi ý rằng ta có thể thực thi SQL query trong `tracking cookie`. Vậy ta bắt đầu bằng cách thử payload này:

    ' AND '1' = '1

<img width="2560" height="1443" alt="image" src="https://github.com/user-attachments/assets/05ad4a7f-3654-4eb2-a21e-999f62454805" />

Phát hiện ra rằng không có gì biến hóa nhưng ở gợi ý có nói rằng nếu query gặp lỗi sẽ trả về 1 error message được custom nên ta sẽ thử chèn 1 payload bị lỗi xem:

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/3474c650-1533-43fc-8671-74501346321c" />

Ở đây tôi chèn thử 1 payload thừa dấu nháy đơn xem và nó báo về lỗi như trên.

Vậy thì ở đây ta biết được rằng nếu query đúng sẽ không có gì hết nhưng nếu query sai sẽ báo về lỗi `Internal Server Error` vậy thì ta có thể tận dụng lỗi này để khai thác không?

Và ở đây ta sẽ dùng **Conditional Logic** để khai thác và được biết qua HINT là Database sử dụng là Oracle vậy trước hết ta sẽ thử bằng payload này:

    ' AND (SELECT CASE WHEN '1'='1' THEN TO_CHAR(1/0) ELSE NULL END FROM dual)='a

Giải thích sơ qua về query thì khi 1=1 sẽ lấy 1 chia cho 0 gây ra lỗi và sẽ ép kiểu kết quả trả về chuỗi vì oracle khá khắt khe trong việc kiểm soát data type đặc biệt là trong các mệnh đề như 
`CASE WHEN` hay `UNION` cần đồng nhất data type, do đó nếu không ép kiểu về chuỗi thì nó sẽ luôn trả về lỗi `ORA-00932: inconsistent datatypes` ngay cả trước khi kiểm tra điều kiện. Và theo sau 
`1/0` là nếu điều kiện trên sai sẽ trả về null và với `ORACLE` thì mỗi câu `SELECT` đều cần `FROM` tới bảng bất kỳ do đó ta có thể dùng bảng ảo `dual` có 1 cột 1 hàng duy nhất. Bên cạnh đó, do 
Oracle không hỗ trợ kiểu dữ liệu Boolean, ta bắt buộc phải nối subquery bằng toán tử AND và kết thúc bằng phép so sánh ='a'. Việc này nhằm tạo ra một mệnh đề hợp lệ: nếu điều kiện đúng, phép chia 
cho 0 sẽ gây sập truy vấn; ngược lại nếu điều kiện sai, biểu thức trở thành NULL = 'a' (tương đương False), giúp câu lệnh chạy qua an toàn mà không văng lỗi.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/8fd936d7-f477-4d17-b828-b2e7441b6d8a" />

Về mặt ý tưởng đã là đúng nhưng có vẻ như có vấn đề gì đó đã khiến cho đoạn chia `1/0` không được thực thi dẫn tới đoạn query vẫn được thực thi bình thường không bị crash, ở đây tôi sẽ thay bằng substring
để ép nó thực thi toàn bộ đoạn query bên trong.

    ' || (SELECT CASE WHEN '1'='1' THEN TO_CHAR(1/0) ELSE NULL END FROM dual) || '

Như này thì query sẽ được thực thi và ta phải đóng chuỗi và chèn thêm `||` để nối chuỗi với chuỗi ở cuối đã bị đóng, việc này giúp triệt tiêu hoàn toàn nguy cơ văng lỗi cú pháp (Syntax Error) do thừa
dấu nháy đơn hoặc thiếu toán tử kết nối, đảm bảo rằng ứng dụng sẽ chỉ sập do lỗi chia cho không khi điều kiện ta kiểm tra là Đúng.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/4bb83113-91ae-4578-a253-a25a2b669fb1" />

Tiếp theo là thử với điều kiện sai:

    ' || (SELECT CASE WHEN '1'='2' THEN TO_CHAR(1/0) ELSE NULL END FROM dual) || '

<img width="2561" height="1443" alt="image" src="https://github.com/user-attachments/assets/c5fce003-5d7e-453b-98ce-35b2c86dca2b" />

Sau khi thử 2 điều kiện đúng và sai ta thấy nó đều di đúng hướng ta đã dự kiến, vậy ta có thể thay đổi điều kiện để khai thác rồi. Đầu tiên là ta phải check là có bảng tên `users` không?

    ' || (SELECT CASE WHEN (SELECT 'a' FROM users WHERE ROWNUM = 1) = 'a' THEN TO_CHAR(1/0) ELSE NULL END FROM dual) || '

(Payload ở đây phải dùng ROWNUM vì Oracle không có LIMIT)

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/fdfd49cf-d093-405b-b57c-6afcc0ba6038" />

Ở đây sever trả về lỗi do đó có bảng `users`. 

Hoặc là có thể dùng payload như này cho gọn:

    ' || (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE NULL END FROM all_tables WHERE table_name='USERS' AND ROWNUM = 1) || '

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/f72b6f55-a496-46c1-89ad-358949ab16af" />

Ở trên lí do phải dùng all_tables là do đề phòng nếu `FROM` thẳng từ bảng `users` thì server có thể crash do không tìm được bảng chứ không phải là do `1/0` điều này làm ta khó xác định được rằng
sever crash do bảng này có tồn tại hay không tồn tại

Tiếp theo là xác định xem có user `administrator` hay không và ở đây đã biết là có bảng `users` rồi nên không cần from từ all_tables nữa

    ' || (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator') || '

<img width="2559" height="1443" alt="image" src="https://github.com/user-attachments/assets/bdda7a73-7647-49db-ac6b-4b02f4d61118" />

Server trả về lỗi do đó ta xác định là có user là `administrator`.

Tiếp đến là xác định độ dài password:

    ' || (SELECT CASE WHEN LENGTH(password) > 1 THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator') || '

<img width="2561" height="1443" alt="image" src="https://github.com/user-attachments/assets/055f35b2-4413-42f8-b03d-baf374da7d5e" />

Tiếp tục thử so sánh lần lượt từ 1 trở đi thì cho tới khi độ dài bằng 20 thì nó không trả về lỗi nữa có nghĩa là độ dài này không lớn hơn 20 nhưng lớn hơn 19 do đó độ dài password sẽ là 20.

Tiếp theo là ta sẽ dùng Intruder để brute force từng ký tự trong password với payload như sau và config trong tab Intruder là với wordlist từ `a-zA-z0-9` và sẽ dùng khi gặp `Internal Server Error` sẽ
dừng luôn

    ' || (SELECT CASE WHEN SUBSTR(password,1,1) = '$x$' THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator') || '

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/a7f89a8d-39c0-42fc-a7f8-17cf60dc174f" />

Ở đây ta bắt được ký tự đầu tiên là 2

Tiếp tục cho tới ký tự cuối ta được password là : `2c1g7x6un5lubklj3qus`

Ta sẽ đăng nhập bằng password này cho account `administrator`

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/017e691a-c006-43d5-8583-02f35c104a75" />
