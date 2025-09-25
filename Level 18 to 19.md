### **Mục tiêu**

- Mật khẩu của level tiếp theo được lưu trong file **`readme`** nằm trong thư mục home (`~`).
- Tuy nhiên, file **`.bashrc`** đã bị chỉnh sửa để **tự động logout** ngay khi SSH thành công, khiến ta **không thể nhập lệnh** trên shell.

=> Cần **bypass cơ chế auto-logout** này để có thể đọc nội dung file `readme` và lấy mật khẩu.

---

### **Cách tiếp cận**

- Khi SSH đăng nhập bình thường, shell sẽ chạy `.bashrc`, dẫn đến việc bị logout ngay lập tức.
- **Giải pháp:** Dùng SSH để **chạy trực tiếp lệnh từ xa**, bỏ qua shell tương tác.
    - Cú pháp:
        
        `ssh user@host -p <port> <command>`
        
- Trong bài này, chỉ cần kết hợp với lệnh `cat` để đọc file `readme`.

### **Các lệnh đã dùng**

```
# 1. SSH bình thường để kiểm tra (bị logout ngay sau khi đăng nhập)
ssh bandit18@bandit.labs.overthewire.org -p 2220

# 2. Chạy lệnh cat trực tiếp qua SSH để bypass .bashrc
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

### **Kết quả**

```
# Bypass logout và đọc file thành công
bandit18@bandit:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

Mật khẩu cho level tiếp theo là:

`cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

---

### **Nhận xét**

- `.bashrc` có thể được cấu hình để **chặn truy cập shell** hoặc **chạy script độc hại** khi đăng nhập.
- SSH hỗ trợ chạy lệnh từ xa mà **không cần shell tương tác**, đây là kỹ thuật cực kỳ hữu ích trong:
    - Khắc phục sự cố trên server bị cấu hình sai.
    - Tránh bị ảnh hưởng bởi các script phá hoại trong file cấu hình cá nhân.
        
- Trong thực tế, đây là cách quản trị viên dùng để **kiểm soát và sửa lỗi từ xa** một cách an toàn và nhanh chóng.