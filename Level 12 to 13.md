### **Mục tiêu**

File `data.txt` chứa mật khẩu cho level tiếp theo, nhưng file này **không trực tiếp lưu mật khẩu**, mà chứa **hexdump** của một file đã bị **nén nhiều lần** với nhiều định dạng khác nhau (`gzip`, `bzip2`, `tar`).  
Nhiệm vụ là:
1. Chuyển hexdump thành file nhị phân gốc.
2. Nhận diện định dạng file.
3. Giải nén lặp đi lặp lại cho đến khi thu được file text chứa mật khẩu.

---

### **Cách tiếp cận**

- **Tạo thư mục tạm để làm việc**  
    Giúp giữ cho thư mục home gọn gàng và tránh ghi đè file quan trọng.
    
    `mktemp -d`
    
- **Copy file gốc vào thư mục tạm**
    
    `cp /home/bandit12/data.txt data.txt`
    
- **Chuyển từ hexdump về nhị phân**  
    File `data.txt` ban đầu chỉ chứa các ký tự hex, cần dùng `xxd -r`:
    
    `xxd -r data.txt data.bin`
    *Trong đó:*
    - `xxd` là công cụ để **chuyển đổi giữa dữ liệu nhị phân (binary) và dữ liệu dạng hex (hexadecimal dump)**.
    - Binary → Hexdump (`xxd filename`) và Hexdump → Binary (dùng `-r`)
    
- **Dùng `file` để nhận diện định dạng**  
    Lệnh này cho biết file là `gzip`, `bzip2`, hay `tar`.
    
    `file data.bin`
    
- **Giải nén từng bước**  
    Dựa vào kết quả `file`, dùng lệnh giải nén tương ứng:
    - `gzip compressed data` → `gunzip`
    - `bzip2 compressed data` → `bunzip2`
    - `POSIX tar archive` → `tar -xvf`
        
- **Lặp lại quá trình**:  
    Mỗi lần giải nén, kiểm tra file mới bằng `file` rồi giải nén tiếp cho đến khi ra file text.
### **Các lệnh đã dùng**

```
# Tạo thư mục tạm
mktemp -d
cd /tmp/tmp.wgmwgN4Bs9

# Copy file gốc vào
cp /home/bandit12/data.txt data.txt

# Chuyển hexdump về nhị phân
xxd -r data.txt data.bin
file data.bin
# => gzip compressed data

mv data.bin data.gz
gunzip data.gz

file data
# => bzip2 compressed data

mv data data.bz2
bunzip2 data.bz2

file data
# => gzip compressed data

mv data data.gz
gunzip data.gz

file data
# => POSIX tar archive

mv data data.tar
tar -xvf data.tar
# => data5.bin

file data5.bin
# => POSIX tar archive

mv data5.bin data5.tar
tar -xvf data5.tar
# => data6.bin

file data6.bin
# => bzip2 compressed data

mv data6.bin data6.bz2
bunzip2 data6.bz2

file data6
# => POSIX tar archive

mv data6 data6.tar
tar -xvf data6.tar
# => data8.bin

file data8.bin
# => gzip compressed data

mv data8.bin data8.gz
gunzip data8.gz

file data8
# => ASCII text

cat data8
# => The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

```

### **Kết quả**

```
bandit12@bandit:/tmp/tmp.wgmwgN4Bs9$ cat data8
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

Mật khẩu cho level tiếp theo là:

`FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

---

### **Nhận xét**

- Đây là một bài thực hành hay để luyện kỹ năng:
    - Nhận diện định dạng file bằng `file`.
    - Thành thạo các công cụ nén như `gzip`, `bzip2`, `tar`.
    - Quản lý file tạm gọn gàng nhờ `mktemp -d`.
- Rèn **tư duy tuần tự**.