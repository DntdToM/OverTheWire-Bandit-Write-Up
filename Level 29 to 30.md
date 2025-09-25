### **Mục tiêu**

Tìm mật khẩu cho **bandit30** trong repo Git, trong trường hợp password không nằm ở branch mặc định (**master**) mà được giấu ở branch khác.

---

### **Cách tiếp cận**

- Đầu tiên, clone repo về máy thông qua SSH.
- Kiểm tra `README.md` trong branch hiện tại (`master`) → không có password thật, chỉ có placeholder 
	
	`<no passwords in production!>`.
    
- Dùng `git branch -a` để liệt kê tất cả các branch trong repo.
- Sau đó, lần lượt `git checkout` sang các branch khác (`dev`, `sploits-dev`) để kiểm tra nội dung.
- Khi đọc file `README.md` trong branch `dev`, tìm được password thật.

### **Các lệnh đã dùng**

```
# Tạo thư mục tạm
mktemp -d
cd /tmp/tmp.<random_string>

# Clone repo từ server qua SSH
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
cd repo

# Liệt kê các branch
git branch -a

# Chuyển sang branch dev
git checkout dev

# Đọc nội dung file README.md để tìm password
cat README.md
```

### **Kết quả**

```
bandit29@bandit:/tmp/tmp.fcpKxu7fyE/repo$ git branch -a
  master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev

bandit29@bandit:/tmp/tmp.fcpKxu7fyE/repo$ git checkout dev
Switched to branch 'dev'

bandit29@bandit:/tmp/tmp.fcpKxu7fyE/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

Mật khẩu cho level tiếp theo là:

`qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL`
