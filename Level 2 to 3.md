### **Mục tiêu**

Truy cập file có dấu cách trong tên.

---

### **Cách tiếp cận**

Dùng `ls` để xem file trong thư mục và ta thấy file '-'.
-> Đây là file có tên đặc biệt ([space in filename](https://www.google.com/search?q=spaces+in+filename)) nên ta không thể dùng trực tiếp `cat ./filename`.
=> Ta sẽ thêm "filename", cụ thể là `cat ./"filename"` để đọc file. 

### **Các lệnh đã dùng**

```
# ls
# cat ./"file"
```

### **Kết quả**

```
bandit2@bandit:~$ ls
--spaces in this filename--
bandit2@bandit:~$ cat ./"--spaces in this filename--"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

Mật khẩu cho level tiếp theo là:

`MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

---

### **Nhận xét**

- Khi vào được server, thao tác cơ bản là `ls` để liệt kê file và `cat ./"file`" để đọc nội dung.
- Mật khẩu tìm thấy sẽ dùng cho bước đăng nhập tiếp theo.