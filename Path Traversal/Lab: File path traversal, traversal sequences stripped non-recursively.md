<img width="1245" height="586" alt="image" src="https://github.com/user-attachments/assets/2d261fe4-c4eb-43cb-a6b7-30da5f41c96c" />

hãy mở một tab mới hình ảnh và thử truyền `../../../etc/passwd` thì thấy 

<img width="1230" height="234" alt="image" src="https://github.com/user-attachments/assets/5ceec127-f609-48bd-a8f6-d83d2c5b9caa" />

đối chiếu với gợi ý đề bài `The application strips path traversal sequences from the user-supplied filename before using it.` cho thấy trang đã "loại bỏ" các đường truyền dẫn như `../`
vậy thì có thể truyền như này `....//` thay cho `../` để khi gặp chuỗi `../` trang sẽ loại bỏ còn lại `../` vậy thì ta sẽ truyền thử 1 path hoàn chỉnh là 
`....//....//....//etc/passwd`

<img width="1227" height="476" alt="image" src="https://github.com/user-attachments/assets/9a83b9cc-75a2-4b9e-b18b-57b42a158015" />

<img width="1268" height="628" alt="image" src="https://github.com/user-attachments/assets/5d474bc2-eae2-4f05-a90b-513ca83e8898" />

như vậy là thành công được lab này.



