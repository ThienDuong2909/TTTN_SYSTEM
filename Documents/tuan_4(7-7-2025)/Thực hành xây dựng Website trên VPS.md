# Thực hành xây dựng website chạy trên VPS

# Mục lục
  - [LEMP, LAMP Stack](#lemp-lamp-stack)
    - [Giới thiệu](#giới-thiệu)
    - [Cài đặt](#cài-đặt)
  - [LarVPS](#larvps)
    - [Giới thiệu về LarVPS](#giới-thiệu-về-larvps)
    - [Cài đặt LarVPS](#cài-đặt-larvps)
    - [Thao tác với LarVPS](#thao-tác-với-larvps)
  - [Centmin Mod](#centmin-mod)
    - [Giới thiệu về Centmin Mod](#giới-thiệu-về-centmin-mod)
    - [Cài đặt Centmin Mod](#cài-đặt-centmin-mod)
    - [Thao tác với Centmin mod](#thao-tác-với-centmin-mod)
  - [MERN/MEAN Stack](#mernmean-stack)
# LEMP, LAMP Stack

## Giới thiệu

**LAMP** là viết tắt của **L**inux, **A**pache, **M**ySQL và **P**HP (cũng có thể là Python, Perl nhưng bài này chỉ nói về Php), mỗi trong số đó là các gói phần mềm riêng lẻ được kết hợp để tạo thành một giải pháp máy chủ web linh hoạt. Các thành phần này, được sắp xếp theo các lớp hỗ trợ lẫn nhau, tạo thành các stack phần mềm.

**Stack của LAMP**

- **Linux:** là lớp đầu tiên trong stack. Hệ điều hành này là cơ sở nền tảng cho các lớp phần mềm khác.
- **Apache** đóng vai trò một HTTP server dùng để xử lý các yêu cầu gửi tới máy chủ.
- **Mysql** là cơ sở dữ liệu để lưu trữ mọi thông tin trên website.
- **PHP** sau đó sẽ xử lý các nhiệm vụ cần thiết hoặc kết nối với CSDL MySQL để lấy thông tin cần thiết sau đó trả về cho Apache. Apache cuối cùng sẽ trả kết quả nhận được về cho máy khách đã gửi yêu cầu tới.

Các thành phần cấu thành **LEMP** stack cũng gần tương tự với **LAMP**, chỉ khác là Apache sẽ được thay thế bởi **Nginx**. **Nginx** được đọc là “engine-x”, giải thích cho chữ E trong “LEPM”. Nginx có ưu điểm là cho phép xử lý tốc độ tải cao hơn đối với các HTTP request.

Hiện tại, Nginx đã đạt được thành tựu đáng kể khi nó bắt đầu được nhiều người sử dụng từ năm 2008 và hiện trở thành ứng dụng web server tiếng tăm thứ 2 sau Apache.

## Cài đặt

### **Cài đặt Apache**

Nếu đây là lần đầu tiên bạn sử dụng `sudo` trong phiên này, bạn sẽ được nhắc cung cấp mật khẩu người dùng để xác nhận rằng bạn có đặc quyền phù hợp để quản lý các gói hệ thống với `apt`:

```
sudo apt update
```

Sau đó, cài đặt Apache với:

```
sudo apt install apache2
```

Bạn sẽ được nhắc xác nhận cài đặt Apache. Xác nhận bằng cách nhấn `Y`, sau đó `ENTER`.

Sau khi quá trình cài đặt hoàn tất, bạn sẽ cần điều chỉnh cài đặt tường lửa của mình để cho phép lưu lượng HTTP. Công cụ cấu hình tường lửa mặc định của Ubuntu được gọi là Tường lửa không phức tạp (UFW). Nó có các cấu hình ứng dụng khác nhau mà bạn có thể tận dụng. Để liệt kê tất cả các cấu hình ứng dụng UFW hiện có, thực hiện lệnh này:

```
sudo ufw app list
```

Dưới đây là ý nghĩa của từng hồ sơ này:

- **Apache**: Cấu hình này chỉ mở cổng `80` (lưu lượng truy cập web bình thường, không được mã hóa).
- **Apache Full**: Cấu hình này mở cả cổng `80` (lưu lượng truy cập web bình thường, không được mã hóa) và cổng `443` (lưu lượng truy cập được mã hóa TLS/SSL).
- **Apache Secure**: Cấu hình này chỉ mở cổng `443` (lưu lượng được mã hóa TLS/SSL).

Hiện tại, tốt nhất chỉ cho phép kết nối trên cổng `80`, vì đây là bản cài đặt Apache mới và bạn chưa định cấu hình chứng chỉ TLS/SSL để cho phép lưu lượng HTTPS trên máy chủ của mình.

Để chỉ cho phép lưu lượng truy cập trên cổng `80`, hãy sử dụng cấu hình `Apache`:

```
sudo ufw allow in "Apache"
```

Xác minh thay đổi với:

```
sudo ufw status
```

![image.png](/Images/tuan_4_thxdwvps/image.png)

Lưu lượng truy cập trên cổng `80` hiện được phép thông qua tường lửa.

Bạn có thể thực hiện kiểm tra ngay lập tức để xác minh rằng mọi thứ diễn ra theo đúng kế hoạch bằng cách truy cập địa chỉ IP công cộng của máy chủ trong trình duyệt web của bạn (xem ghi chú bên dưới tiêu đề tiếp theo để biết địa chỉ IP công cộng của bạn là gì nếu bạn không có thông tin này đã):

```
http://your_server_ip
```

![image.png](/Images/tuan_4_thxdwvps/image%201.png)

### **Cài đặt MySQL:**

sử dụng `apt` để tải và cài đặt phần mềm này:

Đầu tiên, mở dấu nhắc MySQL:

```
sudo mysql
```

```
sudo apt install mysql-server
```

![image.png](/Images/tuan_4_thxdwvps/image%202.png)

Để tránh đi vào vòng lặp đệ quy này, trước tiên bạn cần điều chỉnh cách xác thực người dùng MySQL **root** của mình.Sau đó chạy lệnh `ALTER USER` sau để thay đổi phương thức xác thực của người dùng root thành phương thức sử dụng mật khẩu. Ví dụ sau thay đổi phương thức xác thực thành `mysql_native_password`

![image.png](/Images/tuan_4_thxdwvps/image%203.png)

Sau đó, bạn có thể chạy tập lệnh mysql_secure_installation mà không gặp vấn đề gì.

Bắt đầu tập lệnh tương tác bằng cách chạy lệnh dưới đây và cấu hình nhập thông tin:

```
sudo mysql_secure_installation
```

![image.png](/Images/tuan_4_thxdwvps/image%204.png)

![image.png](/Images/tuan_4_thxdwvps/image%205.png)

Khi bạn hoàn tất, hãy kiểm tra xem bạn có thể đăng nhập vào bảng điều khiển MySQL hay không bằng cách gõ:

![image.png](/Images/tuan_4_thxdwvps/image%206.png)

### Cài đặt PHP

Để cài đặt các gói này, hãy chạy lệnh sau:

```
sudo apt install php libapache2-mod-php php-mysql
```

![image.png](/Images/tuan_4_thxdwvps/image%207.png)

Sau khi quá trình cài đặt hoàn tất, hãy chạy lệnh sau để xác nhận phiên bản PHP của bạn:

```
php -v
```

![image.png](/Images/tuan_4_thxdwvps/image%208.png)

Tại thời điểm này, LAMP stack của bạn đã hoạt động hoàn toàn, nhưng trước khi thử nghiệm thiết lập của bạn bằng tập lệnh PHP, tốt nhất bạn nên thiết lập máy chủ ảo Apache thích hợp để lưu giữ các tệp và thư mục trên trang web của bạn.

### Tạo một máy chủ ảo cho website

Tạo thư mục cho `your_domain` như sau:

```
sudo mkdir /var/www/your_domain
```

Tiếp theo, gán quyền sở hữu thư mục với biến môi trường `$USER`, biến này sẽ tham chiếu đến người dùng hệ thống hiện tại của bạn:

```bash
sudo chown -R $USER:$USER /var/www/your_domain
```

![image.png](/Images/tuan_4_thxdwvps/image%209.png)

Sau đó, mở tệp cấu hình mới trong thư mục có sẵn trên các trang của Apache bằng trình soạn thảo dòng lệnh ưa thích của bạn. Ở đây, chúng ta sẽ sử dụng `nano`:

```
sudo nano /etc/apache2/sites-available/your_domain.conf
```

Điều này sẽ tạo ra một tập tin trống mới. Thêm vào cấu hình cơ bản sau bằng tên miền của riêng bạn:

```
/etc/apache2/sites-available/your_domain.conf
```

```
<VirtualHost *:80>
    ServerNameyour_domain
    ServerAlias www.your_domain
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

![image.png](/Images/tuan_4_thxdwvps/image%2010.png)

Lưu và đóng tệp khi bạn hoàn tất. Nếu bạn đang sử dụng `nano`, hãy thực hiện điều đó bằng cách nhấn `CTRL X`, sau đó nhấn `Y` và `ENTER`.

Bây giờ, hãy sử dụng `a2ensite` để kích hoạt máy chủ ảo mới:

```
sudo a2ensiteyour_domain
```

Bạn có thể muốn tắt trang web mặc định được cài đặt cùng với Apache. Điều này là bắt buộc nếu bạn không sử dụng tên miền tùy chỉnh vì trong trường hợp này cấu hình mặc định của Apache sẽ ghi đè máy chủ ảo của bạn. Để tắt trang web mặc định của Apache, hãy nhập:

```
sudo a2dissite 000-default
```

Để đảm bảo tệp cấu hình của bạn không chứa lỗi cú pháp, hãy chạy lệnh sau:

```
sudo apache2ctl configtest
```

Cuối cùng, tải lại Apache để những thay đổi này có hiệu lực:

```
sudo systemctl reload apache2
```

![image.png](/Images/tuan_4_thxdwvps/image%2011.png)

Trang web mới của bạn hiện đang hoạt động nhưng web root `/var/www/your_domain` vẫn trống. Tạo tệp `index.html` ở vị trí đó để kiểm tra xem máy chủ ảo có hoạt động như mong đợi không:

```
nano /var/www/your_domain/index.html
```

Bao gồm nội dung sau trong tập tin này:

```
/var/www/your_domain/index.html
```

```
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

Lưu và đóng tệp, sau đó truy cập trình duyệt của bạn và truy cập tên miền hoặc địa chỉ IP của máy chủ của bạn:

```
http://server_domain_or_IP
```

Trang web của bạn sẽ phản ánh nội dung trong tệp bạn vừa chỉnh sửa:

![image.png](/Images/tuan_4_thxdwvps/image%2012.png)

### **Kiểm tra quá trình PHP trên Web Server**

Bây giờ bạn đã có một vị trí tùy chỉnh để lưu trữ các tệp và thư mục trên trang web của mình, hãy tạo tập lệnh kiểm tra PHP để xác nhận rằng Apache có thể xử lý và xử lý các yêu cầu đối với tệp PHP.

Tạo một tệp mới có tên `info.php` bên trong thư mục gốc web tùy chỉnh của bạn:

```
nano /var/www/your_domain/info.php
```

Điều này sẽ mở một tập tin trống. Thêm văn bản sau, là mã PHP hợp lệ, vào bên trong tệp:

```
/var/www/your_domain/info.php
```

```
<?php
phpinfo();
```

Khi bạn hoàn tất, hãy lưu và đóng tệp.

Để kiểm tra tập lệnh này, hãy truy cập trình duyệt web của bạn và truy cập tên miền hoặc địa chỉ IP của máy chủ, theo sau là tên tập lệnh, trong trường hợp này là `info.php`:

```
http://server_domain_or_IP/info.php
```

![image.png](/Images/tuan_4_thxdwvps/image%2013.png)

# LarVPS

## Giới thiệu về LarVPS

**LarVPS** là một dạng Script quản lý VPS, nó được tích hợp tự động cài đặt và tối ưu Nginx – PHP – MariaDB cho VPS. Do LarVPS là dạng Script chỉ hoạt động trên SSH , nên tiêu thụ tài nguyên máy chủ vô cùng ít. LarVPS được tích hợp nhiều chức năng hiển thị bằng Menu thân thiện và dễ hiểu.

Hiện tại LarVPS đang ngày càng được nhiều người dùng VPS tại Việt Nam tin dùng vì nó được phát triển bởi chính người Việt, điều này mang lại sự tin tưởng và đem lại một cộng đồng đông đảo cho người dùng tại Việt Nam. Ở LarVPS được chia thành các cấp độ từ cơ bản đến nâng cao tương ứng với bản miễn phí và trả phí. Các bạn có thể xem chi tiết bảng giá cũng như các tính năng đi kèm của từng gói ở link bên dưới.

## Cài đặt LarVPS

Đầu tiên chúng ta cần cập nhật lại gói và nâng cấp các gói hiện có:

```bash
sudo apt update
sudo apt upgrade
```

Tiếp là tải và cài đặt LarVPS bằng lệnh:

```bash
curl -sO https://larvps.com/scripts/larvps && bash larvps
```

Sau đó nó sẽ yêu cầu reboot lại máy chủ

Cuối cùng máy chủ chạy lại hãy chạy lệnh sau với quyền root để vào trang quản lý script LarVPS:

```bash
larvps
```

![image.png](/Images/tuan_4_thxdwvps/image%2014.png)

Đây là giao diện quản lý của LarVPS:

![image.png](/Images/tuan_4_thxdwvps/image%2015.png)

## Thao tác với LarVPS

**1. Cấu hình thêm Domain mới:**

Ở giao diện quản lý **LarVPS** → nhấn số **1** → **Enter**

![image.png](/Images/tuan_4_thxdwvps/image%2016.png)

Khi vào được **Quản lý Domain** → nhấn phím **2** → **Enter**

![image.png](/Images/tuan_4_thxdwvps/image%2017.png)

Vào giao diện **Thêm tên miền** → Nhập tên miền → Tùy chọn cài đặt **Wordpress** → Chọn phiên bản **PHP** → Xem lại thông tin → Xác nhận thêm **Domain**

![image.png](/Images/tuan_4_thxdwvps/image%2018.png)

**2. Cài chứng chỉ SSL**

https://damtrungkien.com/cai-dat-chung-chi-ssl-co-phi-tren-larvps/

**Thao tác cài chứng chỉ miễn phí (Let’s Encrypt)**

**Bước 1:** Vào giao diện quản lý chính của **LarVPS** bằng quyền **root** và nhập số **2 + enter** để vào giao diện **Quản lý SSL**:

```bash
larvps
```

![image.png](/Images/tuan_4_thxdwvps/image%2019.png)

**Bước 2:** Sau khi vào được giao diện Quản lý SSL thỉ ta nhập **1 + enter** để thêm SSL miễn phí 

![image.png](/Images/tuan_4_thxdwvps/image%2020.png)

**Bước 3:** Chọn tên miền cần cài chứng chỉ SSL

![image.png](/Images/tuan_4_thxdwvps/image%2021.png)

**Bước 4:** Xác nhận lại và tiến hành cài SSL

Sau khi xác nhận lại domain muốn cài SSL và nhấn enter thì sẽ tiến hành cải chứng chỉ. Do chạy trên môi trường máy ảo để test nên có xảy ra lỗi không tìm được domain như bến dưới.

![image.png](/Images/tuan_4_thxdwvps/image%2022.png)

**Thao tác cài chứng chỉ trả phí (EV, OV, …)**

**Bước 1:** Vào giao diện quản lý chính của **LarVPS** bằng quyền **root** và nhập số **2 + enter** để vào giao diện **Quản lý SSL**:

```bash
larvps
```

![image.png](/Images/tuan_4_thxdwvps/image%2019.png)

**Bước 2:** Nhập tiếp số 9 + enter để vô giao diện cài chứng chỉ trả phí

![image.png](/Images/tuan_4_thxdwvps/image%2023.png)

Bước 3: Tiếp đó LarVPS sẽ yêu cầu bạn tạo 2 File có định dạng bên dưới và lưu nó tại dường dẫn: ***/home/domaincom/domain.com/ssl*** (Nhớ thay đường dẫn đúng với đường dẫn của website bạn )

![image.png](/Images/tuan_4_thxdwvps/image%2024.png)

**Bước 4:** Tạo file **domain.com.crt**

Bây giờ chúng ta sẽ di chuyển vào đúng thư mục SSL của website cần cài đặt với đường dẫn: ***/home/domaincom/domain.com/ssl*** và tạo 2 File chứng chỉ theo yêu cầu.

Bạn sử dụng lệnh **nano** để tạo file và sau khi nhập xong thì lưu file và thoát:

```bash
nano domain.com.crt
```

![image.png](/Images/tuan_4_thxdwvps/image%2025.png)

**Bước 5:** *Tạo File **domain.com.key***

Bạn sử dụng lệnh **nano** để tạo file (nhớ phải nằm trong thư mục ***/home/domaincom/domain.com/ssl*** ) và cũng lưu trước khi thoát:

```bash
nano domain.com.key
```

![image.png](/Images/tuan_4_thxdwvps/image%2026.png)

**Bước 6:** Kiểm tra file và khởi động lại **Nginx SSL**

Bạn có thể kiêm tra lại 2 file đã lưu trong  ***/home/domaincom/domain.com/ssl*** bằng lệnh sau:

```bash
ll
```

![image.png](/Images/tuan_4_thxdwvps/image%2027.png)

Như yêu cầu của **LarVPS**, sau khi thêm chứng chỉ xong, chúng ta cần ***Restore Nginx SSL*** để áp dụng cấu hình **SSL**.

Tại Menu **LarVPS**, bạn nhấn số ***7*** tương ứng ***Quản lý Nginx →*** Khi vào được rồi thì nhấn sô **2 + enter** để phục hồi **Nginx SSL**

![image.png](/Images/tuan_4_thxdwvps/image%2028.png)

Sau khi phục hồi xong thì SSL sẽ đc cài đặt. Do ở ví dụ nàyt tôi chỉ thực hành sử dụng nên chưa có chứng chỉ SSL thật.

![image.png](/Images/tuan_4_thxdwvps/image%2029.png)

**3. Cài đặt Wordpress**

Ở giao diện quản lý chỉnh của LarVPS → nhấn số 9 + enter để vào **Quản lý Application**

![image.png](/Images/tuan_4_thxdwvps/image%2030.png)

Nhấn số 1 + enter để cài đặt Wordpress

![image.png](/Images/tuan_4_thxdwvps/image%2031.png)

Sau đó chọn domain để cài đặt và xác nhận lại thông tin

![image.png](/Images/tuan_4_thxdwvps/image%2032.png)

Cuối cùng sẽ hiện cài đặt thành công Wordpress

# Centmin Mod

## Giới thiệu về Centmin Mod

**Centmin Mod** là một LEMP (Linux, Nginx, MariaDB MySQL and PHP-FPM) web stack cho CentOS được viết bởi George Liu (eva2000) kết hợp thêm với shell menu dùng để quản lý VPS rất tiện dụng, dễ dàng sử dụng.

Centmin Mod không hoạt động cùng với WHM/Cpanel, Plesk hoặc DirectAdmin nên bạn cần chuẩn bị một VPS mới tinh để cài đặt.

Tốt nhất bạn nên chuẩn bị VPS với ít nhất 512MB RAM cho CentOS 6.x, 2GB cho CentOS 7.x.

Tất cả các thao tác bên dưới cần thực hiện với quyền root khi login bằng SSH.

## Cài đặt Centmin Mod

Đầu tiên ta cần cập nhật lại các gói và tải về phiên bản mới nhất của **Centmin Mod**

```bash
yum -y update
curl -O https://centminmod.com/installer.sh && chmod 0700 installer.sh && bash installer.sh
```

Sau đó, script sẽ tự động cài đặt, bạn không phải nhập bất cứ thông tin gì cả. Thời gian cài đặt có thể kéo dài 30 phút tùy cấu hình VPS

Cuối cùng, nếu không gặp vấn đề gì bạn sẽ nhận được thông tin Memcached Server Admin Login và MySQL root password. Nhớ lưu lại thông tin này để sử dụng sau này.

Để sử dụng giao diện, hãy chạy lệnh:

```bash
centmin
```

![image.png](/Images/tuan_4_thxdwvps/image%2033.png)

## Thao tác với Centmin mod

**1. Cấu hình thêm Domain mới:**

Bạn nhập lệnh `centmin` để mở bảng menu lên nhé. Sau đó bạn chọn `2). Add Nginx vhost domain` để thêm vào.

![image.png](/Images/tuan_4_thxdwvps/image%2034.png)

Sau đó là nhập những thông tin cần thiết và nhấn y để tiếp tục

![image.png](/Images/tuan_4_thxdwvps/image%2035.png)

Tiếp tục điền những thông tin cần thiết, chọn mức bảo mật, tạo FTP account 

![image.png](/Images/tuan_4_thxdwvps/image%2036.png)

Cuối cùng là thông báo thành công

![image.png](/Images/tuan_4_thxdwvps/image%2037.png)

**2. Cài chứng chỉ SSL**

**Đối với chứng chỉ miễn phí (Let’s Encrypt)**

Centminmod

Với bản Centmin Mod `123.09beta01` có bổ sung thêm tính năng hỗ trợ cài đặt SSL Let’s Encrypt khi bạn thêm website vào Centmin Mod. Nhưng với điều kiện tên miền của bạn phải được trỏ về IP máy chủ Centmin Mod thì việc cấp phát chứng chỉ SSL mới thành công. Sau khi tên miền đã được trỏ thành công bạn hãy bật tính năng tự động cài SSL lên, bạn chỉ thực hiện một lần duy nhất thôi thì các thao tác thêm lần sau sẽ sử dụng mãi mãi.

Bạn sử dụng lệnh `vi` `vim` hoặc `nano` để mở file `vi etc/centminmod/custom_config.inc` lên vào thêm vào đoạn sau.

```bash
LETSENCRYPT_DETECT='y'
```

Tiếp đến bạn chạy tiếp 2 lệnh này để cài đặt tính năng vừa thêm.

```
cd /usr/local/src/centminmod/addons
./acmetool.sh acmeinstall
```

![image.png](/Images/tuan_4_thxdwvps/image%2038.png)

**Đối với chứng chỉ SSL trả phí (OV, EV,…)**

Hiện tại thì Centmin mod chưa hỗ trợ cài SSL trả phí qua giao diện, nên bạn có thể cài từng bước thủ công

**3. Cài đặt Wordpress**

**Bước 1. Tạo database**

Giống như thường lệ, ta cần phải có một database trước khi [cài WordPress](https://thachpham.com/wordpress/wordpress-tutorials/cai-dat-website-wordpress-len-host.html) nhé.

Để tạo một database trong Centminmod, bạn chạy lệnh sau:

```
/usr/local/src/centminmod/addons/mysqladmin_shell.sh createuserdb dbname dbuser dbpass
```

Trong đó, bạn thay:

- dbname: Tên database cần tạo.
- dbuser: Tên người dùng database cần tạo.
- dbpass: Mật khẩu kết nối với người dùng database, nên đặt phức tạp lên nhé. Và tốt nhất là không có ký tự đặc biệt.

Sau khi chạy xong thì bạn đã tạo một database hoàn tất.

**Bước 2. Cài đặt WordPress vào máy chủ**

Bây giờ bạn hãy dùng lệnh cd để truy cập vào thư mục public của domain bạn đã thêm vào VPS. Ví dụ:

```bash
cd /home/nginx/domains/thiencentmod.com/public
```

Sau đó chúng ta tải source của WordPress về bằng lệnh wget:

```bash
wget https://wordpress.org/latest.tar.gz
```

Và giải nén file latest.tar.gz ra:

```bash
tar -xvf latest.tar.gz
```

Sau khi giải nén xong, source của WordPress sẽ nằm trong một thư mục tên là wordpress. Bây giờ chúng ta nên đưa toàn bộ file và thư mục trong thư mục wordpress ra ngoài thư mục public.

```bash
mv wordpress/* .
```

Nhớ đừng bỏ xót dấu chấm (.) nhé. Bây giờ bạn có thể xóa thư mục wordpress kia đi cho đỡ nhức mắt.

**Bước 3. CHOWN thư mục WordPress**

Mỗi lần cài mới WordPress, bạn cần phải cấp quyền cho user nginx và group nginx sở hữu thư mục của các domain để nó có thể tự tạo folder khi cài theme/plugin và có thể upload ảnh. Bạn chạy lệnh dưới đây vào:

```bash
chown -R nginx:nginx /home/nginx/domains/
```

Bây giờ bạn có thể truy cập vào domain và cài đặt WordPress như bình thường rồi đó.

**Bước 4. Thiết lập permalink NGINX cho WordPress**

NGINX không sử dụng file .htaccess nên có thể sẽ bị lỗi nếu bạn sử dụng chức năng permalink mà chưa cấu hình lại NGINX. Để sử dụng được permalink, bạn hãy mở file cấu hình domain của bạn trong `/usr/local/nginx/conf/conf.d/domain.ltd.ssl.conf` ra và tìm đoạn bên dưới rồi bỏ dấu `#` trước nó đi:

```
#try_files $uri $uri/ /index.php?q=$uri&$args;
```

Bây giờ hãy lưu lại và khởi động lại NGINX.

```bash
service nginx restart
```

**Fix lỗi 404 nếu sử dụng permalink .html**

Nếu bạn cố tình sử dụng permalink có cấu trúc là đuôi .html như blog Thachpham.com thì bạn hãy mở file /usr/local/nginx/conf/staticfiles.conf và tìm đoạn dưới đây rồi xóa đi:

location ~* \.(html|html|txt)$ {

……

}

# MERN/MEAN Stack

**MEAN** cũng tương tự như **MERN** nhưng thay vì sử dụng **React** thì lại sử dụng **Angular .**Ở bài thực hành này mình sẽ thực hành **MERN,** cụ thể hơn là mình sẽ chọn 1 source code sử dụng MongoDB, Node.js, ReactJS để thực hành. Đây là link của đồ án này [chat_app](https://github.com/ThienDuong2909/Chat_RealTime_Web)

**Đầu tiên ta sẽ cài đặt MongoDB**

```bash
wget -qO - [https://www.mongodb.org/static/pgp/server-4.4.asc](https://www.mongodb.org/static/pgp/server-4.4.asc) | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-4.4.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/mongodb-4.4.gpg] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

sudo apt-get update
sudo apt-get install -y mongodb-org=4.4.29 mongodb-org-server=4.4.29 mongodb-org-shell=4.4.29 mongodb-org-mongos=4.4.29 mongodb-org-tools=4.4.29
```

Khởi động lại và xem trạng thái dịch vụ bằng những lệnh:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```

![image.png](/Images/tuan_4_thxdwvps/image%2039.png)

**Tiếp theo, cài Node.js và công cụ npm, pm2:**

```bash
sudo apt update
sudo apt install -y nodejs npm nginx
sudo npm install -g pm2
```

**Clone dự án cần thực hành về:**

Về thư mục /home/user và clone repo về:

```bash
cd ~
git clone https://github.com/ThienDuong2909/Chat_RealTime_Web
```

![image.png](/Images/tuan_4_thxdwvps/image%2040.png)

Nếu như dự án chia 2 phần frontend và backend như trong ví dụ thì bạn cần tạo 2 file .env tương ứng ở 2 phía và thêm dữ liệu vào 2 file .env đó. Có thể tạo fiel bằng lệnh sau:

```bash
touch .env
```

![image.png](/Images/tuan_4_thxdwvps/image%2041.png)

![image.png](/Images/tuan_4_thxdwvps/image%2042.png)

Tải những thư viện cần thiết đã khai báo ở 2 file package.json tương ứng 2 folder

```bash
cd /backend
npm install
```

```bash
cd /frontend
npm install
```

![image.png](/Images/tuan_4_thxdwvps/image%2043.png)

![image.png](/Images/tuan_4_thxdwvps/image%2044.png)

**Cấu hình NGINX Proxy để chuyển hướng 80/443**

Tạo file cấu hình của NGINX cho site mới bằng lệnh: 

```bash
sudo nano /etc/nginx/sites-available/your_app
```

Và chỉnh sủa file bằng lệnh **nano:**

```bash
server {
        listen 80;
        server_name [thienmern.com](http://thienmern.com/);
        return 301 https://$host$request_uri;
}
server {
        listen 443 ssl;
        server_name [thienmern.com](http://thienmern.com/);
        ssl_certificate /etc/ssl/certs/thienmern-selfsigned.crt;
        ssl_certificate_key /etc/ssl/private/thienmern-selfsigned.key;
        
        #đây là backend
        location /api/ {
            proxy_pass <http://localhost:3030/>;
            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
        }
        
        #đây là frontend
        location / {
            proxy_pass <http://localhost:5173/>;
            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_cache_bypass $http_upgrade;
        }
};
```

**Chạy đồ án với pm2**

```bash
cd /backend   
pm2 start npm --name chat-backend -- run dev
```

```bash
cd /frontend
pm2 start npm --name chat-frontend -- run dev
```

Có thể kiểm tra tiến các tiến trình pm2 đang chạy:

```bash
pm2 list
```

![image.png](/Images/tuan_4_thxdwvps/image%2045.png)

**Kiểm tra trên trình duyệt** 

Truy cập vào `http://your_domain` và [`https://your_domain`](https://your_domain) xem có hiện giao diện hay không. Nếu truy cập `http`thì xem có tự chuyển hướng sang `https`hay không.

![image.png](/Images/tuan_4_thxdwvps/image%2046.png)

---
THE END