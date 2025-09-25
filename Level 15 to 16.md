### **Mục tiêu**

- Ta phải **gửi mật khẩu hiện tại (Level 15)** đến **cổng `30001` trên `localhost`**.
- Khác với Level 14, kết nối này **yêu cầu SSL/TLS encryption**.
- Khi gửi đúng, ta sẽ nhận được mật khẩu của **Level 16**.

> 🔹 `SSL/TLS` là giao thức mã hóa dữ liệu trong quá trình truyền, thường được dùng cho HTTPS hoặc các dịch vụ bảo mật.

---

### **Cách tiếp cận**

Sử dụng `openssl s_client`
Để gửi dữ liệu qua một kết nối SSL/TLS, ta sử dụng:

	`openssl s_client -connect localhost:30001`

- `openssl` là bộ công cụ để làm việc với SSL/TLS.
- `s_client` mở kết nối an toàn tới một server.
- `-connect localhost:30001` nghĩa là kết nối đến cổng `30001` trên máy hiện tại (`localhost`).

> Lệnh này tương tự `nc`, nhưng nó **tạo kết nối an toàn** với chứng chỉ SSL.

Khi kết nối thành công, ta sẽ thấy một đoạn thông tin về chứng chỉ và kết nối SSL.  
Sau đó gõ password lvl 15, ấn Enter và nhận được phản hồi:

`Correct! kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

### **Các lệnh đã dùng**

```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
CONNECTED(00000003)
...
# Thông tin về chứng chỉ xuất hiện ở đây
...
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
closed
```

### **Kết quả**

```
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

Mật khẩu cho level tiếp theo là:

`kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

---

### **Giải thích thêm**

### Vì sao không dùng `nc` như Level 14?

- `nc` chỉ hỗ trợ kết nối TCP thuần túy, **không có mã hóa**.
- Port `30001` yêu cầu **giao thức SSL/TLS** → cần `openssl` hoặc `socat` với cấu hình TLS.

### Ý nghĩa các thông tin chứng chỉ

Khi chạy `openssl s_client`, bạn thấy các dòng như:
	
	`depth=0 CN = SnakeOil verify error:num=18:self-signed certificate`

- **Self-signed certificate**: Chứng chỉ do chính server cấp, không phải từ một CA uy tín.
- Đây là bình thường trong môi trường học tập như OverTheWire.
