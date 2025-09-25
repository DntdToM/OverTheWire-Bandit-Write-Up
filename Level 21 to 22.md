### **Mục tiêu**

- Có một chương trình đang chạy **tự động** theo chu kỳ bằng [**cron job**](https://viblo.asia/p/cron-job-la-gi-huong-dan-su-dung-cron-tab-E375zLo2ZGW) với quyền **bandit22**.
- Cron job này có thể chứa **manh mối hoặc script** ghi password của bandit22 ra một nơi nào đó mà ta có thể đọc được.
- Nhiệm vụ là:
    - Kiểm tra các cron job trong `/etc/cron.d/`.
    - Phân tích script để tìm nơi lưu **password của bandit22**.
    - Đọc password từ file đó.

---

### **Cách tiếp cận**

1. **Kiểm tra thư mục chứa cron job**  
	Các cron job thường được lưu tại `/etc/cron.d/`.

```
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit24      otw-tmp-dir
clean_tmp          e2scrub_all           sysstat
cronjob_bandit22   leviathan5_cleanup
cronjob_bandit23   manpage3_resetpw_job
```

-> Và ta thấy file cronjob_bandit22.

2. **Đọc nội dung file cron job**

```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22 @reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null * * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

**Giải thích:**

- `@reboot`: script sẽ chạy mỗi khi máy reboot.
- `* * * * *`: script chạy **mỗi phút**.
- Script được thực thi bởi **user bandit22**.
- Output bị chuyển hướng sang `/dev/null` → không lưu log.

→ Script chính nằm ở: `/usr/bin/cronjob_bandit22.sh`.

3. **Phân tích script**

```
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash 
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv 
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

4. **Đọc file chứa password**

```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

→ Password của `bandit22` là:

`tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`

### **Các lệnh đã dùng**

```
# 1. Di chuyển đến thư mục chứa cron jobs
cd /etc/cron.d
ls

# 2. Đọc nội dung cron job liên quan đến bandit22
cat cronjob_bandit22

# 3. Đọc nội dung script được gọi trong cron job
cat /usr/bin/cronjob_bandit22.sh

# 4. Xem file trong /tmp chứa password
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

### **Kết quả**

```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

Mật khẩu cho level tiếp theo là:

`tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`
