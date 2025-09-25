### **Mục tiêu**

- Đề nói rằng có một repo Git ở:
    
    `ssh://bandit28-git@localhost:2220/home/bandit30-git/repo`
    
- Bạn cần **clone repo này** và tìm ra **password** để đăng nhập vào `bandit29`.

---

### **Cách tiếp cận**

- **Bước 1:** Tạo thư mục tạm để chứa repo.
- **Bước 2:** Clone repo về thư mục đó.
- **Bước 3:** Xem commit history bằng lệnh:
    
    `git log -- README.md`
    
    → Liệt kê tất cả các lần chỉnh sửa file `README.md`.
    
- **Bước 4:** Xem nội dung cụ thể của commit cũ bằng:
    
    `git show <commit_id>`
    
- **Bước 5:** Tìm dòng chứa password trong phiên bản cũ của file.

### **Các lệnh đã dùng**

```
# Tạo thư mục tạm trong /tmp
mktemp -d

# Chuyển vào thư mục vừa tạo
cd /tmp/tmp.<random_string>

# Clone repository
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo

# Vào thư mục repo
cd repo

# Xem lịch sử commit của README.md
git log -- README.md

# Xem nội dung commit cụ thể
git show <commit_id>
```

### **Kết quả**

Khi xem commit `68314e012fbaa192abfc9b78ac369c82b75fab8f`, ta thấy password cũ:

```
bandit28@bandit:/tmp/tmp.dOPQFBFfbW/repo$ git show 68314e012fbaa192abfc9b78ac369c82b75fab8f
...
4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

Vậy mật khẩu của level tiếp theo là:

`4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7`

---

### **Nhận xét**

- Điểm khác biệt so với level 27:
    - Ở level 27, mật khẩu có sẵn trong file `README`.
    - Ở level 28, mật khẩu đã bị **xoá khỏi phiên bản mới nhất**, ta phải xem lại **lịch sử commit**.
        
- Lệnh cần nhớ:
    - `git log` → xem danh sách các commit.
    - `git show <commit_id>` → xem chi tiết thay đổi trong một commit.