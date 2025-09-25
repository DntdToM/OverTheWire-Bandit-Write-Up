### **Mục tiêu**

Tìm mật khẩu nằm trong một file **bất kỳ bên trong thư mục `inhere`**, với các đặc điểm:

1. Nội dung là **human-readable** (text).
2. Kích thước file chính xác **1033 bytes**.
3. File **không có quyền thực thi** (not executable).

---

### **Cách tiếp cận**

Thư mục `inhere` chứa rất nhiều thư mục con (`maybehere00` → `maybehere19`), mỗi thư mục này có các file ẩn.  
Thay vì kiểm tra thủ công từng file, ta sẽ **tìm trực tiếp file đúng điều kiện** bằng lệnh `find` kết hợp với các tham số lọc:

- `-type f` → chỉ tìm **file** (bỏ qua thư mục).
- `-size 1033c` → tìm file có kích thước **1033 bytes** (`c` = bytes).
- `! -executable` → loại trừ các file có quyền **thực thi**.

Nhờ đó, chỉ file đúng yêu cầu mới xuất hiện, cực nhanh và chính xác.

### **Các lệnh đã dùng**

```
# Bước 1: Tìm file có đúng 3 điều kiện
find . -type f -size 1033c ! -executable

# Bước 2: Khi tìm được đường dẫn, dùng cat để đọc nội dung
cat ./maybehere07/.file2
```

### **Kết quả**

```
bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable
./maybehere07/.file2

bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

Mật khẩu cho level tiếp theo là:

`HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

---

### **Nhận xét**

- `find` là công cụ cực mạnh để **tìm file dựa trên điều kiện cụ thể**.
- Ba tham số quan trọng cần nhớ:
    - `-type f` → lọc ra file.
    - `-size [số]c` → lọc theo kích thước (tính bằng byte).
    - `! -executable` → lọc bỏ các file có quyền thực thi.
- Với bài này, không cần phải mở từng file, tiết kiệm rất nhiều thời gian và tránh làm rối terminal.

⚡ **Tip nâng cao:**  
Có thể kết hợp `find` với `xargs` hoặc `exec` để tự động `cat` file ngay khi tìm thấy:

`find . -type f -size 1033c ! -executable -exec cat {} \;`