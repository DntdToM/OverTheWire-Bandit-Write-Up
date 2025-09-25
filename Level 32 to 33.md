### **Mục tiêu**

- Khi đăng nhập vào `bandit32`, bạn không có shell bình thường (`/bin/bash`), mà thay vào đó là **UPPERCASE SHELL**, nơi tất cả lệnh bạn gõ sẽ bị tự động **chuyển thành chữ in hoa**.
- Nhiệm vụ là thoát khỏi shell bị giới hạn này, sau đó đọc password trong file `/etc/bandit_pass/bandit33`.

---

### **Cách tiếp cận**

Shell hiện tại đổi mọi lệnh thành chữ in hoa, ví dụ:

![[Pasted image 20250925095642.png]]

→ Lệnh `ls` bị đổi thành `LS` nên không chạy được, vì Linux phân biệt chữ hoa và chữ thường.

- Để bypass, ta cần:
    1. Chạy lệnh `$0` để thoát khỏi shell đặc biệt này.
    2. Sau khi có shell chuẩn (`sh`), chỉ cần dùng `cat` để đọc password.

### **Các lệnh đã dùng**

```
# Thoát khỏi restricted shell
>> $0

# Đọc nội dung file password của level tiếp theo
$ cat /etc/bandit_pass/bandit33
```

### **Kết quả**

```
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

Mật khẩu cho level tiếp theo là:

`tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0`
