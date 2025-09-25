### **Mục tiêu**

Tìm ra **file duy nhất chứa nội dung human-readable (text)** trong thư mục `inhere` và lấy password từ đó.

---

### **Cách tiếp cận**

- Trong thư mục `inhere` có 10 file, hầu hết chứa dữ liệu nhị phân (binary).
- Dùng `file` để phân tích nội dung file và xác định file nào là **ASCII text**.
- Khi tìm được file, sử dụng `cat` để đọc nội dung và lấy password.

### **Các lệnh đã dùng**

```
# Bước 1: Kiểm tra loại nội dung của tất cả file trong thư mục
file ./*

# Bước 2: Đọc nội dung file vừa tìm được
cat ./-file07
```

### **Kết quả**

```
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data

bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

Mật khẩu cho level tiếp theo là:

`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

---

### **Nhận xét**

- `file` là công cụ mạnh để phân biệt **text file** và **binary file**.
- Khi thư mục có nhiều file, kết hợp với `grep` giúp xác định file cần tìm rất nhanh.
- Đây là kỹ năng cơ bản trong **forensics** và **pentesting**, đặc biệt khi phân tích dump hoặc log.