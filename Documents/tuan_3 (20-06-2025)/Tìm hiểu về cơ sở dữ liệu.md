# Tìm hiểu về cơ sở dữ liệu

# Mục lục


  - [1. Giới thiệu về cơ sở dữ liệu](#1-giới-thiệu-về-cơ-sở-dữ-liệu)
    - [Khái niệm](#khái-niệm)
    - [Phân loại cơ sở dữ liệu](#phân-loại-cơ-sở-dữ-liệu)
  - [2. Cấu hình và quản lý cơ sở dữ liệu](#2-cấu-hình-và-quản-lý-cơ-sở-dữ-liệu)
    - [Cài đặt và cấu hình MySQL](#cài-đặt-và-cấu-hình-mysql)
    - [Cài đặt và cấu hình cụ thể 1 phiên bản MariaDB](#cài-đặt-và-cấu-hình-cụ-thể-1-phiên-bản-mariadb)
    - [Quản lý dịch vụ MySQL](#quản-lý-dịch-vụ-mysql)
    - [Thao tác cơ bản với MariaDB](#thao-tác-cơ-bản-với-mariadb)
  - [3. Tối ưu hóa cơ sở dữ liệu](#3-tối-ưu-hóa-cơ-sở-dữ-liệu)
    - [Lưu thông tin truy vấn chậm (Slow Query Log)](#lưu-thông-tin-truy-vấn-chậm-slow-query-log)
    - [Caching](#caching)
  - [4. Backup và restore cơ sở dữ liệu](#4-backup-restore-cở-sở-dữ-liệu)
    - [Backup dữ liệu](#backup-dữ-liệu)
    - [Restore dữ liệu](#restore-dữ-liệu)

## 1. Giới thiệu về cơ sở dữ liệu

### Khái niệm

**Cơ sở dữ liệu (Database)** là một tập hợp dữ liệu có cấu trúc, được tổ chức một cách hệ thống để dễ dàng truy cập, quản lý và cập nhật. Hiểu đơn giản, CSDL là một kho lưu trữ thông tin điện tử, cho phép người dùng tìm kiếm, sắp xếp và phân tích dữ liệu một cách hiệu quả.

Ví dụ, thư viện có thể được xem như một cơ sở dữ liệu đơn giản, nơi sách được phân loại theo chủ đề để dễ dàng tìm kiếm.

Mục đích của việc sử dụng CSDL là để quản lý thông tin hiệu quả, đảm bảo tính nhất quán và cho phép truy xuất dữ liệu nhanh chóng khi cần thiết.

### Phân loại cơ sở dữ liệu

Có nhiều dạng cơ sở dữ liệu khác nhau, trong đó các loại phổ biến bao gồm:

- Cơ sở dữ liệu liên kết (Relational Database): Đây là loại cơ sở dữ liệu phổ biến nhất và được rộng rãi sử dụng trong các ứng dụng doanh nghiệp. Cơ sở dữ liệu liên kết được tổ chức thành các bảng, trong đó mỗi bảng đại diện cho một đối tượng hoặc một loại dữ liệu cụ thể.
- Cơ sở dữ liệu phi liên kết (Non-Relational Database): Đây là loại cơ sở dữ liệu được thiết kế để lưu trữ các dữ liệu không có mối quan hệ với nhau. Cơ sở dữ liệu phi liên kết thường được sử dụng trong các ứng dụng web, ứng dụng di động và ứng dụng IoT.
- Cơ sở dữ liệu đối tượng (Object-oriented Database): Đây là loại cơ sở dữ liệu được thiết kế để lưu trữ đối tượng và thuộc tính của chúng. Cơ sở dữ liệu đối tượng thường được sử dụng trong các ứng dụng quản lý tài nguyên và các ứng dụng phân tích dữ liệu.

## 2. Cấu hình và quản lý cơ sở dữ liệu

### Cài đặt và cấu hình MySQL

**Bước 1: Cài đặt MySQL client**

![image.png](/Images/tuan_3_csdl/image.png)

Kiểm tra phiên bản MySQL client để xác minh xem quá trình cài đặt có thành công hay không:

```
mysql -V
```

![image.png](/Images/tuan_3_csdl/image%201.png)

```
mysql -u <username> -p <password>-h HOSTNAME_OR_IP
```

![image.png](/Images/tuan_3_csdl/image%202.png)

Bây giờ bạn có thể sử dụng lệnh sau để thiết lập kết nối từ xa với máy chủ MySQL:

**Bước 2: Cài đặt máy chủ MySQL**

Sử dụng lệnh apt để cập nhật các gói hệ thống từ kho lưu trữ như sau:

```sql
sudo apt update
```

Bây giờ sử dụng lệnh sau để cài đặt gói máy chủ MySQL:

```
sudo apt install mysql-server -y
```

![image.png](/Images/tuan_3_csdl/image%203.png)

Sau khi cài đặt hoàn tất, dịch vụ MySQL sẽ tự động khởi động. Để kiểm tra xem máy chủ MySQL có đang chạy hay không, hãy dùng lệnh sau đây:

```
sudo systemctl status mysql
```

![image.png](/Images/tuan_3_csdl/image%204.png)

**Bước 3: Cấu hình MySQL**

MySQL đi kèm với một tập lệnh là **mysql_secure_installation** có thể thực hiện một số hoạt động liên quan đến bảo mật. Để sử dụng các bạn chạy lệnh sau:

```
sudo mysql_secure_installation
```

Bây giờ nó sẽ hỏi các câu hỏi tiếp theo:

1. Để xóa người dùng thử nghiệm ẩn danh
2. Vô hiệu hóa đăng nhập từ xa từ người dùng root
3. Xóa cơ sở dữ liệu kiểm tra
4. Tải lại bảng đặc quyền để lưu tất cả các thay đổi

![image.png](/Images/tuan_3_csdl/image%205.png)

![image.png](/Images/tuan_3_csdl/image%206.png)

**Bước 4: Điều chỉnh xác thực người dùng**

Để đảm bảo xác thực người dùng qua mật khẩu, bạn có thể thay đổi plugin từ "**auth_socket**" thành "**mysql_native_password**." Với mục đích này, hãy mở màn hình nhắc MySQL bằng cách sử dụng lệnh **sudo mysql** và xác minh plugin đang được sử dụng để xác thực người dùng, như sau:

```
SELECT user, authentication_string, plugin, host FROM mysql.user;
```

![image.png](/Images/tuan_3_csdl/image%207.png)

Như kết quả ở trên thì đối với tài khoản đăng nhập root tự xác thực bằng cách sử dụng plugin "auth_socket". Để khởi tạo xác thực root bằng mật khẩu, ta câu lệnh ALTER USER để đặt plugin "mysql_native_password"

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_passwd';
```

Lưu lại những thay đổi trên:

```
FLUSH PRIVILEGES;
```

Cuối cùng là thoát khỏi màn hình làm việc MySQL bằng lệnh:

```
exit
```

![image.png](/Images/tuan_3_csdl/image%208.png)

Kiểm tra lại xe đổi thành công plugin chưa:

```
SELECT user, authentication_string, plugin, host FROM mysql.user;
```

![image.png](/Images/tuan_3_csdl/image%209.png)

Sau khi đổi phương plugin thành công thì ta muốn đăng nhập và sử dụng MySQL phải cần nhập user và mật khẩu tương ứng:

![image.png](/Images/tuan_3_csdl/image%2010.png)

### Cài đặt và cấu hình cụ thể 1 phiên bản MariaDB

Ở ví dụ này, mình thực hành cài **MariaDB** version 10.6, đây là phiên bản không sẳn trong `apt`

```
root@thienduong:/home/thienduong# apt-cache policy mariadb-server
mariadb-server:
  Installed: None
  Candidate: 1:10.3.39-0ubuntu0.20.04.2
  Version table:
    1:10.3.39-0ubuntu0.20.04.2 500
            500 http://vn.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages
            500 http://vn.archive.ubuntu.com/ubuntu focal-updates/universe i386 Packages
            500 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages
            500 http://security.ubuntu.com/ubuntu focal-security/universe i386 Packages
        1:10.3.22-1ubuntu1 500
            500 http://vn.archive.ubuntu.com/ubuntu focal/universe amd64 Packages
            500 http://vn.archive.ubuntu.com/ubuntu focal/universe i386 Packages
```

**Bước 1: Cài đặt các công cụ cần thiết**

Chạy lệnh này để đảm bảo hệ thống có thể tải gói từ địa chỉ HTTPS:

```bash
sudo apt-get install apt-transport-https curl
```

**Bước 2: Tạo thư mục lưu khóa xác thực**

Tạo thư mục để lưu key xác thực của MariaDB:

```bash
sudo mkdir -p /etc/apt/keyrings
```

**Bước 3: Tải khóa xác thực MariaDB về**

Tải file `mariadb-keyring.pgp` (dùng để xác minh gói MariaDB là an toàn):

```bash
sudo curl -o /etc/apt/keyrings/mariadb-keyring.pgp 'https://mariadb.org/mariadb_release_signing_key.pgp'
```

**Bước 4: Tạo file cấu hình kho phần mềm MariaDB**

Tạo một file repo mới, ví dụ: `/etc/apt/sources.list.d/mariadb.list`, bằng lệnh:

```bash
sudo nano /etc/apt/sources.list.d/mariadb.list
```

Rồi dán nội dung sau vào (dành cho MariaDB 10.6 và Ubuntu 20.04 - *focal*):

```bash
deb [signed-by=/etc/apt/keyrings/mariadb-keyring.pgp] https://vn-mirrors.vhost.vn/mariadb/repo/10.6/ubuntu focal main
```

Nếu như bạn muốn cài đặt version khác thì có thể thay thế dòng này và chỉnh lại version cần cài.

**Bước 5: Cập nhật lại danh sách gói**

Sau khi thêm repo mới, cần cập nhật lại:

```bash
sudo apt-get update
```

**Bước 6: Cài đặt MariaDB**

Giờ bạn đã có thể cài MariaDB 10.6:

```bash
sudo apt-get install mariadb-server
```

![image.png](/Images/tuan_3_csdl/image%2011.png)

Kiểm tra lại bằng lệnh:

```bash
mariadb -V
```

![image.png](/Images/tuan_3_csdl/image%2012.png)

**Bước 7: Cấu hình MariaDB**

Tương tự MySQL, chúng ta chạy lệnh dưới đây để cấu hình bảo mật cho MariaDB:

```bash
sudo mysql_secure_installation
```

Tiếp đó cấu hình tương tự như trên MySQL đã thực hiện, có thể tham khảo như sau:

![image.png](/Images/tuan_3_csdl/image%2013.png)

![image.png](/Images/tuan_3_csdl/image%2014.png)

Sau khi cấu hình ban đầu cho MariDB, ta truy cập vào cơ sở dữ liệu bằng user root và mật khẩu vừa tạo và thao tác với cơ sở dữ liệu :

```bash
sudo mysql -u root -p
```

![image.png](/Images/tuan_3_csdl/image%2015.png)

### Quản lý dịch vụ MySQL

Đặt dịch vụ chạy khi khởi động máy chủ Ubuntu bằng cách sử dụng lệnh :

```
sudo systemctl enable mysql
```

Ngoài ra, bạn có thể kiểm tra xem máy chủ có hoạt động hay không bằng cách gõ:

```
sudo systemctl status mysql
```

Dừng chạy tạm thời dịch vụ MySQL

```
sudo systemctl stop mysql
```

### Thao tác cơ bản với MariaDB

**Để xem các bảng cơ sở dữ liệu có sẵn trong MariaDB như sau :**

```
show databases;
```

![image.png](/Images/tuan_3_csdl/image%2016.png)

**Để tạo 1 cơ sở dữ liệu mới , thực hiện câu lệnh sau :**

```
create database <Tên cơ sở dữ liệu>;
```

![image.png](/Images/tuan_3_csdl/image%2017.png)

**Tải lên 1 cơ sở dữ liệu có sẵn**

Ví dụ mình muốn tải lên 1 cơ sở dữ liệu có sẵn của `mysqltutorial.org`, ta sẽ làm như sau :

- Tải về file zip của cơ sở dữ liệu mẫu :
    
    ```
    cd / 
    wget https://www.mysqltutorial.org/wp-content/uploads/2018/03/mysqlsampledatabase.zip
    ```
    
    ![image.png](/Images/tuan_3_csdl/image%2018.png)
    

- Sau khi tải về ta có file zip với dạng như sau :
    
    ```
    mysqlsampledatabase.zip
    ```
    
    ![image.png](/Images/tuan_3_csdl/image%2019.png)
    

- Giải nén file zip
    
    ```
    unzip mysqlsampledatabase.zip
    ```
    
- Sau khi giải nén ta sẽ được file có đuôi là .sql
    
    ```
    mysqlsampledatabase.sql
    ```
    
    ![image.png](/Images/tuan_3_csdl/image%2020.png)
    

- Truy cập MariaDB và sử dụng câu lệnh sau:
    
    ```
    source /root/mysqlsampledatabase.sql;
    ```
    
    ![image.png](/Images/tuan_3_csdl/image%2021.png)
    

- Kiểm tra lại cơ sở dữ liệu mẫu vừa tải lên :
    
    ```
    show databases;
    ```
    
    ![image.png](/Images/tuan_3_csdl/image%2022.png)
    

## 3. Tối ưu hóa cơ sở dữ liệu

### **Lưu thông tin truy vấn chậm (Slow Query Log)**

Logging query chậm có thể giúp bạn xác định các cơ sỡ dữ liệu và và debug.

Hiển thị thông tin cấu hình của tính năng Slow Query Log:

```bash
SHOW VARIABLES LIKE 'slow_query_log';
SHOW VARIABLES LIKE 'slow_query_log_file';
SHOW VARIABLES LIKE 'long_query_time';
```

Trong đó:

- `slow_query_log` : nếu “ON” thì tính năng lưu truy vấn chậm này đang được bật và ngươc lại
- `slow_query_log_file`: đường dẫn file log này
- `long_query_time`: nếu như thời gian của lệnh truy vấn lớn hơn `long_query_time` thì sẽ tiến hành ghi lại truy vấn

Để xem file log theo thời gian thật, ở đây file log tôi lưu ở /var/lib/mysql/thienduong-slow.log thì :

```bash
sudo tail -f /var/lib/mysql/thienduong-slow.log
```

![image.png](/Images/tuan_3_csdl/image%2023.png)

### Caching

Bộ đệm truy vấn lưu trữ văn bản của câu lệnh select cùng với kết quả tương ứng được gửi đến máy khách. Nếu một câu lệnh giống hệt được nhận sau đó, máy chủ sẽ lấy kết quả từ bộ đệm truy vấn thay vì phân tích cú pháp và thực hiện lại câu lệnh. Bộ đệm truy vấn được chia sẻ giữa các phiên, do đó, một tập kết quả được tạo bởi một khách hàng có thể được gửi để đáp lại cùng một truy vấn do một khách hàng khác đưa ra.
Bộ đệm truy vấn có thể hữu ích trong môi trường nơi bạn có các bảng không thay đổi thường xuyên và máy chủ nhận được nhiều truy vấn giống nhau. Đây là một tình huống điển hình cho nhiều máy chủ Web tạo ra nhiều trang động dựa trên nội dung cơ sở dữ liệu.
Bộ đệm truy vấn không trả về dữ liệu cũ. Khi các bảng được sửa đổi, mọi mục nhập có liên quan trong bộ đệm truy vấn sẽ bị xóa.
Điều này cực kỳ hữu ích trong môi trường đọc cao, viết thấp (như hầu hết các trang web đặc biệt là các trang tin tức). Nó không mở rộng tốt trong các môi trường có I/O  cao trên các server đa lõi (multi-core ), do đó, nó bị tắt theo mặc định.

Để kiêm tra máy chủ cơ sở dữ liệu có hỗ trợ query cache hay không?  ta có thể kiểm tra với lệnh sau:

```
MariaDB [(none)]> SHOW VARIABLES LIKE 'have_query_cache';
```

![image.png](/Images/tuan_3_csdl/image%2024.png)

Nếu giá trí là “YES” thì có hỗ trợ query cache.

Mặc định thì tính năng query cache này được tắt đi. để bật tính năng này lên thì ta mở file cấu hình :

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Thêm những dòng này vào file cấu hình:

```bash
query_cache_type = 1
query-cache-size = 10M
query_cache_limit =2M
```

Trong đó:

- `query_cache_type` = 1 : để bật tính năng query cache lên, 0 nếu muốn tắt đi
- `query-cache-size`:  biến quy định kích thước của cache
- `query_cache_limit`: Kích thước mà kết quả lớn hơn mức này không được lưu trữ trong bộ đệm truy vấn.

## 4. Backup, restore cở sở dữ liệu
### Backup dữ liệu

**Cài đặt gói cần thiết cho viêc backup và restore MariaDB:**

```bash
sudo apt install mariadb-backup
```

![image.png](/Images/tuan_3_csdl/image%2025.png)

Tạo folder để lưu các file backup:

```bash
sudo mkdir -p /var/mariadb/backup
```

Phân quyền cho folder này:

```bash
sudo chown mysql:mysql /var/mariadb/backup
```

![image.png](/Images/tuan_3_csdl/image%2026.png)

Tiến hành backup vào folder vừa tạo:

```bash
mariadb-backup --backup \
 --target-dir=/var/mariadb/backup/ \
--user=username --password=your_passwd
```

![image.png](/Images/tuan_3_csdl/image%2027.png)

Tới đây là ta hoàn thành backup dữ liệu trong MariaDB

### Restore dữ liệu

Chuẩn bị dữ liệu để bước tiếp restore:

```bash
mariadb-backup --prepare \
--target-dir=/var/mariadb/backup/
```

![image.png](/Images/tuan_3_csdl/image%2028.png)

Tiến hành restore dữ liệu:

```bash
mariadb-backup --copy-back  --target-dir=/var/mariadb/backup/
```

![image.png](/Images/tuan_3_csdl/image%2029.png)

---
THE END