# Quản Lý Thư Mục và Tệp Tin Trên Linux

## Mục Lục
- [1. Cấu Trúc Thư Mục Trong Linux](#1-cấu-trúc-thư-mục-trong-linux)
- [2. Các Lệnh Quản Lý Thư Mục và Tệp Tin](#2-các-lệnh-quản-lý-thư-mục-và-tệp-tin)
- [3. Quản Lý Quyền Thư Mục và Tệp Tin](#3-quản-lý-quyền-thư-mục-và-tệp-tin)
  - [3.1 Thực Hành Chạy Một Số Lệnh](#31-thực-hành-chạy-một-số-lệnh)
- [4. Nén và Giải Nén Tệp Trong Linux](#4-nén-và-giải-nén-tệp-trong-linux)

---

## 1. Cấu Trúc Thư Mục Trong Linux

![Linux Directory Structure](/Images/directory-structure.png)

**Bảng ý nghĩa các thư mục trong Linux**:

| **Thư mục** | **Miêu tả**                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `/`         | Thư mục gốc, chứa các thư mục cần thiết ở cấp cao nhất trong hệ thống file. |
| `/bin`      | Chứa các file thực thi (*binaries*) có sẵn cho mọi người dùng.              |
| `/boot`     | Chứa các file cần thiết để khởi động hệ thống (*bootloader*, *kernel*).     |
| `/dev`      | Thư mục đặc biệt chứa các file đại diện cho thiết bị phần cứng và ảo.       |
| `/etc`      | Chứa file cấu hình hệ thống, danh sách người dùng, thông điệp hệ thống.     |
| `/home`     | Thư mục chính của người dùng, chứa dữ liệu cá nhân và tài khoản khác.       |
| `/lib`      | Chứa các thư viện chia sẻ và file liên quan đến *kernel*.                   |
| `/media`    | Tự động gắn (*mount*) các thiết bị di động như USB, CD-ROM, DVD.           |
| `/mnt`      | Gắn tạm thời các hệ thống file như CD-ROM, đĩa mềm.                         |
| `/opt`      | Cài đặt phần mềm tùy chọn hoặc ứng dụng bên thứ ba.                         |
| `/proc`     | Chứa thông tin tiến trình và dữ liệu động của hệ thống.                     |
| `/root`     | Thư mục chính của người dùng **root** (*superuser*).                        |
| `/sbin`     | Chứa các file thực thi hệ thống dành cho quản trị viên (*root*).            |
| `/srv`      | Chứa dữ liệu của các dịch vụ như web, FTP, hoặc mạng.                       |
| `/tmp`      | Lưu trữ tạm thời các file và thư mục của ứng dụng.                          |
| `/usr`      | Chứa chương trình, thư viện, tài liệu, và tài nguyên hệ thống chung.        |
| `/var`      | Lưu trữ dữ liệu biến đổi (*log*, *database tạm*, *mail*).                   |

---

## 2. Các Lệnh Quản Lý Thư Mục và Tệp Tin

**Bảng các lệnh quản lý thư mục và tệp tin**:

| **Lệnh**                            | **Ý Nghĩa**                                                                 | **Ví dụ**                                                                 | **Ghi chú**                                                                 |
|-------------------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `ls`                                | Liệt kê thư mục/tệp tin trong thư mục hiện tại.                             | `ls`<br>Kết quả: `file1.txt folder1`                                      | Hiển thị dạng ngắn, không bao gồm tệp ẩn.                                   |
| `ls -l`                             | Liệt kê chi tiết (quyền, kích thước, ngày sửa đổi).                         | `ls -l`<br>Kết quả: `drwxr-xr-x 2 user user 4096 Oct 10 folder1`          | Thường viết tắt là `ll`.                                                    |
| `ls -a`                             | Liệt kê tất cả, kể cả tệp ẩn (bắt đầu bằng `.`).                            | `ls -a`<br>Kết quả: `. .. .hidden file1.txt folder1`                      | Hiển thị `.` (thư mục hiện tại) và `..` (thư mục cha).                      |
| `ls -R`                             | Liệt kê đệ quy tất cả thư mục/tệp tin.                                      | `ls -R`<br>Kết quả: `folder1: file2.txt`                                  | Hiển thị cấu trúc cây thư mục.                                              |
| `ls -lh`                            | Liệt kê chi tiết với kích thước dễ đọc (KB, MB, GB).                        | `ls -lh`<br>Kết quả: `-rw-r--r-- 1 user user 1.2M Oct 10 file1.txt`       | Kết hợp `-l` và `-h` (*human-readable*).                                    |
| `mkdir <tên_thư_mục>`               | Tạo thư mục mới.                                                            | `mkdir myfolder`                                                          | Tạo thư mục trong thư mục hiện tại.                                         |
| `mkdir -p <đường_dẫn>`              | Tạo nhiều thư mục lồng nhau, không báo lỗi nếu đã tồn tại.                  | `mkdir -p folder1/folder2/folder3`                                        | Tạo cả `folder1`, `folder2`, `folder3` nếu chưa tồn tại.                    |
| `touch <tên_tệp>`                   | Tạo tệp rỗng hoặc cập nhật thời gian sửa đổi.                               | `touch file1.txt`                                                         | Tạo `file1.txt` nếu chưa có.                                                |
| `rm <tên_tệp>`                      | Xóa tệp tin.                                                                | `rm file1.txt`                                                            | Chỉ xóa tệp, không xóa thư mục.                                             |
| `rm -r <tên_thư_mục>`               | Xóa thư mục và nội dung bên trong (đệ quy).                                 | `rm -r myfolder`                                                          | Xóa `myfolder` và tất cả tệp/thư mục con.                                   |
| `rm -rf <tên_thư_mục>`              | Xóa thư mục và nội dung mà không hỏi xác nhận.                              | `rm -rf myfolder`                                                         | **Nguy hiểm**, cần thận trọng.                                              |
| `mv <nguồn> <đích>`                 | Di chuyển hoặc đổi tên thư mục/tệp tin.                                     | `mv file1.txt file2.txt`<br>`mv folder1 /path/to/folder2`                 | Nếu `<đích>` chưa tồn tại, tệp/thư mục sẽ được đổi tên hoặc di chuyển.      |
| `cp <nguồn> <đích>`                 | Sao chép tệp tin.                                                           | `cp file1.txt file2.txt`                                                  | Tạo bản sao `file2.txt` từ `file1.txt`.                                     |
| `cp -r <nguồn> <đích>`              | Sao chép thư mục và nội dung (đệ quy).                                      | `cp -r folder1 folder2`                                                   | Sao chép `folder1` và nội dung sang `folder2`.                              |
| `cd <đường_dẫn>`                    | Chuyển đến thư mục chỉ định.                                                | `cd /home/user/folder1`                                                   | Thay đổi thư mục làm việc hiện tại.                                         |
| `cd ..`                             | Chuyển về thư mục cha.                                                      | `cd ..`                                                                   | Di chuyển lên một cấp thư mục.                                              |
| `cd ~`                              | Chuyển về thư mục *home* của người dùng.                                    | `cd ~`                                                                    | Ví dụ: `/home/user`.                                                        |
| `pwd`                               | Hiển thị đường dẫn tuyệt đối của thư mục hiện tại.                          | `pwd`<br>Kết quả: `/home/user/folder1`                                    | In ra vị trí hiện tại.                                                      |
| `rmdir <tên_thư_mục>`               | Xóa thư mục rỗng.                                                           | `rmdir emptyfolder`                                                       | Chỉ xóa nếu thư mục không chứa tệp/thư mục con.                             |
| `find <đường_dẫn> -name <tên>`      | Tìm kiếm tệp/thư mục theo tên.                                              | `find /home -name file1.txt`                                              | Tìm `file1.txt` trong `/home`.                                              |
| `tree`                              | Hiển thị cấu trúc cây thư mục.                                              | `tree`<br>Kết quả: `├── file1.txt └── folder1`                            | Cần cài đặt gói `tree`.                                                     |
| `cat <tên_tệp>`                     | Hiển thị nội dung tệp tin.                                                  | `cat file1.txt`                                                           | In nội dung `file1.txt` ra màn hình.                                        |
| `less <tên_tệp>`                    | Xem nội dung tệp tin theo từng trang.                                       | `less file1.txt`                                                          | Cuộn lên/xuống, thoát bằng `q`.                                             |
| `chmod <quyền> <tên_tệp/thư_mục>`   | Thay đổi quyền truy cập của tệp/thư mục.                                    | `chmod 755 script.sh`                                                     | Quyền: `755` (chủ sở hữu: rwx, nhóm/khác: rx).                              |
| `chown <người_dùng> <tên_tệp/thư_mục>` | Thay đổi chủ sở hữu của tệp/thư mục.                                       | `chown user file1.txt`                                                    | Gán `user` làm chủ sở hữu `file1.txt`.                                      |

---

## 3. Quản Lý Quyền Thư Mục và Tệp Tin

**Bảng các lệnh quản lý quyền**:

| **Lệnh**    | **Cú pháp**                                                                 | **Ý Nghĩa**                                                                 | **Ví dụ**                                                                 | **Ghi chú**                                                                 |
|-------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `chmod`     | `chmod [quyền] <tên_tệp/thư_mục>`<br>`chmod [tùy_chọn] [quyền] <tên_tệp/thư_mục>` | Thay đổi quyền truy cập (*read: r, write: w, execute: x*).                  | `chmod 777 file1.txt`<br>`chmod u+x script.sh`                            | Quyền số: `r=4`, `w=2`, `x=1`. Quyền ký tự: `u` (user), `g` (group), `o` (others). Tùy chọn: `-R` (đệ quy). Yêu cầu `sudo` nếu không phải chủ sở hữu. |
| `chown`     | `chown [người_dùng][:nhóm] <tên_tệp/thư_mục>`<br>`chown [tùy_chọn] [người_dùng][:nhóm] <tên_tệp/thư_mục>` | Thay đổi chủ sở hữu (*user*) và/hoặc nhóm (*group*).                       | `chown user1 file1.txt`<br>`chown user1:group1 folder1`<br>`chown -R user1 folder1` | Tùy chọn: `-R` (đệ quy). Yêu cầu `sudo`. Dùng `chgrp` để chỉ thay đổi nhóm. |
| `chgrp`     | `chgrp [nhóm] <tên_tệp/thư_mục>`<br>`chgrp [tùy_chọn] [nhóm] <tên_tệp/thư_mục>` | Thay đổi nhóm sở hữu.                                                      | `chgrp group1 file1.txt`<br>`chgrp -R group1 folder1`                     | Tùy chọn: `-R` (đệ quy). Yêu cầu `sudo`. Ít phổ biến hơn `chown`.           |
| `stat`      | `stat [tùy_chọn] <tên_tệp/thư_mục>`                                        | Hiển thị thông tin chi tiết (quyền, chủ sở hữu, thời gian sửa đổi).         | `stat file1.txt`<br>`stat -c "%a" file1.txt`                              | Tùy chọn `-c "%a"`: Hiển thị quyền dạng số octal.                           |
| `getfacl`   | `getfacl <tên_tệp/thư_mục>`                                                | Hiển thị danh sách kiểm soát truy cập (*ACL*).                              | `getfacl folder1`                                                         | Xem quyền chi tiết hơn `ls -l`. Cần hỗ trợ ACL.                             |
| `setfacl`   | `setfacl [tùy_chọn] -m u:[người_dùng]:[quyền] <tên_tệp/thư_mục>`           | Thiết lập/sửa đổi ACL.                                                      | `setfacl -m u:user1:rwx file1.txt`<br>`setfacl -R -m g:group1:r folder1`  | Tùy chọn: `-m` (sửa đổi), `-x` (xóa), `-R` (đệ quy). Yêu cầu `sudo`.       |
| `ls -l`     | `ls -l <tên_tệp/thư_mục>`                                                  | Hiển thị quyền và thông tin chi tiết.                                       | `ls -l file1.txt`<br>Kết quả: `-rwxr-xr-x 1 user group1 123 Oct 10 file1.txt` | Quyền dạng `rwxr-xr-x` (chủ sở hữu, nhóm, khác).                           |
| `umask`     | `umask [giá_trị]`                                                          | Thiết lập/hiển thị mặt nạ quyền mặc định khi tạo tệp/thư mục.               | `umask`<br>`umask 0002`                                                   | Quyền mặc định: `666` (tệp) hoặc `777` (thư mục) trừ giá trị `umask`.      |

**Hình ảnh minh họa cấu trúc quyền**:

![File Permission Structure](/Images/structure-permission.png)

**Bảng ký hiệu nhận biết kiểu file**:

![File Type Symbols](/Images/file-type-symbol.png)

**Biểu diễn quyền dưới dạng số**:

![Permission Numbers](/Images/permission-number.png)

### 3.1 Thực Hành Chạy Một Số Lệnh

**#. Hiển thị quyền bằng `ls -l`**  
- Chạy lệnh **`ls -l`** để xem quyền của các thư mục/tệp tin trong thư mục hiện tại:  

![List Permissions](/Images/ls-l.png)

**#. Xem quyền dạng số bằng `stat`**  
- Chạy lệnh **`stat -c "%a" <tên_tệp/thư_mục>`** để xem quyền dưới dạng số nguyên:  

![Stat Permissions](/Images/stat-permission.png)

**#. Thay đổi quyền bằng `chmod`**  
- Chạy lệnh **`chmod 777 <tên_tệp/thư_mục>`** để cấp quyền đọc, ghi, thực thi cho tất cả:  

![ls -l](/Images/ls-l-1.png)  
![Chmod 777](/Images/chmod.png)  


**#. Thay đổi nhóm bằng `chgrp`**  
- Chạy lệnh **`chgrp <tên_nhóm> <tên_tệp/thư_mục>`** để đổi nhóm sở hữu:  

![Change Group](/Images/chgrp.png)

---

## 4. Nén và Giải Nén Tệp Trong Linux

**Bảng các lệnh nén và giải nén**:

| **Lệnh**    | **Cú pháp**                                                                 | **Ý Nghĩa**                                                                 | **Ví dụ**                                                                 | **Ghi chú**                                                                 |
|-------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `tar`       | `tar [tùy_chọn] <tệp_tar> <tệp/thư_mục>`                                   | Tạo, giải nén, hoặc xem nội dung tệp tar.                                   | `tar -cvf archive.tar file1.txt folder1`<br>`tar -xvf archive.tar`        | Tùy chọn: `-c` (tạo), `-x` (giải nén), `-v` (chi tiết), `-f` (tệp đích). Không nén dữ liệu. |
| `tar.gz`    | `tar -zcvf <tệp.tar.gz> <tệp/thư_mục>`<br>`tar -zxvf <tệp.tar.gz>`         | Nén/giải nén tệp tar với gzip.                                              | `tar -zcvf archive.tar.gz folder1`<br>`tar -zxvf archive.tar.gz`          | Tùy chọn `-z`: Dùng gzip. Kích thước nhỏ hơn `tar`.                         |
| `tar.bz2`   | `tar -jcvf <tệp.tar.bz2> <tệp/thư_mục>`<br>`tar -jxvf <tệp.tar.bz2>`       | Nén/giải nén tệp tar với bzip2.                                             | `tar -jcvf archive.tar.bz2 folder1`<br>`tar -jxvf archive.tar.bz2`        | Tùy chọn `-j`: Dùng bzip2. Nén tốt hơn `gzip` nhưng chậm hơn.               |
| `zip`       | `zip [tùy_chọn] <tệp.zip> <tệp/thư_mục>`                                   | Nén thành định dạng zip.                                                    | `zip archive.zip file1.txt folder1`<br>`zip -r archive.zip folder1`       | Tùy chọn `-r`: Nén đệ quy. Tương thích với Windows.                         |
| `unzip`     | `unzip [tùy_chọn] <tệp.zip>`                                               | Giải nén tệp zip.                                                           | `unzip archive.zip`<br>`unzip archive.zip -d /path/to/dest`               | Tùy chọn `-d`: Chỉ định thư mục đích. Cần cài đặt `unzip`.                  |
| `gzip`      | `gzip <tệp>`<br>`gunzip <tệp.gz>`                                          | Nén/giải nén tệp đơn với gzip.                                              | `gzip file1.txt`<br>`gunzip file1.txt.gz`                                 | Chỉ nén tệp đơn, thay thế tệp gốc.                                          |
| `bzip2`     | `bzip2 <tệp>`<br>`bunzip2 <tệp.bz2>`                                       | Nén/giải nén tệp đơn với bzip2.                                             | `bzip2 file1.txt`<br>`bunzip2 file1.txt.bz2`                              | Chỉ nén tệp đơn, nén tốt hơn `gzip` nhưng chậm hơn.                         |
| `xz`        | `xz <tệp>`<br>`unxz <tệp.xz>`                                              | Nén/giải nén tệp đơn với xz.                                                | `xz file1.txt`<br>`unxz file1.txt.xz`                                     | Nén hiệu quả hơn `gzip` và `bzip2`. Chỉ nén tệp đơn.                        |
| `tar.xz`    | `tar -Jcvf <tệp.tar.xz> <tệp/thư_mục>`<br>`tar -Jxvf <tệp.tar.xz>`         | Nén/giải nén tệp tar với xz.                                                | `tar -Jcvf archive.tar.xz folder1`<br>`tar -Jxvf archive.tar.xz`          | Tùy chọn `-J`: Dùng xz. Hiệu quả nén cao.                                   |
| `7z`        | `7z a <tệp.7z> <tệp/thư_mục>`<br>`7z x <tệp.7z>`                           | Nén/giải nén thành định dạng 7z.                                            | `7z a archive.7z file1.txt folder1`<br>`7z x archive.7z`                  | Cần cài đặt `p7zip`. Hỗ trợ nhiều định dạng.                                |

**#. Nén folder bằng lệnh `tar -czvf`**  
- Chạy lệnh **`tar -czvf file.tar.gz Game/`** để nén thư mục `Game`:  

![Change Group](/Images/tar.png)

**#. Nén folder bằng lệnh `tar -czvf`**  
- Chạy lệnh **`tar -xzvf file.tar.gz`** giải nén file `file.tar.gz`:  

![Change Group](/Images/untar.png)

---
THE END