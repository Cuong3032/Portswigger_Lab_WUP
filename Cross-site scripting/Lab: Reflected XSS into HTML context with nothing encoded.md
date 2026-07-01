<img width="1507" height="564" alt="image" src="https://github.com/user-attachments/assets/60071227-3b89-4546-ae5a-f43b55d3005c" />

Yêu cầu: gọi 1 hàm `alert()`.

Gợi ý: Lab này chứa lỗ hổng `reflected cross-site scripting` trong thanh `search`.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/9fa352bb-6a85-4245-a06f-ec1a861798e1" />

Ở đây tôi có thử bằng tag `h1`

<img width="2497" height="1380" alt="image" src="https://github.com/user-attachments/assets/9e9a0bdc-732c-4ce4-85b4-8113ec9577d4" />

Thì ở đây phần tag đã không in ra có nghĩa là phần tag đó đã bị xử lí không phải 1 chuỗi và ở đây tôi có bật `view source code` lên để xem cho rõ hơn.

<img width="2505" height="1443" alt="image" src="https://github.com/user-attachments/assets/7b09f16b-25bc-4806-919b-1cd9c9f3deea" />

Ở đây có thể thấy rõ hơn tag `h1` đã được tô màu như 1 tag. Và nhìn kỹ lại cách web render thì `'` đang bị đối xử như 1 string bình thường chứ không được cho vào tag `h1` .

Vậy đã rõ ở đây ta có thể thực hiện HTML injection thì ta sẽ chèn vào đây 1 script javascrip alert 1 thông báo xem sao.

<img width="2493" height="1443" alt="image" src="https://github.com/user-attachments/assets/91032ad5-e704-459f-a579-59e68ae3eaa1" />

Ở đây tôi đã thử alert origin của trang web và nó đã trả về thông báo như trên.

<img width="2504" height="1443" alt="image" src="https://github.com/user-attachments/assets/cbce53df-3621-4e2c-8f1f-701a35a94a1d" />
