### **Mục tiêu**

- Trong thư mục home có một file thực thi tên là **`suconnect`** với quyền **SUID**:
    
    `-rwsr-x--- 1 bandit21 bandit20 14884 Aug 15 13:16 suconnect`
    
    - Chạy với quyền `bandit21` → có thể truy cập file mật khẩu của `bandit21`.
        
- Cách hoạt động của `suconnect`:
    1. Nhận **port number** từ tham số dòng lệnh.
    2. Kết nối đến **localhost** tại port này bằng **TCP**.
    3. Đọc một dòng text từ kết nối đó.
    4. So sánh với **password của bandit20** (level hiện tại).
    5. Nếu **đúng**, gửi lại **password của bandit21** qua cùng kết nối.

**Nhiệm vụ:**

- Tạo một server TCP lắng nghe trên một port tùy ý.
- Khi `suconnect` kết nối đến, gửi vào **password của bandit20**.
- Nhận về password của `bandit21` và lưu lại.

---

### **Cách tiếp cận**

1. **Kiểm tra cách sử dụng chương trình**
    
    `./suconnect`
    
    Output:
    
    `Usage: ./suconnect <portnumber> This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.`
    
    → Xác nhận rằng cần truyền port number và chuẩn TCP.
    
1. **Mở một TCP server để lắng nghe**
    - Dùng `nc` (netcat) để mở một cổng TCP trên localhost.
    - Đồng thời gửi **password của bandit20** ngay khi có kết nối.
    
    `echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4900 &`
    
    Giải thích:
    - `echo -n` → gửi password **mà không thêm ký tự newline**.
    - `nc -l -p 4900` → lắng nghe cổng `4900`.
    - `&` → chạy ở chế độ nền để có thể tiếp tục gõ lệnh khác.
        
2. **Chạy chương trình `suconnect` với port tương ứng**
    
    `./suconnect 4900`
    
    - `suconnect` sẽ kết nối tới server netcat ở bước 2.
    - Nếu password đúng → chương trình sẽ gửi lại **password của bandit21**.
        
3. **Đọc kết quả và lấy password mới**

### **Các lệnh đã dùng**

```
# 1. Kiểm tra cách dùng
./suconnect

# 2. Mở TCP server và gửi password của bandit20
echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4900 &

# 3. Chạy suconnect để kết nối tới port 4900
./suconnect 4900
```

### **Kết quả**

```
bandit20@bandit:~$ echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4900 &
[3] 1639195

bandit20@bandit:~$ ./suconnect 4900
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
[3]-  Done          echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4900
```

Mật khẩu cho level tiếp theo là:

`EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

---

### **Nhận xét

- **Bản chất bài này**:
    - `suconnect` đóng vai trò **client** kết nối đến một dịch vụ TCP nội bộ.
    - Netcat (`nc`) giả lập **server** để nhận kết nối và giao tiếp với `suconnect`.
    - Ta tận dụng quyền SUID của `suconnect` để đọc file `/etc/bandit_pass/bandit21`.
        
- **Điểm quan trọng:**
    - Phải **mở listener trước**, sau đó mới chạy `suconnect`.
    - `echo -n` rất quan trọng, nếu không sẽ thêm newline gây sai password.
    - `&` cho phép chạy tiến trình nền, rất hữu ích khi chỉ có **một terminal**.
        
- **Ứng dụng thực tế:**
    - Kỹ thuật này thường thấy trong pentest và CTF khi khai thác các chương trình **SUID** hoặc **socket-based**.
    - `nc` là công cụ mạnh để debug, test kết nối TCP/UDP và viết PoC nhanh chóng.