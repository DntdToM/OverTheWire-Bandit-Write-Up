### **Mục tiêu**

- Đăng nhập vào user bandit26 từ bandit25.
- Tuy nhiên, shell mặc định của bandit26 không phải `/bin/bash` hay shell thông thường, mà là một shell đặc biệt (giới hạn thao tác) (restricted shell).  
    → Khi SSH vào, bạn không có quyền chạy lệnh tùy ý.

---

### **Cách tiếp cận**

- Kiểm tra file trong thư mục bằng `ls`, ta thấy file `bandit26.sshkey`. Dùng file RSA key này để vào `bandit26@localhost`:

	`ssh -i bandit26.sshkey -p 2220 bandit26@localhost`

- Sau khi gõ `yes` thì server đóng ngay lập tức. Vậy ta phải thu nhỏ lại ô cửa sổ Terminal để tránh bị nhảy khỏi server, do có cơ chế cuộn.

![[Pasted image 20250925081304.png]]

- Sau đó gõ `v` để mở trình Vim, rồi nhập:
	`:e /etc/bandit_pass/bandit26`

=> Ta lập tức thấy được password của level 26.

### **Các lệnh đã dùng**

```
# Thu nhỏ cửa sổ Terminal
# Vào bandit26
ssh -i bandit26.sshkey -p 2220 bandit26@localhost

# Mở vim
v
:e /etc/bandit_pass.bandit26
```

### **Kết quả**

```
:e /etc/bandit_pass/bandit26
s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
```

Muốn thoát vim thì gõ ESC + `:q!`

Mật khẩu cho level tiếp theo là:

`s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ`

---

### **Nhận xét**

- Về bản chất bài này:
    - Đây là bài học về restricted shell (rsh hoặc rbash) – dạng shell bị giới hạn tính năng, chỉ cho phép chạy một số lệnh cụ thể.
    - Mục tiêu không chỉ là đăng nhập mà còn thoát khỏi môi trường bị khóa, giống tình huống thực tế khi xâm nhập server nhưng bị hạn chế quyền.
    - Đây là bước chuẩn bị cho kỹ thuật privilege escalation, khi bạn đã vào được một tài khoản nhưng chưa thể làm mọi thứ.
        
- Điểm thú vị và quan trọng:
    - Việc xác định loại shell là cực kỳ quan trọng, dùng:
        
        `cat /etc/passwd | grep bandit26`
        
        để xem shell đang dùng.
        
    - Thường có các dấu hiệu như:
        - Không chạy được `cd` hoặc các lệnh cơ bản.
        - Chỉ được phép gõ một số câu lệnh rất hạn chế.
    - Nhiều shell giới hạn nhưng không kiểm soát hoàn toàn, vẫn có những "kẽ hở" như:
        - Chạy lệnh thông qua trình soạn thảo (`vi`, `nano` → `:!sh`).
        - Sử dụng các lệnh được phép nhưng có khả năng spawn shell (`less`, `man`, `vim`).
        - Khai thác cú pháp `ssh` để truyền lệnh trực tiếp (`ssh bandit26@host cat /etc/bandit_pass/bandit26`).