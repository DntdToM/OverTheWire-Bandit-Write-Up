### **Mục tiêu**

File `data.txt` chứa **dữ liệu được mã hóa bằng Base64**.  
Nhiệm vụ là **giải mã nội dung** để tìm mật khẩu cho level tiếp theo.

---

### **Cách tiếp cận**

- Đọc nội dung file bằng `cat` để xem chuỗi dữ liệu mã hóa.
- Sử dụng `base64 -d` (decode) để giải mã.

### **Các lệnh đã dùng**

```
cat data.txt | base64 -d
#
base64 -d data.txt
#
echo "encodedb64==" | base64 -d
```

### **Kết quả**

```
bandit10@bandit:~$ ls
data.txt

bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==

bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

Mật khẩu cho level tiếp theo là:

`dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

---

### **Nhận xét**

- `base64` là định dạng mã hóa phổ biến, thường dùng để **truyền tải dữ liệu văn bản và file nhị phân**.
- Khi thấy dữ liệu có nhiều ký tự chữ, số, `+`, `/`, `=` ở cuối → rất có thể là Base64.