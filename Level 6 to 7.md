### **Mục tiêu**

Tìm **password** cho user `bandit7`.  
File chứa password có các đặc điểm:
- Loại: **file thường** (`-type f`).
- Kích thước: **33 bytes** (`-size 33c`).
- Chủ sở hữu: **user `bandit7`** (`-user bandit7`).
- Thuộc nhóm: **group `bandit6`** (`-group bandit6`).

---

### **Cách tiếp cận**

- Password có thể nằm **ở bất kỳ đâu trong hệ thống**, nên ta sẽ tìm từ thư mục gốc `/`.
- Có thể gặp lỗi **Permission denied**, cần loại bỏ bằng `2>/dev/null`.
- Lệnh **`find`** là công cụ phù hợp để tìm dựa trên nhiều điều kiện:
    - `-type f`: chỉ tìm file thường.
    - `-size 33c`: file có dung lượng đúng **33 bytes**.
    - `-user bandit7`: file do `bandit7` sở hữu.
    - `-group bandit6`: file thuộc group `bandit6`.

### **Các lệnh đã dùng**

```
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```

Trong đó:
`2>/dev/null` → Ném toàn bộ lỗi vào **thùng rác** để output gọn gàng.

### **Kết quả**

```
bandit6@bandit:~$ cd ..

bandit6@bandit:/home$ 

bandit6@bandit:/home$ find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password

bandit6@bandit:/home$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

```

Mật khẩu cho level tiếp theo là:

`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

---

### **Nhận xét**

- Sử dụng `find` với các tham số chính xác để lọc kết quả.
- `2>/dev/null` giúp ẩn các lỗi không cần thiết.