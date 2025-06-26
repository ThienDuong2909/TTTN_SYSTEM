# Tìm hiểu về FTP 

## Mục lục
- [1. Giới thiệu về FTP](#1-giới-thiệu-về-ftp)
- [2. Phân loại FTP Server](#2-phân-loại-ftp-server)
   - [FTP (File Transfer Protocol)](#ftp-file-transfer-protocol)
   - [FTPS - FTP Secure](#ftps---ftp-secure)
   - [SSH File Transfer Protocol - SFTP](#ssh-file-transfer-protocol---sftp)
   - [So sánh giữa SFTP vs FTP](#so-sánh-giữa-sftp-vs-ftp)
   - [So sánh giữa SFTP vs FTPS](#so-sánh-giữa-sftp-vs-ftps)
- [3. Cài đặt, cấu hình FTP và kiểm tra với lệnh ftp](#3-cài-đặt-cấu-hình-ftp-và-kiểm-tra-với-lệnh-ftp)
- [4. Mã hóa kết nối với SSL/TLS](#4-mã-hóa-kết-nối-với-ssl-tls)
- [5. Cài đặt, cấu hình SFTP và kiểm tra với lệnh sftp](#5-cài-đặt-cấu-hình-sftp-và-kiểm-tra-với-lệnh-sftp)
- [6. Thực hành sử dụng FileZilla](#6-thực-hành-sử-dụng-filezilla)
   - [Kiểm tra kết nối với FTPS](#kiểm-tra-kết-nối-với-ftps)
   - [Kiểm tra kết nối với SFTP](#kiểm-tra-kết-nối-với-sftp)

---

## 1. Giới thiệu về FTP

FTP (File Transfer Protocol) là một giao thức trong mô hình TCP/IP được dùng để truyền các file giữa các máy. FTP cho phép truyền nhận file và quản lý trực tuyến. FTP không cho phép truy xuất một máy khác để thực thi chương trình, nhưng nó rất tiện lợi cho việc thao tác với file.

FTP Server (File Transfer Protocol Server) là một loại phần mềm hoặc dịch vụ mạng được thiết kế để lưu trữ, quản lý và chia sẻ các tên tin hoặc thư mục thông qua Internet. Nó hoạt động dựa trên giao thức FTP, cho phép người dùng kết nối và truy cập vào các tài nguyên được chia sẻ từ xa.

---

## 2. Phân loại FTP Server

### **FTP (File Transfer Protocol)**

Loại này sử dụng giao thức FTP không mã hóa, khiến thông tin dễ bị đánh cắp qua mạng. Chính vì vậy tuy đơn giản, dễ sử dụng nhưng lại tiềm ẩn nguy cơ bảo mật cao do dữ liệu được truyền tải dưới dạng văn bản thuần túy, dễ xâm nhập bởi tin tặc. Vì lẽ đó mà FTP truyền thống thường phù hợp cho các trường hợp cần chia sẻ dữ liệu nội bộ không quá nhạy cảm hoặc tốc độ truyền tải là ưu tiên hàng đầu.

### **FTPS - FTP Secure**

FTPS mang đến mức độ bảo mật cao hơn FTP truyền thống bằng cách sử dụng các giao thức mã hóa như SSL (Secure Sockets Layer) hoặc TLS (Transport Layer Security) với mục đích bảo vệ dữ liệu khi thực hiện quá trình truyền tải. FTPS đảm bảo truy cập an toàn cho người dùng, ngăn chặn việc đánh cắp thông tin nhạy cảm.

### **SSH File Transfer Protocol - SFTP**

SFTP là hệ thống truyền tệp an toàn sử dụng giao thức SSH (Secure Shell) để truyền tải dữ liệu. Có thể nói, đây là một trong số các loại FTP Server cung cấp mức độ bảo mật cao nhất và được nhiều đơn vị tin dùng. SFTP mã hóa dữ liệu và gửi đi trong các gói nhị phân qua một kết nối bảo mật duy nhất bằng SSH, đảm bảo an toàn cho thông tin trong mọi trường hợp.

| **Tính năng** | **FTP (File Transfer Protocol)** | **FTPS (FTP over SSL/TLS)** | **SFTP (SSH File Transfer Protocol)** |
| --- | --- | --- | --- |
| **Bảo mật** | Không | Có (qua SSL/TLS) | Có (qua SSH) |
| **Mã hóa** | Không | Có (Thường chỉ dữ liệu) | Có (Dữ liệu & Lệnh) |
| **Giao thức nền** | TCP/IP | SSL/TLS | SSH |
| **Cổng mặc định** | 21 (Lệnh), 20 (Dữ liệu – Active Mode) | 21 (Lệnh), 990/989 (Implicit), Động (Explicit) | 22 (Thường dùng 1 cổng) |
| **Xác thực Server** | Không | Có (qua [Chứng chỉ SSL](https://interdata.vn/blog/ssl-la-gi/)/TLS) | Có (qua Host Key) |
| **Độ phức tạp FW** | Cao (nhiều cổng) | Cao (nhiều cổng, chế độ Explicit) | Thấp (thường chỉ cổng 22) |

### **So sánh giữa SFTP vs FTP**

Đây là sự so sánh quan trọng nhất về mặt bảo mật.

- **Bảo mật:** Điểm khác biệt lớn nhất. SFTP **an toàn** nhờ mã hóa qua SSH. FTP **không an toàn**, truyền mọi thứ dạng plain text, dễ bị nghe lén.
- **Cơ chế hoạt động:** SFTP là một phần của bộ giao thức SSH, chạy trên một kết nối SSH đã được mã hóa. FTP là một giao thức độc lập, chạy trực tiếp trên TCP/IP.
- **Cổng kết nối:** SFTP thường chỉ sử dụng một cổng duy nhất là **22** cho cả lệnh và dữ liệu. FTP sử dụng cổng **21** cho lệnh và một cổng khác (thường là 20 hoặc một cổng động) cho dữ liệu, gây khó khăn hơn cho cấu hình firewall.
- **Hiệu suất:** Do phải mã hóa/giải mã, SFTP có thể chậm hơn FTP một chút trong việc truyền file lớn. Tuy nhiên, sự đánh đổi này hoàn toàn xứng đáng vì lợi ích bảo mật.

### **So sánh giữa SFTP vs FTPS**

Cả SFTP và FTPS đều là các giao thức truyền file an toàn, nhưng chúng khác nhau về công nghệ nền tảng.

- **Giao thức bảo mật:** SFTP sử dụng **SSH** để bảo mật kết nối. FTPS sử dụng **SSL/TLS** (cùng công nghệ bảo mật với HTTPS của website) để mã hóa kết nối FTP.
- **Cách hoạt động:** SFTP là một giao thức hoàn toàn khác biệt được thiết kế từ đầu để chạy trên SSH. FTPS thực chất là giao thức FTP gốc được “bọc” thêm một lớp bảo mật SSL/TLS.
- **Cổng kết nối:** SFTP thường dùng cổng **22**. FTPS có hai chế độ: **Implicit** (thường dùng cổng 990 cho lệnh, 989 cho dữ liệu, kết nối SSL/TLS được thiết lập ngay từ đầu) và **Explicit** (dùng cổng 21 như FTP, sau đó nâng cấp lên SSL/TLS qua lệnh, thường cần mở nhiều cổng động trên firewall).
- **Xác thực máy chủ:** SFTP dùng SSH host key. FTPS dùng chứng chỉ SSL/TLS (giống như website).
- **Firewall:** SFTP thường dễ cấu hình firewall hơn do chỉ cần mở cổng 22. FTPS, đặc biệt là chế độ Explicit, có thể yêu cầu mở nhiều cổng hoặc cấu hình firewall phức tạp hơn.
- **Hỗ trợ:** Hầu hết các máy chủ và client hiện đại đều hỗ trợ cả SFTP và FTPS. Tuy nhiên, SFTP đôi khi được ưa chuộng hơn trong môi trường quản trị hệ thống Linux/Unix do gắn liền với SSH.

---

## 3. Cài đặt, cấu hình FTP và kiểm tra với lệnh ftp

Bước 1:  cập nhật danh sách nguồn gói hệ thống rồi cài đặt gói **VSFTPD** như sau:

```sql
sudo apt update
sudo apt install vsftpd
```

![image.png](/Images/tuan_2/image.png)

![image.png](/Images/tuan_2/image%201.png)

Bước 2: nếu bạn đã bật tường lửa UFW (nó không được bật theo mặc định) trên máy chủ, bạn phải mở cổng **21** và **20** nơi các trình nền FTP đang lắng nghe

```
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw status
```

![image.png](/Images/tuan_2/image%202.png)

Bước 3: Cấu hình và bảo mật máy chủ VsFTP trong Ubuntu

Tạo bản sao lưu của tệp cấu hình gốc **/etc/vsftpd/vsftpd.conf,** ở đây tôi chép file gốc vào /etc/vsftpd.conf.orig:

```powershell
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
```

Tiếp theo, hãy mở tệp cấu hình **vsftpd:**

```
sudo nano /etc/vsftpd.conf
```

Thêm/sửa đổi các tùy chọn sau với các giá trị sau:

```

listen=YES                         //vsftpd tự lắng nghe cổng 21 (standalone mode)
listen_ipv6=NO                     //Không dùng IPv6
anonymous_enable=NO                //Cấm đăng nhập ẩn danh
local_enable=YES                   //Cho phép user hệ thống đăng nhập
write_enable=YES                   //Cho phép ghi (upload, xóa...)
local_umask=022		                 //Quyền mặc định file upload là 644
dirmessage_enable=YES	             //Hiện nội dung file .message khi vào thư mục 
xferlog_enable=YES	               //Bật ghi log truyền file
connect_from_port_20=YES           //Dùng cổng 20 để gửi dữ liệu   
xferlog_std_format=YES             //Bật ghi log truyền file   
pam_service_name=vsftpd            //Xác thực qua PAM
chroot_local_user=YES              //Nhốt user trong thư mục home
allow_writeable_chroot=YES         //Cho phép ghi trong thư mục bị nhốt 	           
tcp_wrappers=YES                   //Hỗ trợ chặn IP qua hosts.allow/deny
userlist_enable=YES                //Dùng danh sách user
userlist_file=/etc/vsftpd.userlist //File chứa user được phép
userlist_deny=NO                   //Chỉ user có trong danh sách mới được login
pasv_min_port=35000                //Dải cổng dùng cho passive mode FTP
pasv_max_port=40000
ssl_enable=NO                       //Chưa cần mã hóa ssl
pam_service_name=vsftpd
```

Lưu file và đóng nó lại.

Bước 4: Cấu hình thư mục người dùng

 Đầu tiên là tạo người dùng mới:

```powershell
sudo adduser th29k03
```

![image.png](/Images/tuan_2/image%203.png)

Thêm người dùng mới tạo vào danh sách người dùng của FTP:

```powershell
 echo "th29k03" | sudo tee -a /etc/vsftpd.userlist
```

![image.png](/Images/tuan_2/image%204.png)

**Tạo thư mục FTP cho người dùng:**Thư mục `/ftp` này chỉ dùng để chứa thư mục `upload` bên trong, không có quyền ghi trực tiếp

```
sudo mkdir /home/th29k03/ftp
sudo chown nobody:nogroup /home/th29k03/ftp
sudo chmod a-w /home/th29k03/ftp
```

**Tạo thư mục upload có quyền ghi:** Người dùng FTP cần một thư mục để có thể tải file lên. Tạo thư mục `upload` bên trong và gán đúng quyền sở hữu:

```
sudo mkdir /home/th29k03/ftp/upload
sudo chown th29k03:th29k03 /home/th29k03/ftp/upload
```

**Kiểm tra cấu trúc và quyền thư mục:** Sau khi hoàn tất, bạn có thể kiểm tra lại để chắc chắn cấu trúc và phân quyền thư mục đã đúng:

```
sudo ls -al /home/th29k03/ftp
```

![image.png](/Images/tuan_2/image%205.png)

**Tạo file test để kiểm tra:** Cuối cùng, bạn có thể tạo một file mẫu để xác nhận khả năng ghi file hoạt động bình thường:

```
echo "vsftpd test file" | sudo tee /home/th29k03/ftp/upload/test.txt
```

![image.png](/Images/tuan_2/image%206.png)

Bước 5: Kiểm tra kết nối FTP.

Ta thử kết nối với 1 tài khoản ẩn danh anonymous:

![image.png](/Images/tuan_2/image%207.png)

Như bên trên, người dùng bất kì không thể đăng nhập vào FTP Server. Ta thử lại với người dùng mới tạo:

![image.png](/Images/tuan_2/image%208.png)

Với người dùng được cấu hình thì bạn có thể đăng nhập thành công. Thực hiện tải xuống file test.txt vừa tạo:

![image.png](/Images/tuan_2/image%209.png)

Tiếp tục thực hiện đổi tên file test.txt để xác nhận người dùng được tạo có quyền write:

![image.png](/Images/tuan_2/image%2010.png)

Cuối cùng đóng kết nối:

![image.png](/Images/tuan_2/image%2011.png)

---
## 4. Mã hóa kết nối với SSL/TLS

Để có thể mã hoá kết nối FTP, bạn cần phải có một chứng chỉ SSL và cấu hình vsftpd sử dụng nó. Ở đây mình sẽ sử dụng openssl để tạo chứng chỉ như sau:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/private/vsftpd.pem \
-out /etc/ssl/private/vsftpd.pem
```

Sau khi có chứng chỉ, bạn mở file cấu hình của vsftpd:

```
sudo nano /etc/vsftpd.conf
```

Bỏ dấu `#` hoặc thêm 2 dòng sau để kích hoạt chứng chỉ SSL:

```
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem
```

Đảm bảo rằng dòng `ssl_enable` được bật:

```
ssl_enable=YES
```

Thêm vào các dòng dưới đây để tăng cường bảo mật cho kết nối SSL/TLS:

```
allow_anon_ssl=NO  
force_local_data_ssl=YES  
force_local_logins_ssl=YES  
ssl_tlsv1=YES  
ssl_sslv2=NO  
ssl_sslv3=NO  
require_ssl_reuse=NO  
ssl_ciphers=HIGH
```

 Sau khi chỉnh sửa lile /etc/vsftpd.conf sẽ có dạng như này:

![image.png](/Images/tuan_2/image%2012.png)

Cuối cùng, bạn lưu lại file cấu hình và khởi động lại dịch vụ vsftpd để áp dụng thay đổi:

```
sudo systemctl restart vsftpd
```
---

## 5. Cài đặt, cấu hình SFTP và kiểm tra với lệnh sftp

Để đảm bảo rằng các gói OpenSSH đã được cài đặt trên hệ thống Linux của bạn, hãy sử dụng lệnh sau. Đối với máy chủ Debian hoặc Ubuntu, bạn có thể sử dụng lệnh dpkg bên dưới.

```
dpkg -l | grep ssh
```

![image.png](/Images/tuan_2/image%2013.png)

Cột đầu tiên ***ii*** có nghĩa là gói đã được cài đặt. Gói *openssh-sftp-server* được cài đặt trên hệ thống Ubuntu.
Bây giờ ta đi vào cấu hình để sử dụng SFTP

**Bước 1: Tạo nhóm và người dùng**

Tạo một nhóm và người dùng mới cho máy chủ SFTP. Người dùng trong nhóm này sẽ được phép truy cập máy chủ SFTP. Và vì lý do bảo mật, người dùng SFTP không thể truy cập dịch vụ SSH. Người dùng SFTP chỉ truy cập máy chủ SFTP.

Thực hiện lệnh sau để tạo một nhóm mới *sftpgroup*.

```
sudo groupadd sftpgroup
```

Tạo người dùng mới *sftpuser* bằng lệnh sau.

```
sudo useradd -G sftpgroup -d /srv/sftpuser -s /sbin/nologin sftpuser
```

Tùy chọn chi tiết:

- **G** : tự động thêm người dùng vào *sftpgroup*.
- **d** : chỉ định thư mục chính cho người dùng mới.
- **s** : đặt mặc định cho người dùng mới thành */sbin/nologin*, nghĩa là người dùng *không thể* truy cập Máy chủ SSH.

Tiếp theo, tạo mật khẩu cho người dùng *sftpuser* bằng lệnh bên dưới.

```
passwd sftpuser
```

![image.png](/Images/tuan_2/image%2014.png)

**Bước 2: Tạo và định cấu hình thư mục chroot cho người dùng SFTP:**

Đối với người dùng sftpuser, thư mục chính mới sẽ ở */srv/sftpuser*. Thực hiện lệnh dưới đây để tạo nó.

```
mkdir -p /srv/sftpuser
```

Thay đổi quyền sở hữu của thư mục thành người dùng root bằng lệnh sau.

```
sudo chown root /srv/sftpuser
```

Cho phép nhóm đọc và thực thi, nhưng không cho phép viết.

```
sudo chmod g+rx /srv/sftpuser
```

![image.png](/Images/tuan_2/image%2015.png)

Tiếp theo, tạo một thư mục dữ liệu mới bên trong thư mục */srv/sftpuser* và thay đổi quyền sở hữu thư mục *data* đó thành người dùng *sftpuser*.

```
mkdir -p /srv/sftpuser/data
chown sftpuser:sftpuser /srv/sftpuser/data
```

Sau đó bạn có thể kiểm tra lại thư muc và quyền của các thư mục đã tạo:

```
ls -lah /srv/
ls -lah /srv/sftpuser/
```

![image.png](/Images/tuan_2/image%2016.png)

![image.png](/Images/tuan_2/image%2017.png)

Tới bước này, cấu hình chi tiết bên dưới cho thư mục người dùng SFTP.

- Thư mục */srv/sftuser* là thư mục chính mặc định.
- Người dùng *sftpuser* *không thể* ghi vào thư mục */srv/sftpuser*, nhưng có thể đọc bên trong thư mục đó.
- Người dùng *sftpuser* có thể tải tệp lên máy chủ SFTP tại thư mục */srv/sftpuser/data*.

**Bước 3: Kích hoạt SFTP trên máy chủ SSH**

Để bật máy chủ SFTP trên OpenSSH, bạn phải chỉnh sửa cấu hình SSH /etc/ssh/sshd_config.

Chỉnh sửa cấu hình ssh */etc/ssh/sshd_config* bằng nano hoặc vim.

```
sudo nano /etc/ssh/sshd_config
```

Dán cấu hình sau vào cuối dòng.

```
Subsystem sftp internal-sftp
Match Group sftpgroup
     ChrootDirectory %h     
     X11Forwarding no     
     AllowTCPForwarding no     
     ForceCommand internal-sftp
```

Chi tiết như sau:

- **Subsystem sftp internal-sftp** : sử dụng trình SFTP nội bộ của SSH.
- **Match Group sftpgroup** : áp dụng cấu hình cho nhóm người dùng `sftpgroup`.
- **ChrootDirectory %h** : nhốt người dùng vào thư mục home của họ, thư mục phải do root sở hữu.
- **X11Forwarding no** : tắt chuyển tiếp X11 (đồ họa) để tăng bảo mật.
- **AllowTCPForwarding no** : chặn chuyển tiếp cổng TCP qua SSH.
- **ForceCommand internal-sftp** : ép người dùng chỉ được sử dụng SFTP, không truy cập shell.

![image.png](/Images/tuan_2/image%2018.png)

Lưu cấu hình và thoát, sau đó để áp dụng cấu hình mới, hãy khởi động lại dịch vụ ssh bằng lệnh bên dưới.

```
sudo systemctl restart sshd
sudo systemctl status sshd
```

![image.png](/Images/tuan_2/image%2019.png)

**Bước 4: kiểm tra kết nối SFTP vừa tạo**

Để kết nối với máy chủ SFTP, hãy thực hiện lệnh sftp như bên dưới .

```
sudo sftp username@IP
```

![image.png](/Images/tuan_2/image%2020.png)

Khi bạn đã kết nối với máy chủ SFTP, hãy thực hiện lệnh sau.

Hiển thị thư mục làm việc của đường dẫn hiện tại và liệt kê tất cả các tệp và thư mục có sẵn.

```
pwd
ls
```

![image.png](/Images/tuan_2/image%2021.png)

Thử tải lên 1 file từ máy lên SFTP server nhưng ngoài như mục cho phép với lệnh:

```
put /path/to/file/on/local /
```

![image.png](/Images/tuan_2/image%2022.png)

Như kết quả sẽ ko cho phép tải lên dữ liệu ở vùng không cho phép do bị giới hạn quyền

Tiếp theo tải 1 file từ máy lên SFTP server và tải vào vùng cho phép và kiểm tra lại bằng lệnh :

```
put /path/to/file/on/local /data/
ls /data/
```

![image.png](/Images/tuan_2/image%2023.png)

Tới đây ta đã hoàn thành cấu hình và kiểm tra SFTP server sau khi cấu hình thành công.

---

## 6. Thực hành sử dụng FileZilla

- **Kiểm tra kết nối với FTPS**
    
    Sau khi mã hóa SSL/TLS thì chúng ta không còn sử dụng  **được để test** nữa bởi vì lệnh `ftp` là client FTP truyền thống, **không hỗ trợ mã hóa SSL/TLS**. Khi bạn bật SSL/TLS trong vsftpd (`ssl_enable=YES`), máy chủ yêu cầu client phải **khởi tạo kết nối bảo mật**, nhưng lệnh `ftp` lại không biết làm điều đó → bị từ chối.
    
    ![image.png](/Images/tuan_2/image%2024.png)
    
    Ta có thể test thông qua FileZilla có thể tải [ở đây](https://filezilla-project.org/download.php)
    
    Sau khi tải và cài đặt xong thì bạn mở Filezilla và chọn **File** > **Site Manager** ở góc trên bên trái.
    
    ![image.png](/Images/tuan_2/image%2025.png)
    
    Hoặc nhấn vào biểu tượng khoanh đỏ dưới đây
    
    ![Untitled_____.png](/Images/tuan_2/Untitled_____.png)
    
    Sau đó chọn My site > nhấn New site
    
    ![new_site.png](/Images/tuan_2/new_site.png)
    
    Chọn site vừa tạo và thực hiện theo các bước đánh đánh số:
    
    Ở mục Protocol chọn FTP - File Tranfer Protocol, nhập IP của FTP server ( ví dụ 192.168.0.100), chọn Require explicit FTP over TLS ở mục Encryption, chọn Logon Type Normal và nhập tài khoản FTP server đã tạo → cuối cùng nhấn Connect
    
    ![genaral_newsite.png](/Images/tuan_2/genaral_newsite.png)
    
    Nếu thành công thì sẽ hiện những dòng log Status như sau:
    
    ![image.png](/Images/tuan_2/image%2026.png)
    
    Và ở góc bên trái của màn hình FileZilla có thông tin thư mục và tệp tin tương ứng của tài khoản FTP server:
    
    ![image.png](/Images/tuan_2/image%2027.png)
    
    Như kết quả thu được bao gồm file test.txt và file upload.txt mà ta đã thêm từ trước.
    
    Ta cũng có thể upload file từ máy thật qua FTP server ảo bằng cách kéo file từ máy thật qua:
    
    ![image.png](/Images/tuan_2/image%2028.png)
    Tới đây đã hoàn thành sử dụng FileZilla để kết nối FTPS
    <br>
- **Kiểm tra kết nối với SFTP**
    
    Chúng ta vẫn cài đặt FileZilla và tạo mới Site Manager như đã làm ở Mục Kiểm tra kết nối với FTPS nhưng ở phần cấu hình cho New site ta sẽ thay Protocol thành SFTP - SSH File Transfer Protocol và đang nhập tài khoản đã tạo ở SFTP → Connect:
    
    ![image.png](/Images/tuan_2/image%2029.png)
    
    Sau khi kết nối thành công sẽ hiện log thông báo và thư mục tương ứng của tài khoản SFTP đăng nhập:
    
    ![image.png](/Images/tuan_2/image%2030.png)
    
    Tới đây là hoàn thành kiểm tra kết nối SFTP với FileZilla.
    
    ---
    THE END