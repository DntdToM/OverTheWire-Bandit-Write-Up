### **Mục tiêu**

- Trong thư mục home có hai file:
    - `passwords.old` → Danh sách mật khẩu cũ.
    - `passwords.new` → Danh sách mật khẩu mới.
        
- Nhiệm vụ: **Tìm dòng mật khẩu duy nhất thay đổi giữa 2 file**.
- Mật khẩu mới chính là **password cho level tiếp theo**.

---

### **Cách tiếp cận**

- Xem danh sách file để đảm bảo có đủ 2 file.
- Dùng lệnh `diff` để tìm sự khác nhau giữa `passwords.old` và `passwords.new`.
- Lấy dòng bị thay đổi trong file `passwords.new`.
- Đây sẽ là password để đăng nhập vào `bandit18`.

### **Các lệnh đã dùng**

```
# 1. Kiểm tra file trong thư mục
ls

# 2. Xem nội dung file (optional)
cat passwords.old
cat passwords.new

# 3. Dùng diff để so sánh
diff passwords.new passwords.old

# 4. Hoặc chỉ lấy phần bị thay đổi trong passwords.new
grep -v -f passwords.old passwords.new
```

### **Kết quả**

```
# Chạy lệnh diff
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> gvE89l3AhAhg3Mi9G2990zGnn42c8v20

# Grep
bandit17@bandit:~$ grep -v -f passwords.old passwords.new
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

Mật khẩu cho level tiếp theo là:

`x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

---

### **Nhận xét**

- `diff` là công cụ tuyệt vời để so sánh nội dung file từng dòng.
- Với bài này, ta có thể kết hợp `grep` để lọc nhanh:
    - `grep -v` → lọc ra những dòng **khác biệt**.
    - `-f passwords.old` → dùng file `passwords.old` làm mẫu so sánh.
        
- Đây là tình huống thực tế khi làm việc với file log hoặc config để tìm thay đổi quan trọng.