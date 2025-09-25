### **Mục tiêu**

Nhiệm vụ của level này là:

- Dùng **password hiện tại** (của `bandit16`) gửi đến một **port trên localhost** trong dải **31000 - 32000**.
- Xác định port nào đang chạy dịch vụ và port nào hỗ trợ **SSL/TLS**.
- Trong số các port, chỉ có **1 port duy nhất** sẽ trả về **private key SSH** để đăng nhập level tiếp theo.
- Các port còn lại sẽ chỉ trả lại đúng dữ liệu mà ta gửi lên.

---

### **Cách tiếp cận**

- **Quét các port** từ 31000 đến 32000 trên `localhost` để xem port nào mở.
- **Kiểm tra từng port** để xác định:
    - Port nào chỉ phản hồi bình thường (echo dữ liệu gửi lên).
    - Port nào có hỗ trợ **SSL/TLS**.
        
- **Gửi mật khẩu hiện tại** đến port SSL/TLS hợp lệ.
- Nếu đúng, server sẽ trả về **RSA Private Key**.
- **Lưu private key**, chỉnh quyền truy cập (`chmod 600`), sau đó dùng key để đăng nhập vào `bandit17`.

### **Các lệnh đã dùng**

```
# 1. Quét dải port từ 31000-32000
nmap localhost -p 31000-32000
nmap -sV localhost -p 31000-32000 #quét kết nối gates

# 2. Gửi password qua SSL để lấy key (port: 31790)
openssl s_client -connect localhost:31790 -ign_eof
# hoặc
ncat --ssl localhost 31790

# 3. Lưu key RSA vào file
vim bandit17.key

# 4. Chỉnh quyền file
chmod 600 bandit17.key

# 5. SSH với private key
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

### **Kết quả**

```
bandit16@bandit:~$ ncat --ssl localhost 31790
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
...
-----END RSA PRIVATE KEY-----

vim bandit17.key
chmod 600 bandit17.key
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

---

### **Nhận xét**

- `nmap` giúp nhanh chóng phát hiện port mở và dịch vụ đang chạy.
- Khi làm việc với SSL/TLS, `openssl s_client` là công cụ tuyệt vời để **debug handshake** và xác định chứng chỉ.
- `ncat --ssl` thuận tiện hơn `openssl` khi muốn gửi dữ liệu thô qua SSL.
- Ý nghĩa của `-ign_eof` (Ignore EOF (End Of File)):
	- OpenSSL sẽ không đóng kết nối ngay khi bạn nhập xong dữ liệu.
	- Nó giữ kết nối mở để tiếp tục đọc dữ liệu từ server cho đến khi server tự đóng.
	
- Khi lưu SSH key, luôn **chỉnh quyền về 600** trước khi sử dụng, nếu không SSH sẽ từ chối.