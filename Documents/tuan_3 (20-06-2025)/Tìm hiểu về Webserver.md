# Webserver
# Mục lục
  - [1. Giới thiệu về Webserver](#1-giới-thiệu-về-webserver)
  - [2. Cấu hình Webserver](#2-cấu-hình-webserver)
    - [Apache](#apache)
    - [NGINX](#nginx-1)
    - [OpenLiteSpeed](#openlitespeed-1)
  - [3. Quản lý trạng thái của các Webserver](#quản-lý-trạng-thái-của-các-webserver)
    - [Quản lý trạng thái của Apache](#quản-lý-trạng-thái-của-apache)
    - [Quản lý trạng thái của NGINX](#quản-lý-trạng-thái-của-nginx)
    - [Quản lý trạng thái của OpenLiteSpeed](#quản-lý-trạng-thái-của-openlitespeed)
    - [Lỗi trong quá trình cài đặt](#lỗi-trong-quá-trình-cài-đặt)
## 1. Giới thiệu về Webserver

**Web server** là máy chủ cài đặt các chương trình phục vụ các ứng dụng web. Webserver có khả năng tiếp nhận request từ các trình duyệt web và gửi phản hồi đến client thông qua giao thức HTTP hoặc các giao thức khác. Có nhiều web server khác nhau như: Apache, Nginx, IIS, … Web server thông dụng nhất hiện nay:

![image.png](/Images/tuan_3_webserver/image.png)

### **Chức năng chính của Web Server**

- **Xử lý yêu cầu HTTP:** Web server có khả năng lắng nghe và chấp nhận các yêu cầu được gửi từ trình duyệt qua giao thức HTTP.
- **Phân phát nội dung:** Sau khi nhận yêu cầu, web server sẽ phân phát các loại nội dung khác nhau. Chúng bao gồm các file HTML, CSS để định dạng trang, JavaScript để tạo tương tác, hình ảnh, video và các loại file đa phương tiện khác.
- **Hỗ trợ ngôn ngữ lập trình Server-side:** Web server hỗ trợ tích hợp với các môi trường thực thi của các ngôn ngữ lập trình phía máy chủ như PHP, Python, Node.js hay Ruby on Rails. Điều này cho phép web server xử lý các script động, tạo ra nội dung tùy chỉnh cho mỗi yêu cầu.
- **Ghi Log truy cập:** Web server tự động ghi lại các thông tin chi tiết về mọi yêu cầu được xử lý. Các log này bao gồm địa chỉ IP của người truy cập, thời gian yêu cầu, URL được truy cập, và mã trạng thái HTTP của phản hồi.
- **Bảo mật cơ bản:** Web server tích hợp các tính năng bảo mật cơ bản như xác thực người dùng, mã hóa dữ liệu qua SSL/TLS (HTTPS) và cấu hình tường lửa.

### Một số Webserver phổ biến hiện nay

### **Apache HTTP server**

**Apache** là web server được sử dụng rộng rãi nhất thế giới. Apache được phát triển và duy trì bởi một cộng đồng mã nguồn mở dưới sự bảo trợ của Apache Software Foundation. Apache được phát hành với giấy phép Apache License là được sử dụng tự do, miễn phí.

### **Nginx**

**Nginx** là một web server nhẹ (Đọc thêm Nginx là gì), không chiếm nhiều tài nguyên của hệ thống. Nginx còn là một reserse proxy mã nguồn mở. Nginx khá là ổn định, cấu hình đơn giản và hiệu suất cao.

Nginx được phát triển bởi Igor Sesoev vào năm 2002 chủ yếu là để phục vụ cho website rambler.ru (trang web được truy cập nhiều thứ hai của nước Nga). Theo thống kê của Netcaft, trong một triệu website lớn nhất thế giới có 6.52% sử dụng Nginx.

Nginx là phần mềm mã nguồn mở và miễn phí, được phát hành rộng rãi theo giấy phép BSD. Nginx được phát triển bằng ngôn ngữ  và chạy được trên các hệ điều hành như Linux, FreeBSD, Windows, MacOS…

Nginx có các tính năng như chứng thực người dùng, virtual hosting, hỗ trợ CGI, FCGI, SCGI, WCGI, SSI, ISAPI, HTTPS, Ipv6, …

### OpenLiteSpeed

**OpenLiteSpeed** là một máy chủ web (web server) mã nguồn mở, được phát triển bởi LiteSpeed Technologies. Giải pháp này nổi bật với hiệu suất cao và kiến trúc gọn nhẹ. OpenLiteSpeed được thiết kế để xử lý hàng ngàn kết nối đồng thời mà vẫn tiêu thụ ít tài nguyên hệ thống như CPU và RAM.

## 2. Cấu hình Webserver

# Apache

### Cài đặt Apache:

Trước tiên cập nhật chỉ mục gói cục bộ để phản ánh những thay đổi mới nhất từ nguồn:

```
sudo apt update
```

Sau đó cài đặt gói `apache2` :

```
sudo apt install apache2
```

![image.png](/Images/tuan_3_webserver/image%201.png)

Kiểm tra tường lửa (nếu có) xem đã cho phép dịch vụ hay chưa:

```
sudo ufw app list
```

![image.png](/Images/tuan_3_webserver/image%202.png)

Xem trạng thái dịch vụ:

```
sudo ufw status
```

![image.png](/Images/tuan_3_webserver/image%203.png)

Như output xác nhận, dịch vụ đã khởi động thành công. Tuy nhiên, cách tốt nhất để kiểm tra là yêu cầu một trang từ Apache.

Bạn có thể truy cập trang landing mặc định của Apache để xác nhận rằng phần mềm đang hoạt động đúng bằng cách sử dụng địa chỉ IP của server.

Hãy thử nhập lệnh sau tại Teminal của server:

```
hostname -I
```

![image.png](/Images/tuan_3_webserver/image%204.png)

Khi bạn có địa chỉ IP của server, hãy nhập nó vào thanh địa chỉ của trình duyệt:

```
http://your_server_ip
```

![image.png](/Images/tuan_3_webserver/image%205.png)

### Tạo VirtualHost

**Virtual Host** có thể được định nghĩa là một phương pháp cho phép một server duy nhất phân chia tài nguyên để phục vụ nhiều tên miền khác nhau. Khi một người dùng nhập tên miền vào trình duyệt, web server sẽ sử dụng thông tin trong **HTTP headers** để xác định trang web nào đang được yêu cầu.

- **Cách thức hoạt động**: Mỗi lần một yêu cầu đến server, server sẽ kiểm tra trường “Host” trong request header, từ đó quyết định phục vụ nội dung từ virtual host tương ứng.
- **Ví dụ**: Giả sử bạn có hai tên miền là azdigi.com và azdigi.net. Khi người dùng truy cập azdigi.com, server sẽ tìm virtual host được cấu hình cho azdigi.com và trả về nội dung tương ứng. Việc sử dụng virtual host mang lại sự linh hoạt, giúp giảm thiểu chi phí mà vẫn đáp ứng tốt nhu cầu sử dụng.

Apache trên Ubuntu 20.04 có một server block được kích hoạt mặc định, được cấu hình để phục vụ nội dung từ thư mục `/var/www/html`. Trong khi cấu hình này phù hợp cho một trang web đơn lẻ, nó có thể trở nên bất tiện nếu bạn phục vụ nhiều trang. Thay vì sửa đổi `/var/www/html`, hãy tạo một cấu trúc thư mục trong `/var/www` cho trang **your_domain**, giữ `/var/www/html` làm thư mục mặc định được phục vụ nếu yêu cầu từ client không khớp với trang nào khác.

Tiến hành tạo VirtualHost theo các bước như sau:

Tạo thư mục cho **your_domain** bằng lệnh:

```
sudo mkdir /var/www/your_domain
```

![image.png](/Images/tuan_3_webserver/image%206.png)

Tiếp theo, gán quyền sở hữu thư mục cho biến môi trường `$USER`:

```
sudo chown -R **$USER**:**$USER** /var/www/your_domain
```

Để đảm bảo quyền đã được đặt đúng, cho phép chủ sở hữu có quyền đọc, ghi và thực thi trong khi chỉ cấp quyền đọc và thực thi cho nhóm và người khác, bạn nhập lệnh:

```
sudo chmod -R 755 /var/www/your_domain
```

Tiếp theo, tạo một trang index.html mẫu sử dụng nano hoặc trình soạn thảo yêu thích của bạn:

```
sudo nano /var/www/your_domain/index.html
```

Bên trong, thêm nội dung HTML mẫu sau:

```
<html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```

![image.png](/Images/tuan_3_webserver/image%207.png)

Lưu và đóng tập tin khi bạn đã hoàn tất

Để Apache phục vụ nội dung này, cần tạo một tập tin cấu hình Virtual Host với các chỉ thị chính xác. Thay vì sửa đổi trực tiếp tập tin cấu hình mặc định nằm tại `/etc/apache2/sites-available/000-default.conf`, hãy tạo một tập tin mới tại `/etc/apache2/sites-available/your_domain.conf`:

```
sudo nano /etc/apache2/sites-available/your_domain.conf
```

Dán vào khối cấu hình sau, tương tự như cấu hình mặc định nhưng được cập nhật cho thư mục và tên miền mới:

```
<VirtualHost *:80>
ServerAdmin webmaster@localhost
ServerName your_domain
ServerAlias www.your_domain
DocumentRoot /var/www/your_domain
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

![image.png](/Images/tuan_3_webserver/image%208.png)

Lưu và đóng tập tin khi đã xong.

Kích hoạt tập tin cấu hình bằng công cụ `a2ensite`:

```
sudo a2ensite your_domain.conf
```

![image.png](/Images/tuan_3_webserver/image%209.png)

Vô hiệu hóa trang mặc định được định nghĩa trong `000-default.conf`:

```
sudo a2dissite 000-default.conf
```

![image.png](/Images/tuan_3_webserver/image%2010.png)

Tiếp theo, kiểm tra lỗi cấu hình:

```
sudo apache2ctl configtest
```

![image.png](/Images/tuan_3_webserver/image%2011.png)

Khởi động và kiểm tra lại Apache để áp dụng thay đổi:

```
sudo systemctl restart apache2
sudo systemctl status apache2
```

![image.png](/Images/tuan_3_webserver/image%2012.png)

Apache bây giờ sẽ phục vụ tên miền của bạn. Bạn có thể kiểm tra bằng cách điều hướng đến và bạn sẽ thấy giao diện như sau:

![image.png](/Images/tuan_3_webserver/ea7f2b47-8ec8-4a9b-84bf-7b07c79e25db.png)

Tới đây là ta đã tạo VirtualHost thành công.

### Cấu hình SSL cho VirtualHost

Trước khi sử dụng bất kỳ chứng chỉ TLS nào, bạn cần kích hoạt mod_ssl, một module của Apache cung cấp hỗ trợ mã hóa SSL.

**Bước 1: Kích hoạt mod_ssl**

Kích hoạt mod_ssl bằng lệnh a2enmod:

```
sudo a2enmod ssl
```

Sau đó, khởi động lại Apache để kích hoạt module:

```
sudo systemctl restart apache2
```

![image.png](/Images/tuan_3_webserver/image%2013.png)

Module mod_ssl đã được kích hoạt và sẵn sàng sử dụng.

**Bước 2: Tạo chứng chỉ SSL**

Ta sẽ tạo chứng chỉ tự ký bằng openssl, nếu như bạn chưa cài đặt gói openssl thì có thể chạy lệnh sau:

```
sudo apt install openssl
```

Sau khi cài xong ta tiến hành tạo khóa:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 
-keyout /etc/ssl/private/apache-selfsigned.key 
-out /etc/ssl/certs/apache-selfsigned.crt
```

Trong đó:

- `openssl`**:** Công cụ dòng lệnh để tạo và quản lý chứng chỉ OpenSSL, khóa và các file liên quan.
- `req -x509`**:** Xác định rằng chúng ta sử dụng X.509 certificate signing request (CSR) cho việc quản lý. X.509 là một tiêu chuẩn cơ sở hạ tầng khóa công khai mà TLS tuân theo để quản lý khóa và chứng chỉ.
- `nodes`**:** Cho OpenSSL biết bỏ qua bước bảo vệ chứng chỉ bằng passphrase. Chúng ta cần Apache có thể đọc file mà không cần can thiệp của người dùng khi khởi động máy chủ. Nếu có passphrase, chúng ta sẽ phải nhập lại sau mỗi lần khởi động.
- `days 365`**:** Thiết lập thời gian chứng chỉ được coi là hợp lệ, ở đây là 1 năm. Nhiều trình duyệt hiện đại sẽ từ chối các chứng chỉ có thời gian hợp lệ quá 1 năm.
- `newkey rsa:2048`**:** Tạo một chứng chỉ mới và một khóa mới cùng lúc. Vì chúng ta chưa tạo sẵn khóa để ký chứng chỉ, nên cần tạo cùng lúc. Phần rsa:2048 chỉ định tạo một khóa RSA dài 2048 bit.
- `keyout`**:** Chỉ định nơi lưu file khóa riêng (Private Key) được tạo.
- `out`**:** Chỉ định nơi lưu chứng chỉ được tạo.

Sau khi chạy lệnh trên thì bạn sẽ được yêu cầu nhập 1 số trường cơ bản để khởi tạo Key:

![image.png](/Images/tuan_3_webserver/image%2014.png)

Cả hai file bạn tạo sẽ được lưu vào các thư mục thích hợp dưới `/etc/ssl`.

Tiếp theo, chúng ta sẽ cập nhật cấu hình Apache để sử dụng chứng chỉ và khóa mới này.

**Bước 3: Cấu hình Apache sử dụng SSL/TLS** 

Tạo một file cấu hình tối giản mới. (Nếu bạn đã có một khối `<VirtualHost>` cho Apache và chỉ cần thêm TLS, bạn có thể cần sao chép các dòng cấu hình bắt đầu bằng SSL và chuyển VirtualHost từ cổng 80 sang 443. Chúng ta sẽ xử lý cổng 80 trong bước tiếp theo.)

Mở một file mới trong thư mục `/etc/apache2/sites-available`:

```
sudo nano /etc/apache2/sites-available/your_domain_or_ip.conf
```

Dán vào file cấu hình VirtualHost tối giản sau:

![image.png](/Images/tuan_3_webserver/image%2015.png)

Trong đó:

- `Protocols h2 http/1.1` : Chỉ định Apache sử dụng **giao thức HTTP/2 (`h2`)** và HTTP/1.1 cho kết nối HTTPS
- `ErrorLog ${APACHE_LOG_DIR}/error.log` : Xác định **vị trí lưu file log lỗi** cho VirtualHost này
- `CustomLog ${APACHE_LOG_DIR}/access.log combined` : Ghi log **tất cả truy cập** đến website này

Cài đặt module HTTP/2:

```
sudo a2enmod http2
```

Tiếp theo, tạo thư mục DocumentRoot và thêm một file HTML đơn giản để kiểm tra:

```
sudo mkdir /var/www/your_domain_or_ip
```

Mở file `index.html` với trình soạn thảo:

```
sudo nano /var/www/your_domain_or_ip/index.html
```

Dán nội dung sau vào file:

```
<html>
   <head>
     <title>Server cua toi</title>
</head>
<body>
<h1>Xin chao the gioi. cap chung chi SSL thanh cong</h1>
</body>
</html>

```

![image.png](/Images/tuan_3_webserver/image%2016.png)

Lưu và đóng file. Sau đó, kích hoạt file cấu hình với công cụ `a2ensite`:

```
sudo a2ensite your_domain_or_ip.conf
```

Tiếp theo, kiểm tra lỗi cấu hình:

```
sudo apache2ctl configtest
```

Nếu mọi thứ đều thành công, bạn sẽ thấy thông báo như sau:

![image.png](/Images/tuan_3_webserver/image%2017.png)

Giờ hãy mở trang web của bạn trong trình duyệt, nhớ sử dụng `https://` ở đầu địa chỉ.

Bạn sẽ thấy một cảnh báo lỗi. Đây là điều bình thường với chứng chỉ tự ký! Trình duyệt cảnh báo rằng không thể xác minh danh tính máy chủ vì chứng chỉ không được ký bởi bất kỳ tổ chức chứng thực nào mà trình duyệt biết. Trong mục đích thử nghiệm và sử dụng cá nhân, bạn có thể chọn “nâng cao” hoặc “xem thêm thông tin” và tiếp tục truy cập trang web.

Sau khi chấp nhận cảnh báo, thì đây là kết quả nhận được:

![image.png](/Images/tuan_3_webserver/image%2018.png)

**Bước 4: Chuyển hướng HTTP sang HTTPS**

Hiện tại, cấu hình của bạn chỉ phản hồi các yêu cầu HTTPS trên cổng 443. Thực hành tốt là phản hồi trên cổng 80, ngay cả khi bạn muốn buộc toàn bộ lưu lượng truy cập được mã hóa. Hãy thiết lập một VirtualHost để phản hồi các yêu cầu không mã hóa này và chuyển hướng chúng sang HTTPS.

Mở lại file cấu hình Apache mà ta đã tạo:

```
sudo nano /etc/apache2/sites-available/your_domain_or_ip.conf
```

Ở cuối file, tạo thêm một khối VirtualHost để xử lý các yêu cầu trên cổng 80. Sử dụng chỉ thị ServerName để khớp với tên miền hoặc địa chỉ IP của bạn, sau đó dùng lệnh Redirect để chuyển hướng tất cả các yêu cầu sang VirtualHost TLS. Hãy nhớ thêm dấu gạch chéo ở cuối:

![image.png](/Images/tuan_3_webserver/image%2019.png)

Lưu và đóng file. Sau đó, kiểm tra lại cấu hình:

```
sudo apachectl configtest
sudo systemctl reload apache2
```

Kiểm tra chức năng chuyển hướng mới bằng cách truy cập trang web với `http://` ở đầu địa chỉ. Bạn sẽ được tự động chuyển hướng sang `https://`

# NGINX

### Cài đặt NGINX

Trước tiên cập nhật chỉ mục gói cục bộ để phản ánh những thay đổi mới nhất từ nguồn:

```
sudo apt update
```

![image.png](/Images/tuan_3_webserver/image%2020.png)

Tiếp theo là cài gói NGINX:

```
sudo apt install nginx
```

![image.png](/Images/tuan_3_webserver/image%2021.png)

Cấu hình Firewall (nếu có):

```
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

Kiểm tra trạng thái bằng lệnh:

```
sudo systemctl status nginx
```

![image.png](/Images/tuan_3_webserver/image%2022.png)

Tìm kiếm địa chỉ IP máy trên trình duyệt bất kỳ:

![image.png](/Images/tuan_3_webserver/image%2023.png)

Tới đây là t đã hoàn thành cài đặt NGINX. 

### Tạo VirtualHost

Giống như Apache thì NGINX cũng cho phép bạn quản lý nhiều tên miền hoặc dự án trên một máy chủ.

Đói với một vài phiên bản Ubuntu thì khi cài đặt NGINX sẽ không có 2 thư mục `/etc/nginx/sites-available` và `/etc/nginx/sites-enabled`. Nếu bạn muốn quản lý giống như Apache thì có thể tự tạo, hoặc có thể cấu hình trong file mặc định `/etc/nginx/nginx.conf.`

Ở ví dụ này mình sẽ tạo `/etc/nginx/sites-available` và `/etc/nginx/sites-enabled` để dễ quản lý.

Để tạo 2 thư mục trên thực hiện lệnh sau:

```
 mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
```

![image.png](/Images/tuan_3_webserver/image%2024.png)

Sau đó tạo file và cấu hình sau cho domain → ở bước này chỉ cấu hình đơn giản cho http (port 80):

```
sudo nano /etc/nginx/sites-available/your_domain.conf
```

![image.png](/Images/tuan_3_webserver/image%2025.png)

Sau khi cấu hình file `/etc/nginx/sites-available/your_domain.conf` xong, ta tạo symlink:

```
sudo ln -s /etc/nginx/sites-available/your_domain.conf 
            /etc/nginx/sites-enabled/
```

Chỉnh `/etc/nginx/nginx.conf`, thêm dòng sau vào bên trong khối **`http {}`:**

```
 include /etc/nginx/sites-enabled/*;
```

![image.png](/Images/tuan_3_webserver/image%2026.png)

Tạo file giao diện sao cho khớp với đường dẫn bạn đã khai báo trong VirtualHost trước đó. Ở đây tôi khai báo `/var/www/thienduong.com/html/nginx` nên tôi sẽ tạo file index ở `/var/www/thienduong.com/html/nginx/index.html` 

![image.png](/Images/tuan_3_webserver/image%2027.png)

Sau khi cấu hình xong, thực hiện các lệnh sau để kiêm tra cú pháp:

```
 sudo nginx -t
```

![image.png](/Images/tuan_3_webserver/image%2028.png)

Để áp dụng cấu hình đã thay đổi :

```
sudo systemctl reload nginx
```

Tìm kiếm bằng domain bạn vừa tạo trong VirtualHost trên trình duyệt bất kỳ:

![image.png](/Images/tuan_3_webserver/image%2029.png)

Tới đây, ta đã tạo và cấu hình thành công cho VirtualHost ở NGINX.

### Cấu hình SSL cho VirtualHost

**Bước 1. Tạo khóa SSL tự ký**

Ta sẽ tạo chứng chỉ tự ký bằng openssl, nếu như bạn chưa cài đặt gói openssl thì có thể chạy lệnh sau:

```
sudo apt install openssl
```

Sau khi cài xong ta tiến hành tạo khóa:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 
-keyout /etc/ssl/private/nginx-selfsigned.key 
-out /etc/ssl/certs/nginx-selfsigned.crt
```

Trong đó:

- `openssl`**:** Công cụ dòng lệnh để tạo và quản lý chứng chỉ OpenSSL, khóa và các file liên quan.
- `req -x509`**:** Xác định rằng chúng ta sử dụng X.509 certificate signing request (CSR) cho việc quản lý. X.509 là một tiêu chuẩn cơ sở hạ tầng khóa công khai mà TLS tuân theo để quản lý khóa và chứng chỉ.
- `nodes`**:** Cho OpenSSL biết bỏ qua bước bảo vệ chứng chỉ bằng passphrase. Chúng ta cần Apache có thể đọc file mà không cần can thiệp của người dùng khi khởi động máy chủ. Nếu có passphrase, chúng ta sẽ phải nhập lại sau mỗi lần khởi động.
- `days 365`**:** Thiết lập thời gian chứng chỉ được coi là hợp lệ, ở đây là 1 năm. Nhiều trình duyệt hiện đại sẽ từ chối các chứng chỉ có thời gian hợp lệ quá 1 năm.
- `newkey rsa:2048`**:** Tạo một chứng chỉ mới và một khóa mới cùng lúc. Vì chúng ta chưa tạo sẵn khóa để ký chứng chỉ, nên cần tạo cùng lúc. Phần rsa:2048 chỉ định tạo một khóa RSA dài 2048 bit.
- `keyout`**:** Chỉ định nơi lưu file khóa riêng (Private Key) được tạo.
- `out`**:** Chỉ định nơi lưu chứng chỉ được tạo.

![image.png](/Images/tuan_3_webserver/image%2030.png)

**Bước 2. Cập nhật file cấu hình VirtualHost**

Thêm cấu hình cho https (port 443) như sau:

Trong đó:

```
server {
listen 443 ssl http2;
server_name your_domain;
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key ;
root /var/www/your_domain/html/nginx;
index index.html;
}
```

- `listen 443 ssl http2`;
    - `443`: Cổng HTTPS.
    - `ssl`: Kích hoạt SSL/TLS cho kết nối an toàn.
    - `http2`: Bật giao thức HTTP/2 để tăng hiệu năng.
- `server_name yourdomain.local`;
    - `yourdomain.local:` Tên miền mà khối này xử lý.
- `ssl_certificate /etc/nginx/ssl/yourdomain.crt`;
    - `/etc/nginx/ssl/yourdomain.crt`: Đường dẫn đến tệp chứng chỉ SSL công khai.
- `ssl_certificate_key /etc/nginx/ssl/yourdomain.key`;
    - `/etc/nginx/ssl/yourdomain.key`: Đường dẫn đến tệp khóa riêng SSL.
- `root /var/www/yourdomain`;
    - `/var/www/yourdomain`: Thư mục gốc chứa tệp tĩnh để phục vụ.
- `index index.html`;
    - `index.html`: Tệp mặc định được phục vụ khi truy cập thư mục gốc.

Ví dụ:

![image.png](/Images/tuan_3_webserver/image%2031.png)

**Bước 3: Chuyển hướng HTTP sang HTTPS**

Hiện tại, cấu hình của bạn chỉ phản hồi các yêu cầu HTTPS trên cổng 443. Thực hành tốt là phản hồi trên cổng 80, ngay cả khi bạn muốn buộc toàn bộ lưu lượng truy cập được mã hóa. Hãy thiết lập một VirtualHost để phản hồi các yêu cầu không mã hóa này và chuyển hướng chúng sang HTTPS.

Mở lại file cấu hình NGINX mà ta đã tạo:

```
sudo nano /etc/nginx/sites-available/your_domain_or_ip.conf
```

Ở cuối file, tạo thêm một khối VirtualHost để xử lý các yêu cầu trên cổng 80. Sử dụng chỉ thị ServerName để khớp với tên miền hoặc địa chỉ IP của bạn, sau đó dùng lệnh Redirect để chuyển hướng tất cả các yêu cầu sang VirtualHost TLS. Hãy nhớ thêm dấu gạch chéo ở cuối:

![image.png](/Images/tuan_3_webserver/image%2032.png)

Sau đó khởi động lại NGINX:

```
sudo systemctl restart nginx
```

Để kiểm tra, bạn vào trình duyệt bất kỳ tìm kiếm `http://your_domain` , nếu hoạt động đúng thì trình duyệt sẽ tự động chuyển hướng qua `https://your_domain` như khi tìm kiếm `https://your_domain`

Đến đây ta đã cơ bản cấu hình SSL cho VirtualHost thành công rồi.

### Cấu hình NGINX làm proxy

Theo mặc định, máy chủ web Apache lắng nghe trên cổng 80. Muốn Nginx làm proxy cho Apache thì cần phảithay đổi cổng mặc định của Apache thành 8080. Bạn có thể thay đổi nó bằng cách chỉnh sửa tệp sau:

```
nano /etc/apache2/ports.conf
```

Tìm dòng sau:

```
Listen 80
Listen 443
```

Và, thay thế nó bằng dòng sau:

```
Listen 8080
Listen 8443
```

![image.png](/Images/tuan_3_webserver/image%2033.png)

Lưu và đóng tập tin khi bạn hoàn tất. Tiếp theo, bạn cũng sẽ cần chỉnh sửa tệp cấu hình tương ứng với VirtualHost.

Bạn có thể chỉnh sửa nó bằng lệnh sau:

```
nano /etc/apache2/sites-available/your_domain.conf
```

![image.png](/Images/tuan_3_webserver/image%2034.png)

Lưu và khởi động lại dịch vụ Apache:

```
sudo systemctl restart apache2
```

Tại thời điểm này, Apache được khởi động và lắng nghe trên cổng 8080. Bạn có thể kiểm tra nó bằng lệnh sau:

```
ss -antpl | grep apache2
```

![image.png](/Images/tuan_3_webserver/image%2035.png)

Vậy là xong bên phía Apache, giờ ta qua NGINX sửa file cấu hình `/etc/nginx/sites-available/your_domain.conf`

```
nano /etc/nginx/sites-available/your_domain.conf
```

![image.png](/Images/tuan_3_webserver/image%2036.png)

Ta bỏ đi phần cấu hình thư mục tĩnh mặc định (`root`) và thay thế bằng đoạn `location` 

**Trong đó:**

- `proxy_pass http://192.168.0.100:8080;`: Chuyển tiếp yêu cầu tới Apache đang chạy trên `192.168.0.100` port `8080`, có thể thay thế bằng `127.0.0.1:8080` nếu bạn muốn chạy trên `localhost`.
- `proxy_set_header Host $host;`: Gửi tên domain gốc của client đến Apache (giữ nguyên `Host` header).
- `proxy_set_header X-Real-IP $remote_addr;`: Gửi địa chỉ IP thực của client đến Apache.
- `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`: Gửi chuỗi IP forwarding (theo dõi IP qua nhiều proxy) đến Apache.

Lưu là kiểm tra lại bằng lệnh:

```
nginx -t
```

![image.png](/Images/tuan_3_webserver/image%2037.png)

Sau đó khởi động lại NGINX:

```
sudo systemctl restart nginx
```

Thử kiểm tra bằng cách tìm kiếm ở trình duyệt bất kỳ [`http://your_domain`](http://your_domain) thì trình duyệt sẽ tự chuyển sang https://your_domain

![image.png](/Images/tuan_3_webserver/image%2038.png)

Hoặc cũng so thể kiêm tra bằng lệnh:

```
curl -k https://your_domain_or_ip:8443 
```

![image.png](/Images/tuan_3_webserver/image%2039.png)

Tới đây thì ta đã cấu hình thành công NGINX proxy cho Apache

# OpenLiteSpeed

### Cài đặt OpenLiteSpeed

Bước 1: Thêm kho lưu trữ LiteSpeed 

```
wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh 
| sudo bash
```

![image.png](/Images/tuan_3_webserver/image%2040.png)

**Bước 2: Cài đặt.**

Với kho lưu trữ được thêm vào, bạn có thể cài đặt máy chủ web OpenLiteSpeed trên

Bạn cần đảm bảo danh sách APT được cập nhật trước khi cài đặt xong.

```
sudo apt update
sudo apt install openlitespeed
```

**Bước 3: Cài đặt PHP** 

Các lệnh bên dưới sẽ cài đặt PHP 7.4 với tất cả các gói được sử dụng phổ biến cho OpenLiteSpeed từ Debian Repo của LiteSpeed. Sau khi cài đặt, symlink được tạo sẽ hướng dẫn OpenLiteSpeed sử dụng PHP đã cài đặt.

```
sudo apt install lsphp74
```

![image.png](/Images/tuan_3_webserver/image%2041.png)

```
sudo ln -sf /usr/local/lsws/lsphp74/bin/lsphp /usr/local/lsws/fcgi-bin/lsphp5
```

Để khởi động máy chủ, chúng ta sẽ chạy `/usr/local/lsws/bin/lswsctrl start` và dừng lại nó, chúng ta sẽ chạy `/usr/local/lsws/bin/lswsctrl stop`

```
$ sudo /usr/local/lsws/bin/lswsctrl start
[OK] Send SIGUSR1 to 94667
```

![image.png](/Images/tuan_3_webserver/image%2042.png)

**Bước 4: Đặt mật khẩu quản trị**

Để đăng nhập vào bảng điều khiển DirectAdmin, chúng ta phải tạo Mật khẩu quản trị. OpenLitespeed cung cấp một tập lệnh để hướng dẫn chúng tôi một cách khéo léo trong phần này. Chỉ cần chạy lệnh bên dưới và điền thông tin chi tiết khi được nhắc.

```
sudo /usr/local/lsws/admin/misc/admpass.sh
```

![image.png](/Images/tuan_3_webserver/image%2043.png)

**Bước 5: Truy cập OpenLiteSpeed Web Admin**

Cổng mặc định mà bảng điều khiển WebAdmin lắng nghe là 7080. Trỏ trình duyệt của bạn tới ***http://your-server-ip:7080*** và bạn sẽ nhận được trang đăng nhập như được hiển thị dưới. Nhập tên người dùng và mật khẩu bạn vừa tạo.

![image.png](/Images/tuan_3_webserver/image%2044.png)

![image.png](/Images/tuan_3_webserver/image%2045.png)

Theo mặc định, máy chủ ảo OpenLiteSpeed chấp nhận các kết nối trên cổng 8088. Nếu trỏ trình duyệt của mình tới cổng đó, bạn sẽ thấy một trang như bên dưới:

![image.png](/Images/tuan_3_webserver/image%2046.png)

Để kiểm tra xem Webserver đã hoạt động đúng chưa, mình sẽ trỏ trình duyệt của mình tới `http://your-server-ip:8088/file-name`

Tạo một tệp thử nghiệm php mẫu và thêm đoạn mã html vào:

```
$ sudo nano /usr/local/lsws/Example/html/test.php
```

```
<html>
    <head>
        <?php
        echo '<title>Sample PHP Script</title>';
        ?>
</head>
<body>
    <?php
    echo '<p>This is to confirm that our PHP is working</p>';
    ?>
    <h1>OpenLiteSpeed</h1>
    <p>OpenLiteSpeed is a high-performance, lightweight, open source HTTP
            server edition of LiteSpeed Web Server Enterprise</p>
</body>
</html>
```

![image.png](/Images/tuan_3_webserver/image%2047.png)

Trỏ trình duyệt  tới `http://your-server-ip:8088/test.php`

![image.png](/Images/tuan_3_webserver/image%2048.png)

Ta cũng có thể đổi port mặc định của webserver này:

![image.png](/Images/tuan_3_webserver/image%2049.png)

Sau tùy chỉnh port thì nhấn lưu → restart lại webserver

![image.png](/Images/tuan_3_webserver/image%2050.png)

![image.png](/Images/tuan_3_webserver/image%2051.png)

Kiểm tra lại với port vừa đổi:

![image.png](/Images/tuan_3_webserver/e54043e3-4158-4575-9378-a72cb8092267.png)

### Tạo VitualHost

**Bước 1: Bạn truy cập vào Virtual Hosts => nhấn vào biểu tượng dấu + để tiến hành thêm Vhost.**

![image.png](/Images/tuan_3_webserver/image%2052.png)

**Bước 2: Điền thông tin VirtualHost, có thể tham khảo như sau:**

![image.png](/Images/tuan_3_webserver/image%2053.png)

Trong đó:

- `$SERVER_ROOT` : là biến nội bộ do OpenLiteSpeed định nghĩa, thường trỏ đến thư mục `/usr/local/lsws`
- `Virtual Host Name` **:** Tên định danh của Virtual Host (thường là tên domain).
- `Virtual Host Root`**:** Thư mục gốc chứa file cấu hình và thông tin của Virtual Host.
- `Config File`**:** Đường dẫn tới file `.conf` chứa cấu hình chi tiết của Virtual Host.
- `Follow Symbolic Link`**:** Cho phép theo liên kết mềm (symlink) nếu đặt là Yes.
- `Enable Scripts/ExtApps`**:** Bật hỗ trợ PHP hoặc các ứng dụng ngoài như CGI, FastCGI (nên bật).
- `Restrained` **:** Nếu bật, hạn chế quyền truy cập/điều khiển cho virtual host (ít khi dùng).
- `External App Set UID Mode`**:** Nếu bật, hạn chế quyền truy cập/điều khiển cho virtual host (ít khi dùng).

Sau khi điền xong thì nhấn nút lưu thông tin.

**Bước 3: Tùy chỉnh chung**

Trở ra tab **Virtual Hosts →** nhấn vào biểu thượng kính lúp của Virtual Host vừa tạo.

![image.png](/Images/tuan_3_webserver/image%2054.png)

Chuyển qua tab **General →** ở mục đầu tiên (General) nhấn vào biểu tượng chỉnh sửa bên góc phải.

![image.png](/Images/tuan_3_webserver/image%2055.png)

Điền thông tin chung về Virtual Host, có thể tham khảo mẫu như sau:

![image.png](/Images/tuan_3_webserver/image%2056.png)

Trong đó:

- `Document Root*`: Đường dẫn đến thư mục chứa nội dung website (HTML, PHP…) được hiển thị ra ngoài.
- `Domain Name`**:** Tên domain chính mà Virtual Host này phục vụ (VD: `example.com`).
- `Enable GZIP Compression`**:** Bật nén GZIP giúp tăng tốc tải trang bằng cách nén dữ liệu gửi về client.

Sau khi hiệu chỉnh thì nhấn nút lưu.

![image.png](/Images/tuan_3_webserver/image%2057.png)

Trở ra, ở mục **Index Files** nhấn nút chỉnh sửa:

**Auto Index: No** – Không hiển thị danh sách file nếu thư mục không có file index (giúp bảo mật).

![image.png](/Images/tuan_3_webserver/image%2058.png)

Xong lưu thông tin lại.

Khởi động lại LiteSpeed

![image.png](/Images/tuan_3_webserver/image%2059.png)

**Bước 4: Tạo Listener mới**

Mở tab **Listeners** → nhấn vào hình dấu + bên phải:

![image.png](/Images/tuan_3_webserver/image%2060.png)

Điền thông tin để tạo 1 Listener mới, có thẻ tham khảo như sau:

![image.png](/Images/tuan_3_webserver/image%2061.png)

Trong đó:

- `Listener Name`**:** Tên định danh cho listener (ví dụ: `HTTP`, `HTTPS`, `MyListener`).
- `IP Address`**:** Địa chỉ IP mà listener sẽ lắng nghe (thường để `ANY` để nhận mọi IP).
- `Port`**:** Cổng mà listener sử dụng (ví dụ: `80` cho HTTP, `443` cho HTTPS, ở đây tôi chọn port `4443`).
- `Secure`**:** Đặt **Yes** nếu là listener HTTPS (yêu cầu SSL), **No** nếu là HTTP bình thường.

Tùy chỉnh xong thì lưu lại thông tin.

Sau khi nhấn lưu thông tin trên, sẽ trở tra tab General → nhấn vào biểu tượng dấu cộng  ở mục Virtual Host Mappings

![image.png](/Images/tuan_3_webserver/image%2062.png)

Chọn VirtualHost vừa tạo và nhập domain tương ứng:

![image.png](/Images/tuan_3_webserver/image%2063.png)

![image.png](/Images/tuan_3_webserver/image%2064.png)

Nhập xong thì nhấn nút lưu thông tin

Restart lại LiteSpeed.

![image.png](/Images/tuan_3_webserver/image%2065.png)

**Bước 5: Tạo thư mục chứa website**

Thư mục gốc chứa file cấu hình và thông tin của Virtual Host đã tạo và khai báo trước đó. Ở trường hợp này thì domain của minh là `thienduonglitespeed.com`

```
sudo mkdir /usr/local/lsws/your_domain/
sudo mkdir /usr/local/lsws/your_domain/public_html
```

Và kiểm tra lại

![image.png](/Images/tuan_3_webserver/image%2066.png)

Mình sẽ tạo một file index.html mẫu để kiểm tra với lệnh **`nano`**, nội dung bên trong bạn có thể thêm nội dung theo ý muốn.

```
sudo nano /usr/local/lsws/thienduonglitespeed/public_html/index.html
```

Nội dung file index của mình:

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Chào mừng tới ThienDuongLiteSpeed</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f4f4f4;
                color: #333;
                margin: 0;
                padding: 0;
                display: flex;
                align-items: center;
                justify-content: center;
                height: 100vh;
            }
            .welcome-container {
                text-align: center;
            }
            h1 {
                color: #007bff;
            }
            p {
                font-size: 18px;
            }
        </style>
    </head>
    <body>
        <div class="welcome-container">
        <h1>Chào mừng bạn đến với ThienDuongLiteSpeed</h1>
        <p>Chúng tôi cung cấp các dịch vụ hosting chất lượng cao và hỗ trợ khách hàng tận tâm.</p>
        <p>Hãy khám phá những điều tuyệt vời mà chúng tôi mang lại cho bạn.</p>
        </div>
    </body>
</html>
```

Lưu file index.html và bắt đầu kiểm tra bằng cách tim kiếm trên trình duyệt bất kỳ với `http://your_domain_or_ip:<port>`

![image.png](/Images/tuan_3_webserver/image%2067.png)

Tới đây là ta đã tạo thành công VirtualHost rồi

### Cấu hình SSL cho VirtualHost

[Hướng dẫn cài đặt SSL OpenLite Speed với 2 bước](https://dotrungquan.info/cai-dat-ssl-openlite-speed/)

**Bước 1. Tạo khóa SSL tự ký**

Ta sẽ tạo chứng chỉ tự ký bằng openssl, nếu như bạn chưa cài đặt gói openssl thì có thể chạy lệnh sau:

```
sudo apt install openssl
```

Sau khi cài xong ta tiến hành tạo khóa:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 
-keyout /etc/ssl/private/litespeed-selfsigned.key 
-out /etc/ssl/certs/litespeed-selfsigned.crt
```

Trong đó:

- `openssl`**:** Công cụ dòng lệnh để tạo và quản lý chứng chỉ OpenSSL, khóa và các file liên quan.
- `req -x509`**:** Xác định rằng chúng ta sử dụng X.509 certificate signing request (CSR) cho việc quản lý. X.509 là một tiêu chuẩn cơ sở hạ tầng khóa công khai mà TLS tuân theo để quản lý khóa và chứng chỉ.
- `nodes`**:** Cho OpenSSL biết bỏ qua bước bảo vệ chứng chỉ bằng passphrase. Chúng ta cần Apache có thể đọc file mà không cần can thiệp của người dùng khi khởi động máy chủ. Nếu có passphrase, chúng ta sẽ phải nhập lại sau mỗi lần khởi động.
- `days 365`**:** Thiết lập thời gian chứng chỉ được coi là hợp lệ, ở đây là 1 năm. Nhiều trình duyệt hiện đại sẽ từ chối các chứng chỉ có thời gian hợp lệ quá 1 năm.
- `newkey rsa:2048`**:** Tạo một chứng chỉ mới và một khóa mới cùng lúc. Vì chúng ta chưa tạo sẵn khóa để ký chứng chỉ, nên cần tạo cùng lúc. Phần rsa:2048 chỉ định tạo một khóa RSA dài 2048 bit.
- `keyout`**:** Chỉ định nơi lưu file khóa riêng (Private Key) được tạo.
- `out`**:** Chỉ định nơi lưu chứng chỉ được tạo.

![image.png](/Images/tuan_3_webserver/image%2068.png)

Ở tab **Listeners** nhấn vào biểu tượng kính lúp ở **Listener** cho **VirtualHost** nào bạn muốn cấp SSL cho:

![image.png](/Images/tuan_3_webserver/image%2069.png)

Ở phần **General** nhấn vào chỉnh sửa ở mục **Address Settings:**

![image.png](/Images/tuan_3_webserver/image%2070.png)

Tích vào ô **Yes** của **Secure** và nhấn nút lưu**:**

![image.png](/Images/tuan_3_webserver/image%2071.png)

Sau khi nhấn lưu chuyển qua phần **SSL,** nhấn chỉnh sửa ở mục **SSL Private Keyy & Certificate:**

![image.png](/Images/tuan_3_webserver/image%2072.png)

Chỉnh thông tin cho phù hợp:

![image.png](/Images/tuan_3_webserver/image%2073.png)

Trong đó 

- `Private Key File`: là đường dẫn nơi lưu trữ Key
- `Certificate File`: là đường dẫn nơi lưu trữ chứng chỉ
- `Chained Certificate` **: Yes** nếu file chứng chỉ đã bao gồm cả intermediate CA và ngược lại

Sau đó nhấn lưu, trở ra nhấn chỉnh sửa mục SSL Protocol:

![image.png](/Images/tuan_3_webserver/image%2074.png)

Tích hết các Protocol Version để đáp ứng nhiều trình duyệt nhất có thể:

![image.png](/Images/tuan_3_webserver/image%2075.png)

Sau đó nhấn lưu, và restart lại LiteSpeed:

![image.png](/Images/tuan_3_webserver/image%2076.png)

Cuối cùng là kiểm tra bằng cách tìm kiếm trên trình duyệt bất kỳ `https:your_domain_or_ip:<port>`

![image.png](/Images/tuan_3_webserver/image%2077.png)

Tới đây, ta đã cấp chứng chỉ SSL tự ký cho VirtualHost LiteSpeed 

## 3. Quản lý trạng thái của các Webserver

### Quản lý trạng thái của Apache

Như đã khai báo ở VirtualHost, thì file log lỗi sẽ nằm ở `/var/log/apache2/error.log` và file log tất cả truy cập đến website này ở  `/var/log/apache2/access.log`

![image.png](/Images/tuan_3_webserver/image%2078.png)

Đây là khi ta thực hiện kiểm tra file tất cả truy cập tới máy chủ qua apache: 

![image.png](/Images/tuan_3_webserver/image%2079.png)

Đây là khi ta thực hiện kiểm tra file log lỗi

![image.png](/Images/tuan_3_webserver/image%2080.png)

### Quản lý trạng thái của NGINX

Ta có thể kiểm tra được những truy cập tới webserver NGINX thông qua file `/var/log/nginx/access.log`

```
cat /var/log/nginx/access.log
```

![image.png](/Images/tuan_3_webserver/image%2081.png)

Nếu xảy ra lỗi thì có thể kiểm tra chi tiết lỗi ở `/var/log/nginx/error.log`

```
cat /var/log/nginx/error.log
```

![image.png](/Images/tuan_3_webserver/image%2082.png)

### Quản lý trạng thái của OpenLiteSpeed

Ta có thể kiểm tra được những truy cập tới và  kiểm tra chi tiết lỗi ở các file được cấu hình trong tab **Server Configuration** → **Log**

![image.png](/Images/tuan_3_webserver/image%2083.png)

Ở đây có 2 phần gồm Server Log và Access Log. Server Log là file ghi lại những sự cố phía máy chủ, chẳng hạn như cấu hình sai, lỗi ứng dụng hoặc sự cố backend server. Server Log rất quan trọng để xác định và xử lý sự cố máy chủ. Còn lại là Access Log là file ghi lại những truy cập đến webserver. 

Cũng có thể cấu hình đường dẫn file khác đi của 2 file này.

Ngoài ra với mỗi VirtualHost cũng có thể cấu hình Virtual Host log và Access Log riêng.
Vào tab **Virtual Hosts →** chọn Virtual Host cần thêm file log:

![image.png](/Images/tuan_3_webserver/image%2084.png)

Nhấn qua mục **Log** → biểu tượng chỉnh sửa ở 2 phần log và thêm đường dẫn tương ứng:

![image.png](/Images/tuan_3_webserver/image%2085.png)

Kiểm tra truy cập từ client về Webserver bằng lệnh sau:

```
cat /usr/local/lsws/logs/access.log
```

![image.png](/Images/tuan_3_webserver/image%2086.png)

Kiểm tra lỗi máy chủ, PHP, cấu hình sai, cảnh báo:

```
cat /usr/local/lsws/logs/error.log
```

![image.png](/Images/tuan_3_webserver/image%2087.png)

### Lỗi trong quá trình cài đặt

Khi cài đặt nhiều webserver và các webserver này cùng chạy song song, dễ xảy ra trường hợp lỗi do trùng `port`

Ví dụ như khi vừa cài đặt Apache xong, sau đó cài NGINX và khởi chạy sinh ra lỗi như sau:

Thì lỗi này là do port 80 và port 443 đã được Apache sư dụng từ trước. Để giải quyết vấn đề này thì đơn giản nhất là ta đổi port mặc định của 1 trong 2 webserver này.

![image.png](/Images/tuan_3_webserver/image%2088.png)

![image.png](/Images/tuan_3_webserver/image%2089.png)

Ở đây mình đổi port Apache từ 80 → 8080 và 443 → 8443 để giải quyết vấn đề trùng port và thuận tiện cho việc cấu hình NGINX proxy sau này.

![image.png](/Images/tuan_3_webserver/image%2090.png)

---
THE END