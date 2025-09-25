### **Mục tiêu**

Tìm **dòng duy nhất** trong `data.txt` (chỉ xuất hiện 1 lần), vì đó chính là password.

---

### **Cách tiếp cận**

`cat data.txt | sort | uniq -u`

Giải thích:
- sort → sắp xếp các dòng để các dòng trùng nhau nằm cạnh nhau.
- uniq -u → chỉ hiển thị những dòng xuất hiện đúng 1 lần.
=> Dòng xuất hiện duy nhất này chính là password để vào level tiếp theo.

### **Các lệnh đã dùng**

```
cat data.txt | sort | uniq -u
```

### **Kết quả**

```
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ cat data.txt
ZzQDv5Imr9y5XSYGD3r61uP1fjXAhuod
bjQCibPw40urmZNBNNpv8zTOaFVFWSbv
...
bandit8@bandit:~$ cat data.txt | sort | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

Mật khẩu cho level tiếp theo là:

`4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

---

### **Nhận xét**

- `sort filename | uniq -u` -> sắp xếp, tìm dòng xuất hiện 1 lần.
- `sort filename | uniq -c` -> đếm số lần xuất hiện.
- Đây là kỹ thuật thường dùng trong xử lý log hoặc phân tích dữ liệu để tìm điểm bất thường.