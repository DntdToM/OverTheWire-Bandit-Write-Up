### **Mục tiêu**

Tìm mật khẩu ẩn bên trong thư mục `inhere`.

- Khi đăng nhập vào `bandit3`, ta chỉ có quyền truy cập giới hạn, nên cần tìm file mà ta được phép đọc để lấy password.

---

### **Cách tiếp cận**

1. **Bước 1: Kiểm tra thư mục hiện tại**
    - Sử dụng `ls` để xem có file hoặc thư mục nào đặc biệt không.
    - Nhìn thấy thư mục `inhere`.
        
2. **Bước 2: Xác định loại của `inhere`**
    - Dùng lệnh `file inhere` để kiểm tra, kết quả cho thấy đây là một **directory** (thư mục).
        
3. **Bước 3: Truy cập vào thư mục**
    - `cd inhere` để vào trong, sau đó `ls` nhưng không thấy gì → có khả năng file bị **ẩn**.
        
4. **Bước 4: Liệt kê file ẩn**
    - Dùng `ls -la` để hiện tất cả file, bao gồm file ẩn.
    - Tìm thấy file có tên **`...Hiding-From-You`**.
        
5. **Bước 5: Đọc nội dung file**
    - Do tên file có ký tự đặc biệt (`...`), nên dùng cú pháp `./` trước tên file để `cat` chính xác.
    - Cú pháp đầy đủ:
        
        `cat ./...Hiding-From-You`

### **Các lệnh đã dùng**

```
# Kiểm tra thư mục ban đầu ls file inhere  
# Truy cập và kiểm tra thư mục cd inhere ls -la  
# Đọc nội dung file đặc biệt cat ./...Hiding-From-You
```
### **Kết quả**

```
bandit3@bandit:~$ ls inhere 
bandit3@bandit:~$ file inhere 
inhere: directory 
bandit3@bandit:~$ cd inhere 
bandit3@bandit:~/inhere$ ls 
bandit3@bandit:~/inhere$ ls -la 
total 12 
drwxr-xr-x 2 root    root    4096 Aug 15 13:16 . 
drwxr-xr-x 3 root    root    4096 Aug 15 13:16 .. 
-rw-r----- 1 bandit4 bandit3   33 Aug 15 13:16 ...Hiding-From-You bandit3@bandit:~/inhere$ cat ./...Hiding-From-You 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```
Mật khẩu cho level tiếp theo là:

`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

---

### **Nhận xét**

- Khi không thấy file với `ls`, hãy thử `ls -la` để tìm **file ẩn**.
- Các file có tên đặc biệt hoặc bắt đầu bằng dấu `.` cần gọi kèm `./` để tránh lỗi.
- Quy trình tìm kiếm: **ls → ls -la → cat ./file** là pattern rất thường gặp ở các level đầu.