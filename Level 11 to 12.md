### **Mục tiêu**

File `data.txt` chứa mật khẩu, nhưng **các ký tự chữ cái đã bị mã hóa bằng ROT13**.
- ROT13 là kỹ thuật **dịch chuyển chữ cái 13 vị trí trong bảng chữ cái**:
    - `A ↔ N`, `B ↔ O`, ..., `M ↔ Z`
    - Tương tự cho chữ thường `a ↔ n`, `b ↔ o`, ..., `m ↔ z`
- Cần giải mã ROT13 để tìm ra mật khẩu.

---

### **Cách tiếp cận**

Dùng lệnh `tr` để **chuyển đổi ký tự** từ bảng chữ cái này sang bảng chữ cái dịch chuyển 13 vị trí.

`cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

**Trong đó:**
- `A-Za-z` → tập hợp tất cả ký tự chữ cái hoa và thường.
- `N-ZA-Mn-za-m` → ánh xạ các ký tự sang ký tự dịch 13 vị trí theo ROT13.

### **Các lệnh đã dùng**

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

### **Kết quả**

```
bandit11@bandit:~$ ls
data.txt

bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

Mật khẩu cho level tiếp theo là:

`7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

---

### **Nhận xét**

- ROT13 là một dạng **mã hóa cổ điển.
- `tr` là công cụ mạnh mẽ để **chuyển đổi ký tự hàng loạt**.