# Tìm hiểu về WordPress
## Mục lục
- [Tìm hiểu về WordPress](#tìm-hiểu-về-wordpress)
  - [Giới thiệu về WordPress](#giới-thiệu-về-wordpress)
  - [Cài đặt WordPress](#cài-đặt-wordpress)
  - [Cài đặt LAMP](#cài-đặt-lamp)
  - [Cài đặt WordPress (chi tiết)](#cài-đặt-wordpress-1)
- [Quản lý Theme](#quản-lý-theme)
  - [Giới thiệu về Theme WordPress](#giới-thiệu-về-theme-wordpress)
  - [Phân loại Theme WordPress](#phân-loại-theme-wordpress)
  - [Cài đặt Theme WordPress](#cài-đặt-theme-wordpress)
- [Quản lý Plugin](#quản-lý-plugin)
  - [Giới thiệu về Plugin WordPress](#giới-thiệu-về-plugin-wordpress)
  - [Cài đặt Plugin WordPress](#cài-đặt-plugin-wordpress)
- [Tối ưu hóa bảo mật](#tối-ưu-hóa-bảo-mật)
  - [Cập nhật thường xuyên](#cập-nhật-wordpress-theme-và-plugin-thường-xuyên)
  - [Thay đổi thông tin mặc định](#thay-đổi-các-thông-tin-mặc-định-của-wordpress)
  - [Phân quyền với htaccess](#phân-quyền-bảo-vệ-file-và-thư-mục-với-htaccess)
  - [Cấu hình SSH Key và login passwd](#cấu-hình-ssh-key-và-login-passwd)
  - [Cấu hình thêm Firewall](#cấu-hình-thêm-firewall)
  - [Cài đặt thêm ứng dụng scan mã độc](#cài-đặt-thêm-ứng-dụng-scan-mã-độc)


# Giới thiệu về WordPress

**WordPress** là một phần mềm nguồn mở dùng để tạo và quản lý nội dung website. Nó hoạt động như một hệ thống quản lý nội dung (CMS) cho phép người dùng tạo, chỉnh sửa, đăng và quản lý các bài viết, trang web một cách đơn giản thông qua giao diện trực quan.

Điểm mạnh chính của WordPress là nguồn mã nguồn mở miễn phí, cho phép bất kỳ ai cũng có thể sử dụng, chỉnh sửa và phát triển. Nó cũng có hệ sinh thái plugin và giao diện (theme) phong phú, tùy biến dễ dàng theo nhu cầu. Cộng đồng người dùng và nhà phát triển rất lớn, luôn cập nhật các tính năng mới.

# Cài đặt WordPress

Trước tiên phải cài đặt LAMP/LEMP Stack, trong ví dụ này mình sẽ cài LAMP để thực hành

### Cài đặt LAMP

**Bước 1: Cài đặt Apache và cập nhật Firewall**

Bắt đầu bằng cách cập nhật bộ nhớ cache của trình quản lý gói:

```bash
sudo apt update
```

Tiếp theo, cài đặt Apache với:

```bash
sudo apt install apache2
```

Sau khi cài đặt hoàn tất, bạn cần điều chỉnh cài đặt firewall để cho phép lưu lượng HTTP. Công cụ cấu hình firewall mặc định của Ubuntu được gọi là **Uncomplicated Firewall (UFW)**. Nó có các hồ sơ ứng dụng khác nhau mà bạn có thể sử dụng. Để liệt kê tất cả các hồ sơ ứng dụng UFW hiện có, hãy thực thi lệnh sau:

```bash
sudo ufw app list
```

Output

```bash
Available applications:
Apache
Apache Full
Apache Secure
OpenSSH
```

Dưới đây là ý nghĩa của từng hồ sơ:

● **Apache**: Hồ sơ này chỉ mở cổng 80 (lưu lượng web không mã hóa, bình thường).

● **Apache Full**: Hồ sơ này mở cả cổng 80 (lưu lượng web không mã hóa) và cổng 443 (lưu lượng được mã hóa TLS/SSL).

● **Apache Secure**: Hồ sơ này chỉ mở cổng 443 (lưu lượng được mã hóa TLS/SSL).

Hiện tại, tốt nhất là chỉ cho phép các kết nối qua cổng 80, vì đây là cài đặt Apache mới và bạn chưa cấu hình chứng chỉ TLS/SSL để phục vụ lưu lượng HTTPS trên máy chủ.

Để chỉ cho phép lưu lượng qua cổng 80, sử dụng hồ sơ **Apache**:

```bash
sudo ufw status
```

Xác minh sự thay đổi với:

```bash
sudo ufw status
```

Output

```bash
Status: active
```

```bash
To                         Action      From

- - ------ ----

OpenSSH                    ALLOW       Anywhere

Apache                     ALLOW       Anywhere

OpenSSH (v6)               ALLOW       Anywhere (v6)

Apache (v6)                ALLOW       Anywhere (v6)
```

Lưu lượng qua cổng 80 hiện đã được phép qua firewall.

Bạn có thể kiểm tra ngay bằng cách truy cập địa chỉ IP công cộng của máy chủ trong trình duyệt (tham khảo lưu ý dưới tiêu đề “Cách Tìm Địa Chỉ IP Công Cộng của Máy Chủ” nếu bạn chưa biết địa chỉ IP của mình):

```bash
http://your_server_ip
```

**Bước 2 – Cài đặt MySQL**

sử dụng apt để tải và cài đặt phần mềm này:

```bash
sudo apt install mysql-server
```

Khi được nhắc, hãy xác nhận cài đặt bằng cách nhập **Y** và nhấn **ENTER**.

Vì script `mysql_secure_installation` thực hiện nhiều thao tác hữu ích để bảo mật cài đặt MySQL, vẫn được khuyến nghị chạy nó trước khi bạn bắt đầu sử dụng MySQL để quản lý dữ liệu. 

Tuy nhiên, để tránh bước vào vòng lặp đệ quy này, bạn cần điều chỉnh cách xác thực của tài khoản MySQL root trước.

Đầu tiên, mở giao diện dòng lệnh MySQL:

```bash
sudo mysql
```

Sau đó, chạy lệnh ALTER USER sau để thay đổi phương thức xác thực của tài khoản root sang phương thức sử dụng mật khẩu. 

Ví dụ sau thay đổi phương thức xác thực thành `mysql_native_password`:

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

Sau khi thực hiện thay đổi này, thoát khỏi giao diện MySQL:

```bash
exit
```

Sau đó, bạn có thể chạy script `mysql_secure_installation` mà không gặp vấn đề gì.

Bắt đầu script tương tác bằng cách chạy:

```bash
sudo mysql_secure_installation
```

Sau đó cứ đọc yêu cầu và làm theo cho đến khi hoàn tất

Cuối cùng kiểm tra lại bằng cách đăng nhập vào với tài khoản và mật khẩu mơi:

```bash
sudo mysql -u username -p
```

![image.png](/Images/tuan_5_wordpress/image.png)

**Bước 3 – Cài đặt PHP**

Để cài đặt các gói này, chạy lệnh sau:

```bash
sudo apt install php libapache2-mod-php php-mysql
```

Sau khi cài đặt hoàn tất, chạy lệnh sau để xác nhận phiên bản PHP của bạn:

```bash
php -v
```

![image.png](/Images/tuan_5_wordpress/image%201.png)

**Bước 4 – Tạo Virtual Host cho trang Web của bạn**

Apache trên Ubuntu có sẵn một virtual host được kích hoạt mặc định, được cấu hình để phục vụ các tài liệu từ thư mục `/var/www/html`. Mặc dù điều này hoạt động tốt cho một trang web đơn, nhưng nếu bạn lưu trữ nhiều trang, việc này có thể gây bất tiện. Thay vì chỉnh sửa `/var/www/html`, chúng ta sẽ tạo một cấu trúc thư mục trong `/var/www` cho trang **your_domain**, giữ lại `/var/www/html` như thư mục mặc định được phục vụ nếu yêu cầu của khách không khớp với trang nào khác.

**Tạo thư mục cho your_domain:**

```bash
sudo mkdir /var/www/your_domain
```

Tiếp theo, gán quyền sở hữu cho thư mục với biến môi trường `$USER`, đại diện cho người dùng hệ thống hiện tại của bạn:

```bash
sudo chown -R **$USER**:**$USER** /var/www/your_domain
```

Sau đó, mở một file cấu hình mới trong thư mục `sites-available` của Apache bằng trình soạn thảo dòng lệnh ưa thích (ở đây ta sử dụng nano):

```bash
sudo nano /etc/apache2/sites-available/your_domain.conf
```

File này sẽ được tạo ra dưới dạng file trống. Thêm vào nội dung cấu hình cơ bản sau, nhớ thay thế **your_domain** bằng tên miền của bạn:

```jsx

<VirtualHost *:80>

	ServerName your_domain
	
	ServerAlias www.your_domain
	
	ServerAdmin webmaster@localhost
	
	DocumentRoot /var/www/your_domain
	
	ErrorLog ${APACHE_LOG_DIR}/error.log
	
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

Lưu và đóng file khi hoàn tất (nếu dùng nano, nhấn **CTRL+X**, sau đó nhập **Y** và nhấn **ENTER**).

Với cấu hình VirtualHost này, bạn đang chỉ định cho Apache phục vụ **your_domain** sử dụng thư mục `/var/www/your_domain` làm thư mục gốc (web root). Nếu bạn muốn thử nghiệm Apache mà không có tên miền, bạn có thể xóa hoặc chú thích các tùy chọn `ServerName` và `ServerAlias` bằng cách thêm dấu thăng (#) vào đầu mỗi dòng.

Bây giờ, kích hoạt virtual host mới bằng cách sử dụng lệnh `a2ensite`:

```jsx
sudo a2ensite your_domain
```

Bạn có thể muốn vô hiệu hóa trang web mặc định đi kèm với Apache. Điều này cần thiết nếu bạn không sử dụng tên miền tùy chỉnh, vì trong trường hợp này cấu hình mặc định của Apache sẽ ghi đè virtual host của bạn. Để vô hiệu hóa trang mặc định, gõ:

```jsx
sudo a2dissite 000-default
```

Để đảm bảo file cấu hình không có lỗi cú pháp, chạy lệnh sau:

```jsx
sudo apache2ctl configtest
```

Cuối cùng, tải lại Apache để áp dụng các thay đổi:

```jsx
sudo systemctl reload apache2
```

Trang web mới của bạn bây giờ đã được kích hoạt, nhưng thư mục gốc `/var/www/your_domain` vẫn còn trống. Tạo một file `index.html` trong thư mục đó để kiểm tra xem virtual host hoạt động như mong đợi hay không:

```jsx
nano /var/www/your_domain/index.html
```

Chèn nội dung sau vào file:

```jsx
<html>

	<head>
	
		<title>your_domain website</title>
	
	</head>
	
	<body>
	
		<h1>Hello World!</h1>
		
		<p>This is the landing page of <strong>your_domain</strong>.</p>
	
	</body>

</html>
```

Lưu và đóng file, sau đó mở trình duyệt và truy cập tên miền hoặc địa chỉ IP của máy chủ:

```jsx
http://server_domain_or_IP
```

Trang web của bạn sẽ hiển thị nội dung như đã được chỉnh sửa trong file.

Bạn có thể giữ file này làm trang đón tạm thời cho ứng dụng của bạn cho đến khi thiết lập file `index.php` thay thế. Khi làm như vậy, nhớ xóa hoặc đổi tên file `index.html` khỏi thư mục gốc vì theo mặc định nó sẽ được ưu tiên hơn file `index.php`.

### Cài đặt WordPress

**Bước 1 – Tạo cơ sở dữ liệu Mysql và tài khoản người dùng cho WordPress**

Bước đầu tiên là bước chuẩn bị. WordPress sử dụng MySQL để quản lý và lưu trữ thông tin của trang web cũng như người dùng. Bạn đã cài đặt MySQL, nhưng cần tạo một cơ sở dữ liệu và một tài khoản người dùng riêng cho WordPress sử dụng.

Để bắt đầu, đăng nhập vào tài khoản root của MySQL (tài khoản quản trị) bằng lệnh sau (lưu ý: đây không phải là tài khoản root của máy chủ):

```jsx
sudo mysql
```

> Lưu ý: Nếu bạn cài đặt MySQL theo một hướng dẫn khác với phần yêu cầu, có thể bạn đã bật xác thực bằng mật khẩu cho người dùng root của MySQL. Trong trường hợp đó, bạn có thể kết nối đến MySQL bằng lệnh:
> 

```jsx
mysql -u root -p
```

Trong MySQL, tạo một cơ sở dữ liệu chuyên dụng để WordPress quản lý. Bạn có thể đặt tên theo ý bạn, nhưng trong hướng dẫn này chúng ta sẽ dùng tên **wordpress**. Tạo cơ sở dữ liệu bằng lệnh:

```jsx
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Tạo người dùng bằng lệnh sau (nhớ chọn một mật khẩu mạnh cho người dùng này thay cho `'password'`):

```jsx
CREATE USER 'wordpressuser'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```

Sau đó, cấp quyền truy cập đầy đủ cho người dùng `wordpressuser` trên cơ sở dữ liệu **wordpress**:

```jsx
GRANT ALL ON wordpress.* TO 'wordpressuser'@'%';
```

Bây giờ, bạn đã có cơ sở dữ liệu và tài khoản người dùng riêng cho WordPress. Để MySQL nhận diện các thay đổi mới, hãy làm mới quyền bằng lệnh:

```jsx
FLUSH PRIVILEGES;
```

Thoát khỏi MySQL:

```jsx
EXIT;
```

**Bước 2 – Cài đặt thêm các PHP Extension**

Khi cài đặt LAMP stack, chúng ta chỉ cần một tập hợp rất cơ bản các extension để PHP có thể giao tiếp với MySQL. Tuy nhiên, WordPress và nhiều plugin của nó lại yêu cầu thêm một số PHP extension khác.

Trước tiên, hãy cập nhật danh sách các gói phần mềm trên máy chủ bằng lệnh APT:

```jsx
sudo apt update
```

Sau đó, cài đặt các PHP extension phổ biến cho WordPress:

```jsx
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```

Những extension này sẽ tạo nền tảng cho việc cài đặt các plugin bổ sung trên trang WordPress của bạn.

> Lưu ý: Mỗi plugin WordPress có thể có yêu cầu riêng về PHP. Nếu plugin của bạn cần thêm gói PHP nào đó, hãy kiểm tra tài liệu hướng dẫn của plugin và cài đặt theo cách tương tự.
> 

Sau khi cài đặt, bạn cần khởi động lại Apache để nạp các extension mới. Bạn có thể chờ đến bước điều chỉnh cấu hình Apache ở phần tiếp theo hoặc khởi động ngay bây giờ:

```jsx
sudo systemctl restart apache2
```

Sau khi khởi động lại, bạn có thể tiếp tục chuyển sang bước điều chỉnh cấu hình Apache.

**Bước 3 – Điều chỉnh cấu hình Apache cho phép ghi đè và Rewrite vớI .htaccess**

Tiếp theo, bạn sẽ thực hiện một vài thay đổi nhỏ trong cấu hình của Apache. Dựa theo các hướng dẫn yêu cầu, bạn sẽ có một tệp cấu hình cho trang web của mình trong thư mục `/etc/apache2/sites-available/`.

Trong hướng dẫn này, chúng ta sẽ sử dụng tệp `/etc/apache2/sites-available/wordpress.conf` làm ví dụ, nhưng bạn hãy thay thế bằng đường dẫn tới tệp cấu hình hiện có của bạn nếu cần. Ngoài ra, chúng ta sẽ sử dụng `/var/www/wordpress` làm thư mục gốc cho cài đặt WordPress. 

> Lưu ý: Có thể bạn đang dùng cấu hình mặc định 000-default.conf (với thư mục gốc là /var/www/html). Điều này hoàn toàn được nếu bạn chỉ muốn lưu trữ một trang web trên máy chủ. Nếu không, hãy chia nhỏ cấu hình thành các tệp riêng biệt, một tệp cho mỗi trang web.
> 

Sau khi xác định được các đường dẫn, bạn chuyển sang cấu hình file `.htaccess` để Apache có thể xử lý các thay đổi cấu hình theo thư mục.

***Cho phép ghi đè với .htaccess***

Hiện tại, việc sử dụng file `.htaccess` đang bị vô hiệu hóa. WordPress và nhiều plugin của nó sử dụng file này để điều chỉnh hành vi của máy chủ theo từng thư mục.

Mở tệp cấu hình Apache của trang web bằng trình soạn thảo văn bản (ở đây chúng ta dùng `nano`):

```jsx
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Để cho phép file `.htaccess`, bạn cần đặt chỉ thị `AllowOverride` trong khối `Directory` trỏ tới thư mục gốc của trang web. Thêm nội dung sau vào bên trong khối `VirtualHost` của tệp cấu hình, đảm bảo sử dụng đúng thư mục gốc:

```jsx
<VirtualHost *:80>

. . .

<Directory /var/www/wordpress/>

AllowOverride All

</Directory>

. . .

</VirtualHost>
```

Sau khi chỉnh sửa, lưu và đóng tệp. Trong `nano`, bạn nhấn `CTRL+X`, sau đó nhấn `Y` và `ENTER`.

**Kích hoạt Module Rewrite**

Tiếp theo, kích hoạt `mod_rewrite` để có thể sử dụng tính năng permalink của WordPress:

```jsx
sudo a2enmod rewrite
```

Lệnh `a2enmod` sẽ gọi một script kích hoạt module được chỉ định trong cấu hình của Apache.

**Áp dụng các thay đổI**

Trước khi áp dụng các thay đổi, hãy kiểm tra xem có lỗi cú pháp nào không bằng lệnh:

```jsx
sudo apache2ctl configtest
```

Bạn có thể nhận được kết quả như sau:

```jsx
Syntax OK
```

Nếu bạn muốn ẩn dòng cảnh báo đầu tiên, hãy thêm chỉ thị `ServerName` vào tệp cấu hình Apache chính tại `/etc/apache2/apache2.conf. ServerName` có thể là tên miền hoặc địa chỉ IP của máy chủ. Đây chỉ là cảnh báo và không ảnh hưởng đến chức năng của trang web. Miễn là kết quả có `Syntax OK`, bạn có thể tiếp tục.

Khởi động lại Apache để áp dụng các thay đổi (nhớ khởi động lại ngay cả khi bạn đã khởi động lại trước đó):

```jsx
sudo systemctl restart apache2
```

### **Bước 4:Tải về WordPress**

Khi phần mềm máy chủ đã được cấu hình, ta có thể tải về và cài đặt WordPress. Vì lý do bảo mật, luôn luôn nên tải phiên bản WordPress mới nhất từ trang chủ của nó.

Đầu tiên, chuyển đến một thư mục có quyền ghi (chúng tôi khuyên dùng thư mục tạm như `/tmp`):

```jsx
cd /tmp
```

Sau đó, tải về gói nén của bản phát hành mới nhất với lệnh curl:

```jsx
curl -O https://wordpress.org/latest.tar.gz
```

Giải nén tệp đã tải để tạo cấu trúc thư mục của WordPress:

```jsx
tar xzvf latest.tar.gz
```

Bạn sẽ chuyển các tệp này vào thư mục gốc tài liệu (document root) trong chốc lát. Trước khi làm việc đó, bạn có thể tạo một file .htaccess rỗng (dummy) để WordPress có thể sử dụng sau này:

```jsx
touch /tmp/wordpress/.htaccess
```

Sao chép file cấu hình mẫu sang tên mà WordPress sử dụng:

```jsx
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```

Ngoài ra, tạo thư mục **upgrade** để tránh các vấn đề về phân quyền khi WordPress tự cập nhật:

```jsx
mkdir /tmp/wordpress/wp-content/upgrade
```

Bây giờ, sao chép toàn bộ nội dung trong thư mục `/tmp/wordpress` vào thư mục gốc của trang web. Dấu chấm “.” ở cuối đường dẫn nguồn chỉ ra rằng mọi thứ, kể cả các tệp ẩn (như file .htaccess vừa tạo), sẽ được sao chép. Hãy đảm bảo thay `/var/www/wordpress` bằng thư mục gốc bạn đã thiết lập trên máy chủ:

```jsx
sudo cp -a /tmp/wordpress/. /var/www/wordpress
```

**Bước 5: Cấu hình WordPress**

### **Điều chỉnh quyền sở hữu và phân quyền**

Một bước quan trọng nữa là thiết lập quyền truy cập hợp lý cho các tệp và thư mục mà WordPress sử dụng để hoạt động.

Đầu tiên, chuyển quyền sở hữu toàn bộ các tệp cho người dùng và nhóm `www-data`. Đây là tài khoản mà Apache sử dụng, và Apache cần có quyền đọc và ghi các tệp WordPress để phục vụ trang web cũng như tự động cập nhật.

Cập nhật quyền sở hữu bằng lệnh `chown` (hãy đảm bảo trỏ đúng đến thư mục trên máy chủ của bạn):

```jsx
sudo chown -R www-data:www-data /var/www/wordpress
```

Tiếp theo, chạy hai lệnh `find` để đặt quyền phù hợp cho các thư mục và tệp của WordPress. Lệnh đầu tiên đặt quyền `750` cho tất cả các thư mục trong `/var/www/wordpress`:

```jsx
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
```

Lệnh thứ hai tìm tất cả các tệp và đặt quyền `640`:

```jsx
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;
```

Những quyền này sẽ giúp bạn sử dụng WordPress hiệu quả, tuy nhiên một số plugin hoặc quy trình có thể yêu cầu tinh chỉnh thêm.

### **Cấu hình tệp wp-config.php**

Bây giờ, bạn cần thực hiện một số thay đổi trong tệp cấu hình chính của WordPress.

Khi mở tệp, bước đầu tiên là điều chỉnh một số “secret keys” để tăng cường bảo mật cho cài đặt. WordPress cung cấp một công cụ tạo key an toàn để bạn không phải tự nghĩ ra các giá trị phức tạp. Những giá trị này chỉ dùng nội bộ, vì vậy việc sử dụng các giá trị an toàn, phức tạp sẽ không ảnh hưởng đến khả năng sử dụng.

Lấy các giá trị bảo mật từ WordPress Secret Key Generator bằng lệnh sau:

```jsx
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

Bạn sẽ nhận được các giá trị duy nhất tương tự như ví dụ sau:

![image.png](/Images/tuan_5_wordpress/image%202.png)

Sao chép kết quả mà bạn vừa nhận được.

Tiếp theo, mở tệp cấu hình của WordPress:

```jsx
sudo nano /var/www/wordpress/wp-config.php
```

Tìm đến phần chứa các giá trị mẫu của các thiết lập và dán những giá trị nhận được phía trên vào :

![image.png](/Images/tuan_5_wordpress/image%203.png)

Sau đó, bạn sẽ chỉnh sửa một số cài đặt kết nối cơ sở dữ liệu ở đầu tệp. Bạn cần thay đổi tên cơ sở dữ liệu, tên người dùng cơ sở dữ liệu và mật khẩu tương ứng mà bạn đã cấu hình trong MySQL.

Thay đổi khác bạn cần thực hiện là thiết lập phương thức WordPress dùng để ghi vào hệ thống tệp. Vì bạn đã cấp quyền ghi cần thiết cho máy chủ web, bạn có thể đặt rõ ràng phương thức filesystem thành “direct”. Nếu không thiết lập, WordPress sẽ yêu cầu nhập thông tin FTP khi thực hiện một số tác vụ.

Bạn có thể thêm cài đặt này ngay dưới phần cài đặt kết nối cơ sở dữ liệu (hoặc bất cứ đâu trong tệp):

![image.png](/Images/tuan_5_wordpress/image%204.png)

Sau khi hoàn tất, lưu và đóng tệp.

Bước 6: Kiểm tra hoạt động

Có thể kiểm tra xem WordPress hoạt động chưa bằng cách mở một trình duyệt bất kỳ và truy cập vào `http://your_domain_or_IP` 

Nếu cài đặt đúng thì sẽ hiện ra giao diện làm việc với WordPress như dưới đây:

![image.png](/Images/tuan_5_wordpress/image%205.png)

Chọn ngôn ngữ làm việc và nhấn **Continue**

Sau đó hiện giao diện để nhập thông tin cần thiết . Chú ý cần ghi lại hoặc đặt một mật khẩu quen thuộc để dễ hơn → Cuối cùng là nhấn nút **Install WordPress**

![image.png](/Images/tuan_5_wordpress/image%206.png)

Sau khi hoàn tất sẽ hiện thông báo cài đặt thành công > nhấn **Log in** 

![image.png](/Images/tuan_5_wordpress/image%207.png)

Đăng nhập vào tài khoản đã khai báo ta sẽ vào được giao diện **Dashboard**  của WordPress

![image.png](/Images/tuan_5_wordpress/image%208.png)

Tới đây là ta đã cài đặt thành công WordPress rồi

# Quản lý Theme

## Giới thiệu về Theme WordPress

**Theme WordPress** là tập hợp các giao diện thiết kế sẵn, giúp thay đổi toàn bộ diện mạo và cách trình bày của website **WordPress**. Điểm mạnh của việc sử dụng theme trên **WordPress** là bạn có thể dễ dàng thay đổi hàng ngàn giao diện chỉ với vài cú click chuột, mà không cần phải can thiệp hay chỉnh sửa mã nguồn ban đầu.

Về mặt kỹ thuật, **Theme WordPress** được tạo thành từ một tập hợp các tệp tin liên kết chặt chẽ với nhau, hình thành một giao diện thống nhất cho website. Theme sẽ thay đổi cách hiển thị của trang web trên trình duyệt mà không can thiệp vào mã nguồn gốc của **WordPress**.

## **Phân loại Theme WordPress**

Theme WordPress được chia thành 2 phần cơ bản là miễn phí và trả phí.

### **Theme miễn phí**

Theme miễn phí là những giao diện có sẵn trong thư viện của WordPress. Mỗi theme đều đi kèm với các tùy chọn tùy chỉnh cơ bản và có thể được cài đặt một cách dễ dàng chỉ với vài thao tác đơn giản. Với theme miễn phí, bạn có thể lựa chọn màu sắc, bố cục và tùy chỉnh một số yếu tố cơ bản cho trang web của mình. Tuy nhiên, nhược điểm lớn của chúng là giới hạn về tính năng và thiếu các công cụ nâng cao để tạo nên sự độc đáo và chuyên nghiệp cho website.

### **Theme trả phí**

Theme trả phí thường được mua từ các cửa hàng giao diện như ThemeForest, với giá từ vài chục USD trở lên. Những theme này cung cấp khả năng tùy chỉnh phong phú hơn so với bản miễn phí, hỗ trợ thêm các tiện ích mở rộng và thường tích hợp những tính năng đặc biệt mà chỉ có ở những sản phẩm cao cấp. Theme trả phí thường được cập nhật định kỳ, giúp đảm bảo tính bảo mật và hiệu suất cho website. Ngoài ra, người dùng cũng được hưởng dịch vụ hỗ trợ kỹ thuật trong suốt thời gian sử dụng.

## Cài đặt Theme WordPress

Để vào giao diện quản lý của theme WordPress ta thực hiện : tìm kiếm mục **Appearance** bên thanh sidebar **→** nhấn vào phần **Themes**

![image.png](/Images/tuan_5_wordpress/image%209.png)

Tiếp đến ta sẽ tiến hành cài đặt theme 

### **Cách 1: Hướng dẫn cài theme WordPress có sẵn trong kho giao diện**

Bước 1: Ở giao diện quản lý theme WordPress nhấn **Add New Theme**

![image.png](/Images/tuan_5_wordpress/image%2010.png)

Bước 2: Tại thư viện Theme WordPress hiện có hàng ngàn sản phẩm, bạn có thể sử dụng bộ lọc để tìm kiếm theme phù hợp với mục đích và nhu cầu của mình.

![image.png](/Images/tuan_5_wordpress/image%2011.png)

Bước 3: Nhấp và để xem chi tiết hoặc chọn “Preview” để xem trước giao diện. 

Lưu ý rằng giao diện sẽ không hiển thị đầy đủ như ảnh demo cho đến khi cài theme cho WordPress. Một số theme có cách cài đặt phức tạp sẽ đi kèm với thông tin chi tiết hoặc hướng dẫn sử dụng trong phần mô tả.

![image.png](/Images/tuan_5_wordpress/image%2012.png)

Bước 4: Sau khi chọn được theme phù hợp, bạn hãy nhấn chọn “Install” để tải các tệp giao diện lên thư mục website WordPress.

![image.png](/Images/tuan_5_wordpress/image%2013.png)

Bước 5: Sau khi quá trình cài đặt hoàn tất, các tệp tin của theme sẽ được lưu trong thư mục /wp-content/themes. Để bắt đầu sử dụng theme này, bạn hãy nhấn “Activate” để kích hoạt nó.

![image.png](/Images/tuan_5_wordpress/image%2014.png)

### **Cách 2: Hướng dẫn cài đặt theme WordPress bằng cách upload file**

Bước 1: Ở giao diện quản lý theme WordPress nhấn **Add New Theme → Upload theme**

![image.png](/Images/tuan_5_wordpress/image%2015.png)

Bước 2: Nhấn vào “Chọn tệp” và tải tệp theme đã cài đặt trước đó lên WordPress.

![image.png](/Images/tuan_5_wordpress/image%2016.png)

Bước 3: Nhấn vào nút “Install Now” để bắt đầu quá trình cài theme WordPress.

Bước 4: Sau khi cài đặt hoàn tất, bạn hãy nhấn vào nút “Activate” để kích hoạt theme vừa cài đặt.

![image.png](/Images/tuan_5_wordpress/image%2017.png)

# Quản lý Plugin

## Giới thiệu về Plugin WordPress

Plugin chính là các mô-đun mở rộng cho phép bạn thêm chức năng, cải tiến giao diện hoặc nâng cao hiệu suất website. Từ các nhà phát triển cá nhân đến tổ chức như Automattic (đơn vị sáng lập WordPress.com), plugin có thể được cung cấp miễn phí hoặc tính phí tùy theo chức năng.

## Cài đặt Plugin WordPress

Để vào màn hình quản lý của Plugins ta thực hiện: tìm kiếm **Plugins** bên than sidebar → nhấn vào phần **Add Plugin**

![image.png](/Images/tuan_5_wordpress/image%2018.png)

Tiếp theo ta sẽ tiến hành cài đặt **Plugin** 

### **Cách 1: Cài đặt plugin từ thư viện WordPress**

Đầu tiên khi nhấn vào phần **Add Plugins** ta sẽ thấy giao diện với rất nhiều plugin, thì đây là những Plugin có sẵn trong thư viện **WordPress**

Bước 1: Tìm kiếm Plugin mong muốn ở thanh tìm kiếm

![image.png](/Images/tuan_5_wordpress/image%2019.png)

Bước 2: Chọn Plugin phù hợp và nhấn **Install Now**

![image.png](/Images/tuan_5_wordpress/image%2020.png)

Bước 3:Nhấn Active để khởi chạy **Plugin**

![image.png](/Images/tuan_5_wordpress/image%2021.png)

Chưa đầy một phút, plugin đã sẵn sàng để sử dụng!

### **Cách 2: Cài đặt plugin từ nguồn bên ngoài**

Bước 1: Sau khi vào giao diện quản lý Plugin → nhấn **Upload Plugin**

![image.png](/Images/tuan_5_wordpress/image%2022.png)

Bước 2: Chọn file **.zip** Plugin tải lên → nhấn **Install Now**

![image.png](/Images/tuan_5_wordpress/image%2023.png)

Đợi ít phút để cài đặt, sau khi cài đặt thành công sẽ hiện thống báo 

![image.png](/Images/tuan_5_wordpress/image%2024.png)

Nhấn **Activate Plugin** để kích hoạt và sử dụng  

Sau khi cài đặt thành công ta có thể xem những Plugin đã cài đặt ở mục **Plugin** → **Installed Plugins**

![image.png](/Images/tuan_5_wordpress/image%2025.png)

# Tối ưu hóa bảo mật

Bảo mật và tối ưu hóa website là hai yếu tố then chốt giúp duy trì hiệu suất, uy tín và sự ổn định cho bất kỳ nền tảng WordPress nào. Trong bối cảnh các mối đe dọa mạng ngày càng tinh vi, việc kết hợp các chiến lược bảo mật và tối ưu hóa là điều không thể thiếu. 

## **Cập nhật WordPress, theme và plugin thường xuyên**

Lỗi bảo mật thường xuất hiện ở các plugin hoặc theme chưa cập nhật. Mỗi bản cập nhật đều chứa các bản vá cho các lỗi hoặc lỗ hổng bảo mật đã được phát hiện ở phiên bản trước. Khi WordPress được cập nhật, nếu các plugin hoặc theme không được nâng cấp theo, có thể dẫn đến xung đột, lỗi hiển thị hoặc mất chức năng.

Ngoài bảo mật, các bản cập nhật thường cải tiến hiệu năng, tốc độ tải trang hoặc bổ sung tính năng mới giúp website hoạt động hiệu quả hơn.

**Cách thực hiện:**

- Bật cập nhật tự động cho plugin, theme tin cậy. WordPress cho phép bạn bật tự động cập nhật cho các plugin theo thao tác :**Plugins** → **Installed Plugins.** Ở mỗi Plugin đã cài đặt sẽ có cột Automatic Updates bạn chỉ cần nhấn vào các dòng tương ứng sao cho hiện **Enable auto-updates** là được
    
    ![image.png](/Images/tuan_5_wordpress/image%2026.png)
    
- Sao lưu website trước khi cập nhật. Dùng plugin như UpdraftPlus, BlogVault hoặc sử dụng tính năng backup của hosting để lưu trữ toàn bộ dữ liệu và cơ sở dữ liệu.
- Theo dõi blog chính thức của WordPress và nhà cung cấp plugin để cập nhật thông báo bảo mật.
- Không sử dụng plugin, theme null hoặc lậu (dễ chứa mã độc).

## **Thay đổi các thông tin mặc định của WordPress**

Mặc định khi cài WordPress sẽ sử dụng các thông tin mặc định của WordPress, ai cũng biết điều này và hiển nhiên hacker cũng biết. Sau đây là một số thông tin mặc định mà bạn nên thay đổi ngay:

- User: mặc định user có tên `admin` và bạn hãy thay đổi nó.
- prefix database: Mặc định prefix sẽ là `wp_`và bạn hãy thay đổi nó.
- Đường dẫn đăng nhập: `wp-admin` và `wp-login.php`.

## **Phân quyền, bảo vệ file và thư mục với htaccess**

**Vô hiệu hóa thực thi PHP trong thư mục /wp-content/uploads**

```
RewriteRule ^wp\-content/uploads/.*\.(?:php[1-7]?|pht|phtml?|phps|js)$ - [NC,F]
```

**Bảo vệ wp-config.php**

```
<files wp-config.php>
order allow,deny
deny from all
</files>
```

**Bảo vệ .htaccess khỏi truy cập trái phép**

```
<files ~ "^.*\.([Hh][Tt][Aa])">
order allow,deny
deny from all
satisfy all
</files>
```

**Chặn cross-site scripting (XSS)**

```
# Blocks some XSS attacks
<IfModule mod_rewrite.c>
RewriteCond %{QUERY_STRING} (\|%3E) [NC,OR]
RewriteCond %{QUERY_STRING} GLOBALS(=|\[|\%[0-9A-Z]{0,2}) [OR]
RewriteCond %{QUERY_STRING} _REQUEST(=|\[|\%[0-9A-Z]{0,2})
RewriteRule .* index.php [F,L]
</IfModule>
```

**Hạn chế quyền truy cập vào wp-includes**

```
# Blocks all wp-includes folders and files
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>
```

**Hạn chế quyền truy cập trực tiếp vào các file PHP của Plugins & Themes**

```
RewriteRule ^wp\-content/plugins/.*\.(?:php[1-7]?|pht|phtml?|phps)$ - [NC,F]
RewriteRule ^wp\-content/themes/.*\.(?:php[1-7]?|pht|phtml?|phps)$ - [NC,F]
```

**Bảo vệ file xmlrpc.php**

```
<files xmlrpc.php>
order allow,deny
deny from all
</files>
```

**Giới hạn truy cập wp-admin**

```
order deny,allow
allow from địa-chỉ-ip-của-bạn
deny from all
```

**Tóm tắt lại sau đây là file `.htaccess` mẫu** 

```jsx
<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteBase /
	RewriteRule ^index\.php$ - [L]
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule . /index.php [L]
</IfModule>

# Blocks all wp-includes folders and files
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>

# Blocks some XSS attacks
<IfModule mod_rewrite.c>
RewriteCond %{QUERY_STRING} (\|%3E) [NC,OR]
RewriteCond %{QUERY_STRING} GLOBALS(=|\[|\%[0-9A-Z]{0,2}) [OR]
RewriteCond %{QUERY_STRING} _REQUEST(=|\[|\%[0-9A-Z]{0,2})
RewriteRule .* index.php [F,L]
</IfModule>

<IfModule mod_rewrite.c>
	RewriteEngine On
	
	# Disable PHP in Uploads - Security > Settings > System Tweaks > PHP in Uploads
	RewriteRule ^wp\-content/uploads/.*\.(?:php[1-7]?|pht|phtml?|phps|js)$ - [NC,F]
	
	# Disable PHP in Plugins - Security > Settings > System Tweaks > PHP in Plugins
	RewriteRule ^wp\-content/plugins/.*\.(?:php[1-7]?|pht|phtml?|phps)$ - [NC,F]
	
	# Disable PHP in Themes - Security > Settings > System Tweaks > PHP in Themes
	RewriteRule ^wp\-content/themes/.*\.(?:php[1-7]?|pht|phtml?|phps)$ - [NC,F]
</IfModule>

# Bock access xmlrpc.php
<Files xmlrpc.php>
	Order deny,allow
	Deny from all
</Files>

# Bock access wp-config.php
<Files wp-config.php>
	Order deny,allow
	Deny from all
</Files>

# Bock access license.txt
<Files license.txt>
	Order deny,allow
	Deny from all
</Files>

# Bock access readme.html
<Files readme.html>
	Order deny,allow
	Deny from all
</Files>

# Bock access .htaccess
<files ~ "^.*\.([Hh][Tt][Aa])">
	order allow,deny
	deny from all
	satisfy all
</files>
```

## **Cấu hình SSH Key và login passwd**

SSH Key là một giải pháp khá an toàn để chứng thực cho việc đăng nhập. Khi bạn sử dụng SSH Key và tắt login passwd thì kẻ xấu cho dù có passwd root của không thể đăng nhập vào được

## **Cấu hình thêm Firewall**

Firewall là bức tường lửa để ngăn chặn các cuộc tấn công ở bên ngoài. Từ đó giúp cho máy chủ bạn được an toàn hơn

- CSF
- Firewalld
- UFW
- fail2ban

## **Cài đặt thêm ứng dụng scan mã độc**

Hiện có rất nhiều ứng dụng scan mã độc, điển hình là cpguard và imunify. Đây là 2 ứng dụng scan mà tôi thấy tốt nhất và có là sản phẩm trả phí. Tuy nhiên đối với Imunify có bản AV là bản miễn phí và bạn có thể cài đặt nó để sử dụng

---
THE END