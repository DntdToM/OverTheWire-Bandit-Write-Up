### **Mục tiêu**

Truy cập file có tên đặc biệt '-'.

---

### **Cách tiếp cận**

Dùng `ls` để xem file trong thư mục và ta thấy file '-'.
-> Đây là tên file đặc biệt ([dashed filename](https://linux.die.net/abs-guide/special-chars.html)) nên ta không thể dùng trực tiếp `cat -`.
=> Ta sẽ thêm "./", cụ thể là `cat ./-` để đọc file. 

### **Các lệnh đã dùng**

```
# ls
# cat ./-
```

### **Kết quả**

```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

Mật khẩu cho level tiếp theo là:

`263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

---

### **Nhận xét**

- Khi vào được server, thao tác cơ bản là `ls` để liệt kê file và `cat ./file` để đọc nội dung.
- Mật khẩu tìm thấy sẽ dùng cho bước đăng nhập tiếp theo.