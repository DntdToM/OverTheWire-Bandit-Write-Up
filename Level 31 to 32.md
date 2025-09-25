### **Mục tiêu**

- Đề bài yêu cầu **push một file mới lên Git repository** tại:
    
    `ssh://bandit31-git@localhost:2220/home/bandit31-git/repo`
    
- Yêu cầu cụ thể:
    - Tên file: `key.txt`
    - Nội dung: `May I come in?`
    - Branch: `master`
- Sau khi push thành công, hệ thống sẽ xác nhận và trả về password để đăng nhập level 32.

---

### **Cách tiếp cận**

- Tạo tớng dẫn trong README.md    
- Xử lý `.gitignore`
    - Mở file `.gitignore`, thấy `*.txt` → mọi file `.txt` bị bỏ qua khi add.
    - Giải pháp: dùng `git add -f` để ép thêm file bị ignore.       
- Commit và push file
    - Commit file với nội dung `"Add key.txt as required"`.
    - Push lên remote branch `master`:
        
        `git push origin master`
        
- Nhận password từ server: sau khi push, server sẽ kiểm tra nội dung và nếu đúng yêu cầu sẽ hiển thị password.

### **Các lệnh đã dùng**

```
# Tạo thư mục tạm
mktemp -d
cd /tmp/tmp.<random_string>

# Clone repo từ server
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
cd repo

# Đọc hướng dẫn từ README.md
cat README.md

# Tạo file key.txt với nội dung yêu cầu
echo "May I come in?" > key.txt

# Do .gitignore có "*.txt", cần ép add file này
git add -f key.txt

# Commit file
git commit -m "Add key.txt as required"

# Push lên branch master
git push origin master
```

### **Kết quả**

Khi push lên thành công, server phản hồi:

```
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```

Mật khẩu cho level tiếp theo là:

`3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K`

---

### **Nhận xét**

- Đây là bài về Git workflow cơ bản, đặc biệt liên quan đến `.gitignore`:
    - `.gitignore` có thể ngăn file bị commit.
    - Dùng `git add -f` để **ép** Git thêm file bị ignore.
        
- Quá trình chuẩn:
    1. `git add -f` → thêm file.
    2. `git commit` → ghi lại thay đổi.
    3. `git push` → gửi lên remote.