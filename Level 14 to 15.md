### **Mục tiêu**

- Mật khẩu cho **Level 15** không lưu trong file như các level trước.
- Thay vào đó, **ta phải gửi mật khẩu của Level 14** (mật khẩu hiện tại) **tới port `30000` trên `localhost`**.
- Khi gửi đúng, server sẽ trả về mật khẩu cho Level 15.

> **Localhost** là địa chỉ đặc biệt `127.0.0.1` dùng để chỉ chính máy ta đang kết nối.

---

### **Cách tiếp cận**

- **Lấy mật khẩu Level 14**  
    Ở bước trước, chúng ta đã tìm được mật khẩu cho Level 14:
    
    `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`
    
- **Dùng `nc` (Netcat) để kết nối tới port 30000**
    - `nc` là công cụ cho phép mở kết nối TCP/UDP trực tiếp từ command line.
        
        `nc localhost 30000`
        
    - `localhost` = chính máy server đang chạy Bandit.
    - `30000` = số cổng yêu cầu.
        
- **Gửi mật khẩu hiện tại qua kết nối Netcat**
    - Sau khi kết nối, **gõ mật khẩu của Level 14**, nhấn **Enter**.
    - Nếu đúng, server sẽ trả về mật khẩu của Level 15.

### **Các lệnh đã dùng**

```
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

### **Kết quả**

```
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

Mật khẩu cho level tiếp theo là:

`8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

---

### **Nhận xét**

- `nc` hoạt động như một client gửi dữ liệu tới server qua TCP.
    -> Rất hữu ích để kiểm tra dịch vụ đang chạy hoặc debug mạng.
        
- Kiến thức liên quan:
    - **Port** là điểm cuối (endpoint) cho kết nối mạng.
    - `localhost:30000` nghĩa là kết nối **tới chính server này**, tại cổng `30000`.
