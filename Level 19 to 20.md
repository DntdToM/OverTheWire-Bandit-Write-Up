### **Mục tiêu**

- Trong thư mục home có một file thực thi tên là **`bandit20-do`** với quyền **SUID**:
    
    `-rwsr-x---   1 bandit20 bandit19 14884 Aug 15 13:16 bandit20-do`
    
- Mục tiêu là **lấy mật khẩu cho level tiếp theo (`bandit20`)** từ file `/etc/bandit_pass/bandit20`.

=> Cần chạy file `bandit20-do` đúng cách để đọc được mật khẩu, lợi dụng quyền SUID.

---

### **Cách tiếp cận**

- Quyền **SUID (Set User ID)** cho phép file thực thi **chạy với quyền của owner**, trong trường hợp này là `bandit20`.  
    → Khi `bandit19` chạy file `bandit20-do`, nó sẽ được thực thi với quyền `bandit20`.
    
- Ta có thể chạy file này **mà không cần đăng nhập trực tiếp vào user `bandit20`**, nhưng phải cung cấp **lệnh muốn chạy** dưới dạng tham số.
- Ý tưởng:
    1. Chạy file `bandit20-do` **không có tham số** để xem hướng dẫn.
    2. Dùng nó để thực thi lệnh `cat` lên file `/etc/bandit_pass/bandit20`.

### **Các lệnh đã dùng**

```
# 1. Kiểm tra file trong thư mục
ls -la

# 2. Chạy file để xem hướng dẫn
./bandit20-do

# 3. Chạy file với lệnh cat để đọc mật khẩu
./bandit20-do cat /etc/bandit_pass/bandit20
```

### **Kết quả**

```
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

Mật khẩu cho level tiếp theo là:

`0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

---

### **Giải thích**

SUID = Set User ID
- Là một đặc tính quyền của file nhị phân (executable) trên Linux/Unix.
- Khi **file SUID được chạy**, chương trình **chạy với quyền của owner của file**, **không phải quyền của user đang chạy**.

**Ví dụ:**
- File `bandit20-do` có owner là `bandit20` và setuid bật.
- Bạn là `bandit19`. Khi chạy:

	`./bandit20-do`
	`./bandit20-do cat /etc/bandit_pass/bandit20`

→ Chương trình **thực thi với quyền bandit20**, mặc dù bạn là bandit19.

Cách nhận biết file SUID:   `ls -l`
Kết quả có dạng:

`-rwsr-xr-x 1 bandit20 bandit20 12345 Sep 18 10:00 bandit20-do`

- Chú ý **`s` ở vị trí owner** (`rws`) → **đang bật SUID**.
- Nếu là `-rwxr-xr-x` → **không phải SUID**.

### **Nhận xét

- **SUID** là cơ chế quan trọng trong Linux:
    - Giúp một file có thể được chạy với quyền cao hơn, ví dụ như `passwd` để thay đổi mật khẩu.
    - Nhưng nếu quản lý sai, có thể bị lợi dụng để **leo thang đặc quyền** ([privilege escalation](https://en.wikipedia.org/wiki/Privilege_escalation)).
        
- Trong bài này:
    - `bandit20-do` được thiết kế an toàn để chỉ chạy lệnh được chỉ định và không cho phép nhập trực tiếp vào shell.
    - Ta chỉ cần cung cấp lệnh `cat` cùng đường dẫn file để đọc password.
        
- Đây là tình huống thường gặp trong pentest và bảo mật:
    - Nếu một file SUID tồn tại, kẻ tấn công có thể lợi dụng để **thực thi lệnh với quyền cao hơn**.
    - Luôn kiểm tra các file SUID bằng lệnh:
        
        `find / -perm -4000 -type f 2>/dev/null`
        
    - Đảm bảo chỉ những chương trình thật sự cần thiết mới có quyền này.