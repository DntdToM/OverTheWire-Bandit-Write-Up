### **Mục tiêu**

- Có một chương trình chạy **tự động mỗi phút** thông qua **cron job** với quyền `bandit23`.
- Cron job này có nhiệm vụ sao chép password từ `/etc/bandit_pass/bandit23` sang một file tạm trong thư mục `/tmp`.
- Nhiệm vụ:
    - Kiểm tra nội dung cron job để tìm **script** mà nó chạy.
    - Hiểu cách script hoạt động để xác định **đường dẫn chính xác của file chứa password**.
    - Đọc file và lấy password cho level 23.

---

### **Cách tiếp cận**

1. **Kiểm tra thư mục chứa cron job**  
	Các cron job thường được lưu tại `/etc/cron.d/`.

```
bandit22@bandit:/etc/cron.d$ ls -l
total 40
-r--r----- 1 root root  47 Aug 15 13:16 behemoth4_cleanup
-rw-r--r-- 1 root root 123 Aug 15 13:09 clean_tmp
-rw-r--r-- 1 root root 120 Aug 15 13:16 cronjob_bandit22
-rw-r--r-- 1 root root 122 Aug 15 13:16 cronjob_bandit23
-rw-r--r-- 1 root root 120 Aug 15 13:16 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8  2024 e2scrub_all
-r--r----- 1 root root  48 Aug 15 13:17 leviathan5_cleanup
-rw------- 1 root root 138 Aug 15 13:17 manpage3_resetpw_job
-rwx------ 1 root root  52 Aug 15 13:19 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
```

-> Và ta thấy file cronjob_bandit23.

2. **Đọc nội dung file cron job**

```
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

**Giải thích:**

- `@reboot`: script sẽ chạy mỗi khi máy reboot.
- `* * * * *`: script chạy **mỗi phút**.
- Script chính được gọi là `/usr/bin/cronjob_bandit23.sh`.
- Output bị chuyển hướng sang `/dev/null` → không lưu log.

3. **Phân tích script**

```
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

4. **Tự tạo hash để tìm tên file**

Ta chỉ cần chạy chính lệnh trong script để lấy tên file:

```
bandit22@bandit:/etc/cron.d$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

→ Password của `bandit23` là:

`0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`

### **Các lệnh đã dùng**

```
# 1. Kiểm tra thư mục chứa cron jobs
cd /etc/cron.d
ls -l

# 2. Đọc nội dung cron job bandit23
cat cronjob_bandit23

# 3. Đọc script được gọi bởi cron job
cat /usr/bin/cronjob_bandit23.sh

# 4. Tạo hash từ câu "I am user bandit23"
echo I am user bandit23 | md5sum | cut -d ' ' -f 1

# 5. Đọc file trong /tmp chứa password
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

### **Kết quả**

```
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

Mật khẩu cho level tiếp theo là:

`0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`
