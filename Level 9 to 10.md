### **Mục tiêu**

Tìm mật khẩu trong file `data.txt`.
- Mật khẩu **nằm trong một đoạn văn bản đọc được (human-readable)**.
- Nó **xuất hiện sau một chuỗi dấu `=`**.

---

### **Cách tiếp cận**

- Dùng lệnh `strings` để **lọc ra các đoạn text có thể đọc được** từ file nhị phân.
- Sau đó dùng `grep "="` để tìm **các dòng chứa dấu `=`**.
- Quan sát kết quả và tìm **dòng chứa mật khẩu**, dòng đó thường đứng sau từ khóa `"password"`.

### **Các lệnh đã dùng**

```
strings data.txt | grep "="
```

### **Kết quả**

```
bandit9@bandit:~$ ls
data.txt

bandit9@bandit:~$ strings data.txt | grep "="
========== theg
VQ=97
[m=K1x
/i8D2[U?=
========== password
LU=W
========== is
=v$,
h{=,rw_c
=}%q
=D!7
YU=<
5=fq
vJ=ho
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
=AdD
```

Mật khẩu cho level tiếp theo là:

`FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

---

### **Nhận xét**

- `strings` rất hữu ích khi cần **trích xuất dữ liệu đọc được từ file nhị phân**.
- Khi kết hợp với `grep`, ta có thể nhanh chóng **lọc ra thông tin quan trọng**.