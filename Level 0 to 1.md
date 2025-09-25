### **Mục tiêu**

Liệt kê và truy cập (mở, đọc) file trong thư mục.

---

### **Cách tiếp cận**

Để chơi Bandit, ta phải đăng nhập vào server từ xa. SSH là giao thức bảo mật phổ biến dùng để truy cập terminal của máy chủ.

- Đầu tiên, kết nối SSH bằng tài khoản `bandit0`.
- Sau đó kiểm tra thư mục hiện tại để tìm file chứa mật khẩu.
- Đọc file này để lấy password cho level tiếp theo.

### **Các lệnh đã dùng**

```
# Kết nối tới server ssh bandit0@bandit.labs.overthewire.org -p 2220  
# Sau khi login thành công, kiểm tra file trong thư mục ls  
# Đọc nội dung file readme: cat readme
```

### **Kết quả**

```
bandit0@bandit:~$ ls readme 
bandit0@bandit:~$ cat readme 
The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

Mật khẩu cho level tiếp theo là:

`ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

---

### **Nhận xét**

- **SSH** là công cụ nền tảng của game Bandit, nên cần nhớ cú pháp đăng nhập chuẩn.
- Khi vào được server, thao tác cơ bản là `ls` để liệt kê file và `cat` để đọc nội dung.
- Mật khẩu tìm thấy sẽ dùng cho bước đăng nhập tiếp theo.