### **Mục tiêu**

- Đề nói rằng có một repo Git ở:
    
    `ssh://bandit27-git@localhost:2220/home/bandit30-git/repo`
    
- Bạn cần **clone repo này** và tìm ra **password** để đăng nhập vào `bandit28`.

---

### **Cách tiếp cận**

- Thư mục `/home/bandit27` không cho phép ghi file → **không thể clone trực tiếp vào đây**.
- Dùng `mktemp -d` để tạo thư mục tạm thời trong `/tmp`.
- Clone repo từ remote thông qua SSH với cổng `2220`.
- Mở file trong repo (`README`) để tìm mật khẩu.

### **Các lệnh đã dùng**

```
# Tạo thư mục tạm trong /tmp
mktemp -d

# Chuyển vào thư mục vừa tạo
cd /tmp/tmp.<random_string>

# Clone repository từ server
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo

# Di chuyển vào thư mục repo
cd repo

# Kiểm tra file bên trong repo
ls -la

# Đọc nội dung file README để tìm mật khẩu
cat README
```

### **Kết quả**

```
bandit27@bandit:/tmp/tmp.0GqA1KODtG/repo$ cat README
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

Mật khẩu cho level tiếp theo là:

`Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN`

---

### **Nhận xét**

- Luôn dùng `/tmp` để làm việc tạm vì thư mục home bị hạn chế ghi chép.
- Đây là bài giới thiệu cơ bản về **Git**, lệnh chủ yếu sử dụng là `git clone` để tải repo về máy.
- Khi SSH vào repo qua cổng tùy chỉnh, cần thêm `-p 2220` hoặc chỉ rõ port trong URL như đề bài đã cho.