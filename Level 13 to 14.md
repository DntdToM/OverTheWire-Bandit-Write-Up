### **Mục tiêu**

- Ở level này, mật khẩu cho level tiếp theo **không thể đọc trực tiếp bởi user `bandit13`**.
- Thay vào đó, ta được cấp một **private SSH key** (`sshkey.private`) để **đăng nhập trực tiếp vào user `bandit14`**.
- Sau khi đăng nhập thành công với SSH key, ta sẽ tìm được mật khẩu cho level tiếp theo trong file:
    `/etc/bandit_pass/bandit14`

> **Lưu ý:**
> - `localhost` đại diện cho chính server hiện tại.
> - Cổng SSH mặc định của trò chơi này là `2220`.

---

### **Cách tiếp cận**

- Gõ `ls` và ta thấy file `sshkey.private` -> đây là [SSH key](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)
- Đăng nhập vào `bandit14` bằng SSH key, gõ lệnh:
    
    `ssh -i sshkey.private -p 2220 bandit14@localhost`
    
    - `-i sshkey.private`: chỉ định file private key để xác thực.
    - `-p 2220`: kết nối SSH qua cổng 2220 (khác với mặc định 22).
    - `bandit14@localhost`: đăng nhập vào chính máy hiện tại nhưng với user `bandit`.
        
- Đọc mật khẩu trong `/etc/bandit_pass/bandit14`
    Sau khi đăng nhập thành công, dùng lệnh:
	    `cat /etc/bandit_pass/bandit14`

### **Các lệnh đã dùng**

```
bandit13@bandit:~$ ls
sshkey.private

bandit13@bandit:~$ ssh -i sshkey.private -p 2220 bandit14@localhost
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

### **Kết quả**

```
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

Mật khẩu cho level tiếp theo là:

`MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

---

### **Nhận xét**

- Bài này giúp mình làm quen với **xác thực SSH bằng key** thay vì mật khẩu.

- Cần chú ý:
    - Luôn chỉ định đúng private key bằng `-i filekey`.
    - Đúng port với `-p` (ở Bandit là `2220`).
    
- Khi gặp lỗi `Permission denied` hoặc không đăng nhập được:
    - Kiểm tra quyền của key: `chmod 600 sshkey.private` 
	    -> cấp quyền.
    - Đảm bảo đúng hostname và port.