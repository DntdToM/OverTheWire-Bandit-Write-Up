### **Mục tiêu**

- Đề nói rằng có một repo Git ở:
    
    `ssh://bandit30-git@localhost:2220/home/bandit30-git/repo`
    
- Bạn cần **clone repo này** và tìm ra **password** để đăng nhập vào `bandit31`.

---

### **Cách tiếp cận**

- Tạo thư mục tạm để làm việc: `mktemp -d`
- Clone repository: `git clone` 
- Tìm thông tin ẩn
    - Nếu không có gì đặc biệt trong branch chính (`master`), kiểm tra:
        - Branch khác: `git branch -a`
        - Tag ẩn: `git tag` hoặc xem file `.git/packed-refs`.
    - Dùng `git show` để hiển thị nội dung của tag.
- Lấy password từ tag
    - Khi tìm được tag khả nghi, mở nội dung tag để lấy password.

### **Các lệnh đã dùng**

```
# Tạo thư mục tạm để clone repo
mktemp -d
cd /tmp/tmp.<random_string>

# Clone repo từ server qua SSH
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
cd repo

# Kiểm tra file trong repo
ls -la
cat README.md

# Kiểm tra commit history
git log

# Kiểm tra tất cả branch và tag
git branch -a
git tag


# Hiển thị nội dung tag secret
git show secret

## Nếu tag không hiện ra, đọc file packed-refs để tìm
cat .git/packed-refs
```

### **Kết quả**

```
bandit30@bandit:/tmp/tmp.YNrtj1BaOB/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

Mật khẩu cho level tiếp theo là:

`fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy`

---

### **Nhận xét**

- Trong Git, ngoài branch và commit, **tag** cũng có thể chứa dữ liệu quan trọng.
- File `packed-refs` thường được dùng để chứa các tag và branch đã được nén lại, đặc biệt khi repo muốn **giấu thông tin**.
- Luôn kiểm tra đầy đủ:
    - `git log` → commit history
    - `git branch -a` → tất cả branch
    - `git tag` → tag hiện có
    - `.git/packed-refs` → tag ẩn