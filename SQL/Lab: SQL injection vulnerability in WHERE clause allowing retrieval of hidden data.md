Đầu tiên ta có 1 cái URL là `https://0a10005c03e4386980634f1a00e90096.web-security-academy.net/` thì ta biết được là khi user chọn 1 sản phẩm ứng dụng sẽ thực hiện 1 câu truy vấn là `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

Câu truy vấn này có nghĩa là chọn tất cả sản phẩm loại 'Gifts' và 'released = 1' ở đây có nghĩa là các sản phẩm đã được phát hành

Vì thế để có thể in ra các sản phẩm chưa được phát hành ta có thể sử dung `--` để comment lại đoạn 'released = 1' để nó in ra tất cả các sản phẩm bao gồm cả sản phẩm chưa phát hành như này

`https://insecure-website.com/products?category=Gifts'--`

Sau đó ta có thể nâng cấp lên khi chèn thêm lệnh OR 1 = 1 sau Gifts như này `https://insecure-website.com/products?category=Gifts'+OR+1=1--` thì nó sẽ thực hiện câu lệnh SQL như này

`SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

câu lệnh truy vấn này có nghĩa là chọn tất cả sản phẩm thuộc 'Gifts' hoặc '1=1' mà '1=1' luôn đúng dẫn tới nó sẽ trả ra tất cả sản phẩm 

<img width="1280" height="625" alt="image" src="https://github.com/user-attachments/assets/de53c8fe-b91a-4eb2-a815-2f82014ba84e" />
