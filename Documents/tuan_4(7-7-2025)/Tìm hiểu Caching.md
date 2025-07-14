# Tìm hiểu về Caching
# Mục lục
  - [Tìm hiểu về Caching](#tìm-hiểu-về-caching)
  - [Giới thiệu](#giới-thiệu)
    - [Varnish Cache](#varnish-cache)
    - [Memcached](#memcached)
    - [Redis](#redis)
  - [Cài đặt và cấu hình](#cài-đặt-và-cấu-hình)
    - [Cài đặt và cấu hình Redis](#1-cài-đặt-và-cấu-hình-redis)
    - [Cài đặt và cấu hình Memcached](#2-cài-đặt-và-cấu-hình-memcached)
    - [Cài đặt và cấu hình Varnish](#3-cài-đặt-và-cấu-hình-varnish)

# Giới thiệu

**Caching** là một kỹ thuật tăng độ truy xuất dữ liệu và giảm tải cho hệ thống. Cache là nơi lưu tập hợp các dữ liệu, thường có tính chất nhất thời, cho phép sử dụng lại dữ liệu đã lấy hoặc tính toán trước đó, nên sẽ giúp tăng tốc cho việc truy xuất dữ liệu ở những lần sau.

### **Varnish Cache**

là một công cụ reverse proxy caching mạnh mẽ, chuyên dụng để tối ưu hóa các trang web và dịch vụ web. Varnish được thiết kế đặc biệt để xử lý các yêu cầu HTTP/HTTPS, giúp giảm tải cho các máy chủ ứng dụng và nâng cao tốc độ truy cập của người dùng

**Đặc điểm nổi bật của Varnish:**

- **Tối ưu hóa cho HTTP**: Varnish không phải là một công cụ cache chung mà được thiết kế đặc biệt để làm việc với giao thức HTTP, giúp tăng tốc các trang web và dịch vụ API bằng cách lưu trữ và phục vụ lại các phản hồi HTTP.
- **Cấu hình linh hoạt với VCL**: Varnish sử dụng ngôn ngữ cấu hình riêng biệt gọi là **VCL (Varnish Configuration Language)**, cho phép các nhà phát triển tùy chỉnh và tối ưu hóa cách thức cache theo từng yêu cầu cụ thể.
- **Tốc độ cao**: Varnish có thể xử lý hàng triệu yêu cầu mỗi giây nhờ vào việc lưu trữ cache trong bộ nhớ thay vì sử dụng đĩa cứng.

### **Memcached**

**Memcached** là một hệ thống cache key-value đơn giản và hiệu quả, thường được sử dụng để giảm tải cho các cơ sở dữ liệu và tăng tốc các ứng dụng web bằng cách lưu trữ dữ liệu trong bộ nhớ RAM. Memcached là một lựa chọn phổ biến cho những hệ thống yêu cầu caching tốc độ cao với tính đơn giản trong triển khai.

**Đặc điểm nổi bật của Memcached:**

- **Cache Key-Value**: Memcached chỉ hỗ trợ lưu trữ dữ liệu theo kiểu key-value (khóa-giá trị), điều này giúp nó trở thành một công cụ cực kỳ đơn giản và dễ sử dụng.
- **Không có persistence**: Dữ liệu trong Memcached được lưu trữ trong bộ nhớ RAM và sẽ bị mất khi dịch vụ bị dừng hoặc máy chủ gặp sự cố. Điều này làm cho Memcached phù hợp hơn với các ứng dụng cần lưu trữ tạm thời và không yêu cầu bảo vệ dữ liệu lâu dài.
- **Phân tán và khả năng mở rộng**: Memcached hỗ trợ cache phân tán, cho phép mở rộng dễ dàng khi cần thiết, với khả năng chia sẻ cache giữa nhiều máy chủ.

### Redis

**Redis** là một hệ thống lưu trữ dữ liệu trong bộ nhớ với khả năng mạnh mẽ hơn Memcached, không chỉ hỗ trợ cache key-value mà còn hỗ trợ nhiều cấu trúc dữ liệu phức tạp. Redis được coi là một cơ sở dữ liệu trong bộ nhớ với khả năng lưu trữ và xử lý dữ liệu phức tạp, phù hợp cho nhiều loại ứng dụng khác nhau.

**Đặc điểm nổi bật của Redis:**

- **Hỗ trợ nhiều cấu trúc dữ liệu**: Redis không chỉ hỗ trợ kiểu dữ liệu key-value đơn giản mà còn hỗ trợ các kiểu dữ liệu phức tạp như danh sách, tập hợp, hash, sets, hyperloglog và các dữ liệu không đồng bộ khác. Điều này giúp Redis linh hoạt hơn trong việc giải quyết các bài toán phức tạp.
- **Persistency**: Redis có khả năng ghi dữ liệu xuống đĩa (với các chế độ RDB hoặc AOF), giúp bảo vệ dữ liệu khỏi mất mát trong trường hợp máy chủ gặp sự cố. Tuy nhiên, Redis vẫn hoạt động chủ yếu trong bộ nhớ, mang lại hiệu suất cực kỳ cao.
- **Tính năng phân tán và sao chép**: Redis hỗ trợ replication, sharding, và clustering, giúp phân phối tải và sao lưu dữ liệu. Nó cũng có khả năng xử lý các yêu cầu phức tạp như Pub/Sub và message queuing.

# Cài đặt và cấu hình

### **1. Cài đặt và cấu hình Redis.**

### **Cài đặt Redis**

**Bước 1: Cập nhật gói hệ thống**

```bash
sudo apt update
```

**Bước 2: Thêm kho lưu trữ PPA**

Trong bước tiếp theo, chúng tôi sẽ **thêm** “**redislabs** ” **kho lưu trữ PPA** vào hệ thống Ubuntu 22.04 của chúng tôi:

```bash
sudo add-apt-repository ppa:redislabs/redis
```

![image.png](/Images/tuan_4_caching/image.png)

**Bước 3: Cài đặt Redis**

Sau khi thêm hô hấp cần thiết, hãy thực hiện lệnh sau để cài đặt Redis:

```bash
sudo apt install redis
```

![image.png](/Images/tuan_4_caching/image%201.png)

**Bước 4: Kiểm tra phiên bản Redis**

```bash
redis-server -v
```

![image.png](/Images/tuan_4_caching/image%202.png)

### Cấu hình Redis

**Bước 1: Kích hoạt dịch vụ Redis**

Để định cấu hình Redis trên Ubuntu 22.04, trước tiên hãy kích hoạt dịch vụ Redis bằng cách thực hiện lệnh sau:

```bash
sudo systemctl enable --now redis-server
```

**Bước 2: Mở tệp cấu hình Redis**

Ở bước tiếp theo, hãy mở tệp Cấu hình Redis trong trình chỉnh sửa “**nano** ” để thực hiện một số thay đổi bắt buộc:

```bash
sudo nano /etc/redis/redis.conf
```

Tệp “**redis.conf** ” đã mở sẽ trông như thế này:

![image.png](/Images/tuan_4_caching/image%203.png)

Tìm dòng ghi địa chỉ “**bind** ” là “**127.0.0.1** ”:

Thay thế nó bằng “**bind 0.0.0.0** ”

![image.png](/Images/tuan_4_caching/image%204.png)

Và tìm dòng requirepass, bỏ dấu **`#`** và nhập passwd ở phía sau. Sau này khi cần xác minh thì chỉ cần nhập passwd.

![image.png](/Images/tuan_4_caching/image%205.png)

**Bước 3: Khởi động lại và xem trạng thái.**

![image.png](/Images/tuan_4_caching/image%206.png)

**Bước 4: Xác minh IP và cổng Redis**

Sử dụng lệnh “**ss** ” sau để kiểm tra IP và số cổng được Redis sử dụng:

```bash
ss -tunelp | grep 6379
```

![image.png](/Images/tuan_4_caching/image%207.png)

Ngoài ra, nếu bạn có bật firewall thì phải cho phép cổng “**6379** ” cho các kết nối “**tcp** ”:

```bash
sudo ufw allow 6379/tcp
```

![image.png](/Images/tuan_4_caching/image%208.png)

**Bước 5: Kiểm tra máy chủ Redis**

Bây giờ, đã đến lúc kiểm tra máy chủ Redis và kết nối cục bộ với nó:

```bash
redis-cli
```

Trước hết, hãy thực thi lệnh “**AUTH** ” và chỉ định mật khẩu bạn đã nhập trong tệp cấu hình Redis:

```bash
AUTH passwd
```

Nhập đúng mật khẩu sẽ thiết lập kết nối thành công với Redis và xuất ra “**OK** ”:

![image.png](/Images/tuan_4_caching/image%209.png)

### **Bước 6: Kiểm tra thông tin Redis**

Để kiểm tra thông tin Redis, hãy chạy lệnh “**INFO** ”:

```bash
> INFO
```

![image.png](/Images/tuan_4_caching/image%2010.png)

**Bước 7: Dịch vụ Ping Redis**

Tiếp theo, “**ping** ” dịch vụ Redis:

```bash
> ping
```

![image.png](/Images/tuan_4_caching/image%2011.png)

**Bước 8: Thoát Redis CLI**

Nhập lệnh “**quit** ” để thoát shell Redis CLI hiện tại:

```bash
> quit
```

### **2. Cài đặt và cấu hình Memcached**

### **Cài đặt Memcached**

**Bước 1: Cài đặt các gói cần thiết**

```bash
apt install memcached libmemcached-tools -y
```

Sau khi cài đặt Memcached, bạn có thể xác minh phiên bản Memcached bằng lệnh sau:

```sql
memcached --version
```

Bạn sẽ thấy đầu ra sau:

![image.png](/Images/tuan_4_caching/image%2012.png)

**Bước 2: Khởi chạy và kiểm tra trạng thái dịch vụ**

Để khởi động dịch vụ Memcached, hãy chạy lệnh sau:

```
systemctl start memcached
```

Để kích hoạt dịch vụ Memcached để khởi động nó sau khi khởi động lại hệ thống, hãy chạy lệnh sau:

```
systemctl enable memcached
```

Bạn cũng có thể kiểm tra trạng thái của dịch vụ Memcached bằng lệnh sau:

Kết quả sẽ là:

```
systemctl status memcached
```

![image.png](/Images/tuan_4_caching/image%2013.png)

Theo mặc định, Memcached lắng nghe trên cổng 11211. Bạn có thể kiểm tra nó bằng lệnh sau:

```
ss -antpl | grep memcache
```

Bạn sẽ thấy cổng nghe Memcached ở đầu ra sau:

![image.png](/Images/tuan_4_caching/image%2014.png)

### **Cấu hình Memcached**

Tệp cấu hình mặc định của Memcached được đặt tại **/etc/memcached.conf**. Bạn có thể chỉnh sửa nó để thay đổi cài đặt mặc định theo yêu cầu của bạn.

```
nano /etc/memcached.conf
```

Thay đổi các dòng sau theo yêu cầu của bạn:

```powershell
## Specify the IP address on which Memcached listens on.
-l 127.0.0.1

## Disable the UDP

-U 0

## Define the memory to store the cache.

-m 1000

```

Lưu và đóng file sau đó khởi động lại dịch vụ Memcached để áp dụng các thay đổi cấu hình:

```
systemctl restart memcached
```

### 3. Cài đặt và cấu hình Varnish

### **Cài đặt Varnish**

**Bước 1: Đăng ký repo Varnish Cache 7**

Cài đặt các gói cần thiết

![image.png](/Images/tuan_4_caching/image%2015.png)

Bây giờ hãy nhập GPG key.

```powershell
curl -fsSL https://packagecloud.io/varnishcache/varnish70/gpgkey|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/varnish.gpg
```

Thêm Varnish repository trên Ubuntu 22.04

```bash
sudo tee /etc/apt/sources.list.d/varnishcache_varnish70.list > /dev/null <<-EOF
deb https://packagecloud.io/varnishcache/varnish70/ubuntu/ focal main
deb-src https://packagecloud.io/varnishcache/varnish70/ubuntu/ focal main
EOF
```

Sau đó cập nhật lại các gói

```sql
sudo apt update
```

 ****

**Bước 2: Cài đặt Varnish**

Chạy lệnh dưới để cài đặt Varnish:

```bash
sudo apt install varnish
```

![image.png](/Images/tuan_4_caching/image%2016.png)

**Bước 3: Định cấu hình cổng nghe và kích thước bộ đệm**

Cổng nghe mặc định của Varnish được đặt thành **6081**, vì vậy chúng ta cần thay đổi cổng này thành cổng 80 và kích thước bộ đệm thành 2GB.

Trong `/etc/systemd/system/varnish.service`, hãy chỉnh sửa phần bên dưới và thêm các chi tiết mong muốn.

```powershell
$ sudo vim /etc/systemd/system/varnish.service
ExecStart=/usr/sbin/varnishd \
      -a :80 \
      -a localhost:8443,PROXY \
      -p feature=+http2 \
      -f /etc/varnish/default.vcl \
      -s malloc,1g
```

![image.png](/Images/tuan_4_caching/image%2017.png)

Tải lại daemon hệ thống.

```
sudo systemctl daemon-reload
```

Bắt đầu và kích hoạt bộ đệm Varnish.

```
sudo systemctl start varnish
```

**Bước 4: Cấu hình máy chủ web để sử dụng Varnish**

Đảm bảo máy chủ web Nginx đã được cài đặt.

```
sudo apt install nginx
```

Bây giờ hãy chỉnh sửa Máy chủ ảo và thay thế cổng 80 thành 8080 bằng lệnh bên dưới:

```powershell
sudo find /etc/nginx/sites-enabled -name '*.conf' -exec sed -r -i 's/\blisten ([^:]+:)?80\b([^;]*);/listen \18080\2;/g' {} ';'
```

Đoạn script trên sẽ chỉnh sửa bất kỳ cấu hình nào trong đường dẫn ***/etc/nginx/sites-enabled*** . Bạn cũng có thể cần thay đổi trang Nginx mặc định để nghe trên cổng 8080.

```powershell
$ sudo vim /etc/nginx/sites-enabled/default
......
server {
        listen 8080 default_server;
        #listen [::]:80 default_server;

        # SSL configuration
        #
```

Khởi động lại dịch vụ nginx sau khi thay đổi:

```
sudo systemctl restart nginx
```

Apache cũng tương tự như trên 

**Bước 5: Điều chỉnh file VCL**

Những thay đổi được thực hiện ở trên cần được phản ánh trong phần phụ trợ VLC. Theo mặc định, phần phụ trợ VLC được định cấu hình để trỏ đến cổng đã đặt 8080. Tệp này được tìm thấy trong ***/etc/varnish/default.vcl***.

```powershell
sudo nano /etc/varnish/default.vcl
```

![image.png](/Images/tuan_4_caching/image%2018.png)

Bây giờ khởi động lại dịch vụ.

```powershell
sudo systemctl restart nginx varnish
sudo systemctl restart apache2 varnish
```

**Bước 6: Kiểm tra lại đã cài đặt và sử dụng thành công chưa**

Để xác minh rằng máy chủ đang hoạt động bình thường, chúng tôi sẽ sử dụng lệnh cURL bên dưới.

```powershell
curl -I server-ip
```

![image.png](/Images/tuan_4_caching/image%2019.png)

---
THE END