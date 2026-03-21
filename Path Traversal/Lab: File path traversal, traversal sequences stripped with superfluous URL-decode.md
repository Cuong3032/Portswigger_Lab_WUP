<img width="1231" height="667" alt="image" src="https://github.com/user-attachments/assets/322d4943-7b55-4b4f-9007-7c8a808afaf7" />

hãy mở một hình bất kỳ trong trang này sang tab mới

Lab này có gợi ý rằng `The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it.` trang này đã chặn các đầu vào chứa
các chuỗi truyền tải đường dẫn vậy thì không thể truyền tải các chuỗi như `../../../` hoặc `....//....//....//` được nhưng vế sau lại gợi ý rằng nó sẽ decode URL đầu vào trước khi dùng
tới nó vậy hãy biến các chuỗi `../../../` thành 1 đoạn URL code `%2E%2E%2F%2E%2E%2F%2E%2E%2F` sau đó tiếp tục encode thêm 1 lần nữa vì bình thường các trang web sẽ có cơ
chế tự động giải mã lần 1 bằng chứng là khi thử truyền đoạn URL encode lần 1 thì vẫn chưa thành công 

<img width="1221" height="241" alt="image" src="https://github.com/user-attachments/assets/0dab2b30-1569-4e33-9532-ebcbf60ea4b8" />

nên sau khi encode lần 2 ta được 1 đoạn URL code `%252E%252E%252F%252E%252E%252F%252E%252E%252F` sau đó thử truyền đoạn này `%252E%252E%252F%252E%252E%252F%252E%252E%252Fetc/passwd`
vào

<img width="1246" height="495" alt="image" src="https://github.com/user-attachments/assets/07d0e2b5-429d-44d7-baaf-e48823d6a075" />

<img width="1269" height="615" alt="image" src="https://github.com/user-attachments/assets/f4f68587-90af-4102-a31f-432493e0104d" />

thế là thành công lab này


