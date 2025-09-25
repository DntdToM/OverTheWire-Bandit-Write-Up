### **Mục tiêu**

Tìm **password** cho user `bandit8`:
- Password được lưu trong **file `data.txt`**.
- Password **nằm ngay sau từ khóa** `millionth`.

---

### **Cách tiếp cận**

- Tìm **password** cho user `bandit8`.
- Password được lưu trong **file `data.txt`**.
- Password **nằm ngay sau từ khóa** `millionth`.

### **Các lệnh đã dùng**

```
cat data.txt | grep "millionth"

# hoặc

grep "millionth" data.txt
```

### **Kết quả**

```
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ cat data.txt | grep "millionth" millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

Mật khẩu cho level tiếp theo là:

`dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

---

### **Nhận xét**

- Sử dụng `grep` để tìm kiếm chuỗi trong file.
- Hiểu cách lọc dữ liệu bằng `pipe` (`|`).