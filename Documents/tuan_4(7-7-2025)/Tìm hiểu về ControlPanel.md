# Sử dụng control panel

## Mục lục
  - [cPanel](#cpanel)
    - [Giới thiệu về cPanel](#giới-thiệu-về-cpanel)
    - [Cài đặt cPanel](#cài-đặt-cpanel)
    - [Thao Tác với cPanel](#thao-tác-với-cpanel)
  - [CyberPanel](#cyberpanel)
    - [Giới thiệu về CyberPanel](#giới-thiệu-về-cyberpanel)
    - [Ưu điểm](#ưu-điểm)
    - [Nhược điểm](#nhược-điểm)
    - [Cài đặt CyberPanel](#cài-đặt-cyberpanel)
    - [Thao tác với CyberPanel](#thao-tác-với-cyberpanel)
## cPanel

### Giới thiệu về cPanel

**cPanel** là hệ thống quản trị web hosting trên nền tảng Linux phổ biến và mạnh mẽ nhất hiện nay. cPanel cung cấp giao diện đồ họa đơn giản, linh hoạt. Kèm theo rất nhiều tính năng giúp các bạn quản trị hosting và website của mình một cách dễ dàng. Vậy lý do khiến cPanel trở thành hệ thống hosting website rộng rãi nhất là gì?

### Cài đặt cPanel

**Các bước cài đặt cPanel**

Trước hết, đăng nhập với quyền `root` vào máy chủ của bạn.

Nếu bạn có một tường lửa hoạt động, bạn nên tắt và dừng nó trước khi cài đặt cPanel bằng các lệnh sau:

```
iptables-save > ~/firewall.rules
systemctl stop firewalld.service
systemctl disable firewalld.service
```

Tiếp theo, cập nhật các gói hệ thống bằng lệnh dưới đây:

```
sudo apt update
sudo apt upgrade
```

Tiếp theo, bạn cần thiết lập tên miền hoàn chỉnh cho máy chủ. Bởi vì cPanel cần tên miền FQDN hoàn chỉnh để cài đặt. Để đặt hostname cho tên miền, nhập lệnh dưới đây:

```
nano /etc/hostname
```

![image.png](/Images/tuan_4_controlpanel/image.png)

Sau khi tệp hostname mở, đặt tên miền mới. Sau đó nhấn **Ctrl+O** để lưu và **Ctrl+X** để đóng tệp.

Ở bước này, mở tệp bằng câu lệnh sau:

```
nano /etc/hosts
```

Bạn nên thay đổi địa chỉ IP mới bằng tên miền:

```
IP_address yourserver.domain.com yourserver
```

Lưu và đóng tệp như đã đề cập.

Bây giờ sẽ install CPanel và WHM trên Ubuntu 22.04. Để làm điều này, chạy lệnh sau:

```
cd /home && curl -o latest -L https://securedownloads.cpanel.net/latest && sh latest
```

Chờ khoảng 20 đến 40 phút (tùy vào tốc độ mạng của bạn) cho đến khi quá trình cài đặt hoàn thành

Sau khi cài đạt hoàn tất, bạn mở trình duyệt bất kì và tìm kiếm `https://your_domain_or_IP:2087` thì sẽ hiện giao diên đăng nhập của WHM (Web Hosting Manager)

![image.png](/Images/tuan_4_controlpanel/image%201.png)

Ở đăng đăng nhập của WHM, nhập `username`và `password`sau đó nhấn `Log in` , thường thì là tài khoản `root` của máy chủ.

Đăng nhập thành công sẽ tiến vào giao diện chính của WHM :

![image.png](/Images/tuan_4_controlpanel/image%202.png)

**Để sử dụng cPanel thì ta cần tạo tài khoản đăng nhập nhanh với các bước dưới đây**

Tìm mục **Account Functions** → **Create a New Account →** điền các thông tin ở phần **Domain Information, Các phần kia có thể cấu hình sau**

![image.png](/Images/tuan_4_controlpanel/image%203.png)

Sau khi nhập xong thông tin, kéo xuống dưới nhấn **Create**

![image.png](/Images/tuan_4_controlpanel/image%204.png)

Chờ 1 chút để tạo tài khoản

![image.png](/Images/tuan_4_controlpanel/image%205.png)

Sau khi tạo thành công thì ta sẽ nhân được log sau đây

![image.png](/Images/tuan_4_controlpanel/image%206.png)

Có thể kiểm tra tài khoản vừa tạo ở mục **Account Information → List Account**

![image.png](/Images/tuan_4_controlpanel/image%207.png)

Đã tạo tài khoản thành công, ta mở tab trình duyệt khác tìm kiếm [`https://domain_vua_tao:2083`](https://domain_vua_tao:2083) sẽ hiện giao diện đăng nhập **cPanel** → nhập **username**, **password** vừa tạo → nhấn **Login**

![image.png](/Images/tuan_4_controlpanel/image%208.png)

Ta sẽ vào được trang chủ của cPanel

![image.png](/Images/tuan_4_controlpanel/image%209.png)

### Thao Tác với cPanel

**1. Thao tác với domain và sub - domain**

Ở phần **Tools** → tìm mục **Domains →** nhấn vào ô **Domain**

![image.png](/Images/tuan_4_controlpanel/image%2010.png)

Hiện ra giao diện quản lý **Domain,** nếu là cPanel mới thì sẽ thấy mỗi domain chính

![image.png](/Images/tuan_4_controlpanel/image%2011.png)

Ta có thể thêm **sub - domain** bằng cách nhấp nhấn vào nút **Create A New Domain**

![image.png](/Images/tuan_4_controlpanel/image%2012.png)

Điền lần lượt tên **sub - domain** → chọn **Document Root →** nhấn **Submit**

![image.png](/Images/tuan_4_controlpanel/image%2013.png)

Sau khi tạo thành công ta có thể thấy **sub - domain** vừa tạo ở trang **Domains**

![image.png](/Images/tuan_4_controlpanel/image%2014.png)

**2. Thao tác tạo thêm FTP Account**

Ở phần **Tools** → tìm mục **Files** → nhấn vào ô **FTP Accounts**

![image.png](/Images/tuan_4_controlpanel/image%2015.png)

Ta thấy đầu tiên la phần tạo tài khoản FTP, hãy điền đầy đủ thông tin để tạo

![image.png](/Images/tuan_4_controlpanel/image%2016.png)

Sau khi tạo thành công, lăn chuột xuống ta sẽ thầy danh sách các tài khoản đã tạo

![image.png](/Images/tuan_4_controlpanel/image%2017.png)

Ta có thể kiêm tra bằng FireZilla xem có hoạt động hay không

![image.png](/Images/tuan_4_controlpanel/image%2018.png)

![image.png](/Images/tuan_4_controlpanel/image%2019.png)

**3. Thao tác cài đặt chứng chỉ SSL**

**Đối với cài đặt chứng chỉ SSL miễn phí (Let’s Encrypt)**

Ở phần **Tools** → tìm mục **Security** → chọn ô **SSL/TLS Status**

![image.png](/Images/tuan_4_controlpanel/image%2020.png)

Chọn những **Domain** bạn muốn cấp chứng chỉ → nhấn   **Run AutoSSL**

![image.png](/Images/tuan_4_controlpanel/image%2021.png)

Và đợi cho những hình ổ khóa chuyển sang màu xanh là thành công

**Đối với cài đặt chứng chỉ SSL trả phí (OV, DV, …)**

Cần chuẩn bị  đầy đủ các file chứng chỉ sau:

- **Certificate.crt:** File chứng chỉ chính của tên miền.
- **Private.key:** Khóa riêng tư tương ứng với chứng chỉ. Khóa này rất quan trọng và cần được bảo mật tuyệt đối, không chia sẻ file này với bất kỳ ai.
- **CA.crt (hoặc CA Bundle):** File chứng chỉ trung gian giúp xác thực SSL.

Ở phần **Tools** → tìm mục **Security** → chọn ô **SSL/TLS** 

![image.png](/Images/tuan_4_controlpanel/image%2022.png)

Tiếp theo nhấn vào **Manage SSL sites**

![image.png](/Images/tuan_4_controlpanel/image%2023.png)

Chọn Domain cần cài chứng chỉ :

![image.png](/Images/tuan_4_controlpanel/image%2024.png)

Ở dưới bạn sẽ thấy các trường để nhập thông tin chứng chỉ.

- **Certificate (CRT):** Dán nội dung từ file Certificate.crt.
- **Private Key (KEY):** Dán nội dung từ file Private.key.
- **Certificate Authority Bundle (CABUNDLE):** Dán nội dung từ file CA.crt hoặc CA Bundle.

![image.png](/Images/tuan_4_controlpanel/image%2025.png)

Sau khi nhập đầy đủ thông tin, bạn nhấn **Install Certificate** để hoàn tất quá trình cài đặt cài chứng chỉ SSL cho website.

![image.png](/Images/tuan_4_controlpanel/image%2026.png)

Sau khi cài đặt xong có thể kiểm tra thông qua trang [SSL Shopper](https://www.sslshopper.com/ssl-checker.html)

**4. Sử dụng công cụ để Backup & Restore dữ liệu**

**Backup**

Ở phần **Tools** → tìm mục **Files** → nhấp vào ô **Backup Wizard**

![image.png](/Images/tuan_4_controlpanel/image%2027.png)

Nhấn vào nút **Back Up**

![image.png](/Images/tuan_4_controlpanel/image%2028.png)

Tiếp theo, bạn sẽ được lựa chọn loại backup phù hợp với nhu cầu:

- **Full Backup:** Tùy chọn này sẽ thực hiện backup toàn bộ dữ liệu, bao gồm mã nguồn ([source code](https://vietnix.vn/source-code-la-gi/)), cơ sở dữ liệu ([database](https://vietnix.vn/database-la-gi/)) và [email](https://vietnix.vn/email-la-gi/). Đây là lựa chọn an toàn nhất để đảm bảo backup toàn diện.
- **Home Directory:** Tùy chọn này chỉ backup source code website.
- **MySQL Databases:** Chỉ thực hiện backup database.
- **Email Forwarders & Filters:** Chỉ thực hiện backup các email chuyển tiếp và bộ lọc email.

![image.png](/Images/tuan_4_controlpanel/image%2029.png)

Bạn có thể tick chọn vào ô email để nhận thông báo sau khi quá trình backup hoàn tất. Nếu không muốn nhận thông báo, bạn có thể tick chọn vào **Do not send email notification of backup completion.** Sau đó nhấn **Generate Backup**

![image.png](/Images/tuan_4_controlpanel/image%2030.png)

Hiện thông báo rằng quy trình **Backup** đang diễn ra

![image.png](/Images/tuan_4_controlpanel/image%2031.png)

Cuối cùng là tải file nén chứa dữ liệu backup về thôi

![image.png](/Images/tuan_4_controlpanel/image%2032.png)

**Restore**

Ở phần **Tools** → tìm mục **Files** → nhấp vào ô **Backup Wizard**

![image.png](/Images/tuan_4_controlpanel/image%2027.png)

Chọn **Restore**

![image.png](/Images/tuan_4_controlpanel/image%2033.png)

Chọn mục muốn **Restore** dữ liệu

![image.png](/Images/tuan_4_controlpanel/image%2034.png)

Chọn file **Backup** đã lưu từ trước và nhấn **Upload**

![image.png](/Images/tuan_4_controlpanel/image%2035.png)

Chờ thời gian **Restore** xong sẽ nhận được thông báo thành công nếu không xẩy ra lỗi gì:

![image.png](/Images/tuan_4_controlpanel/image%2036.png)

**5. Thao tác quản lý Database**

**Manage My Databases**

Ở phần **Tools** → tìm mục **Databases**→ nhấp vào ô **Manage My Databases**

![image.png](/Images/tuan_4_controlpanel/image%2037.png)

Trong phần **Manage My Databases** sẽ có 2 đối tượng chính đó là **Database** và **User** của **Database**

Đầu tiên ta sẽ thực hiện tạo mới 1 **Database**. Tìm phần **Create New Database**, sau đó nhập tên của **Database** mới và cuối cùng là nhấn nút **Create Database**

![image.png](/Images/tuan_4_controlpanel/image%2038.png)

Sẽ hiện thông báo thành công nếu tạo **Database** mới thành công

![image.png](/Images/tuan_4_controlpanel/image%2039.png)

Tiếp theo, là phần **Modify Databases** để kiểm tra và sửa chữa **Database;** phần **Current Databases để nơi ta có thể biết được các Database hiện có và cũng có thể đổi tên cũng như xóa Database**

![image.png](/Images/tuan_4_controlpanel/image%2040.png)

Kế tiếp là mục Database Users có 2 phần Ađ New User là để tạo tài khoản user mới, còn Add User To Database là thêm user vào Database để có thể sử dụng Database dựa vào quyền đã cấp từ trước đó.

![image.png](/Images/tuan_4_controlpanel/image%2041.png)

![image.png](/Images/tuan_4_controlpanel/image%2042.png)

**phpMyAdmin**

Khi nhấn vào phpMyAdmin sẽ mở tra giao diện quản lý CSDL 

![image.png](/Images/tuan_4_controlpanel/image%2043.png)

![image.png](/Images/tuan_4_controlpanel/image%2044.png)

**6. Thao tác quản lý với Softaculous**

**Cài đặt Softaculous**

Đầu tiên phải Bật **ionCube : Server Configuration → Tweak Settings → chọn tab PHP →** tìm dòng **cPanel PHP loader →** check vào ô **ioncube**

![image.png](/Images/tuan_4_controlpanel/image%2045.png)

**Cài đặt Softaculous qua Terminal**

Chạy lệnh dưới đây để cài đặt **Softaculous:**

```bash
wget -N [https://files.softaculous.com/install.sh](https://files.softaculous.com/install.sh)
chmod 755 [install.sh](http://install.sh/)
./install.sh
```

![image.png](/Images/tuan_4_controlpanel/image%2046.png)

Truy cập vào phần **Plugin** trên WHM và tìm mục **Softaculous Instant Installs**. Nếu thấy plugin xuất hiện, điều đó có nghĩa là Softaculous đã được cài đặt thành công và sẵn sàng để sử dụng.

![image.png](/Images/tuan_4_controlpanel/image%2047.png)

Trên **cPanel**, ở phần **Tools** → tìm phần **Software** → **Softaculous Apps Installer**

![image.png](/Images/tuan_4_controlpanel/image%2048.png)

Với **Softaculous**, bạn có thể cài đặt hàng trăm ứng dụng web nhanh chóng chỉ với vài cú nhấp chuột, mà không cần thực hiện các thao tác kỹ thuật phức tạp

![image.png](/Images/tuan_4_controlpanel/image%2049.png)

## CyberPanel

### **Giới thiệu về CyberPanel**

**CyberPanel** là một bảng điều khiển **hosting** (control panel) miễn phí và mạnh mẽ, được thiết kế đặc biệt cho các hệ thống Linux. Nó cung cấp một giao diện trực quan và dễ sử dụng để quản lý các website, máy chủ web, cơ sở dữ liệu và nhiều dịch vụ khác trên máy chủ của bạn.

### **Ưu điểm**

- **Miễn phí:** Đây là ưu điểm lớn nhất thu hút người dùng. Bạn có thể sử dụng CyberPanel hoàn toàn miễn phí mà không phải trả bất kỳ khoản phí bản quyền nào.
- **Hiệu năng cao:** Nhờ tích hợp với OpenLiteSpeed, một trong những máy chủ web nhanh nhất hiện nay, CyberPanel mang lại tốc độ và hiệu suất vượt trội cho website của bạn.
- **Dễ sử dụng:** Giao diện trực quan, thân thiện với người dùng, giúp bạn quản lý máy chủ một cách dễ dàng, ngay cả khi bạn không có nhiều kiến thức về kỹ thuật.
- **Tính năng đa dạng:** CyberPanel cung cấp đầy đủ các tính năng cần thiết để quản lý một máy chủ web, bao gồm: tạo và quản lý website, cài đặt và quản lý các ứng dụng phổ biến, quản lý cơ sở dữ liệu, quản lý email, quản lý DNS, bảo mật cao.
- **Cộng đồng lớn:** CyberPanel có một cộng đồng người dùng lớn và sôi động, luôn sẵn sàng hỗ trợ bạn khi gặp khó khăn.
- **Hỗ trợ tiếng Việt:** CyberPanel có phiên bản hỗ trợ tiếng Việt, giúp người dùng Việt Nam dễ dàng sử dụng hơn.
- **Linh hoạt:** CyberPanel cho phép bạn tùy chỉnh và cấu hình máy chủ theo ý muốn.

### **Nhược điểm**

- **Cộng đồng nhỏ hơn so với cPanel:** Mặc dù cộng đồng CyberPanel đang phát triển mạnh, nhưng nó vẫn còn nhỏ hơn so với các bảng điều khiển phổ biến như cPanel. Điều này có thể dẫn đến ít tài liệu và hỗ trợ hơn.
- **Tính năng chưa đầy đủ:** So với các bảng điều khiển trả phí, CyberPanel có thể thiếu một số tính năng nâng cao.
- **Cập nhật:** Do là dự án mã nguồn mở, quá trình cập nhật của CyberPanel có thể không ổn định bằng các sản phẩm thương mại.
- **Hỗ trợ kỹ thuật:** Mặc dù có cộng đồng hỗ trợ, nhưng việc tìm kiếm hỗ trợ kỹ thuật chuyên sâu có thể khó khăn hơn so với các bảng điều khiển trả phí.
- **Tùy biến:** Mặc dù CyberPanel cho phép tùy biến, nhưng khả năng tùy biến có thể bị hạn chế so với các giải pháp chuyên dụng hơn.

### Cài đặt CyberPanel

Đầu tiên, trước khi đi vào cài đặt, bạn cần cập nhật hệ thống, chạy lệnh sau sau:

```bash
sudo apt update && sudo apt upgrade -y
```

Tải **CyperPanel Installer:**

```bash
sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)
```

Chạy câu lệnh sau để bắt đầu quá trình cài đặt:

```
sh install.sh
```

Sau khi cài đặt xong, ta có thể kiểm tra bằng cách mở 1 trình duyệt bất kì tìm kiếm `https://your_ip:8090` thì sẽ hiện lên giao diện đăng nhập của CyberPanel, ta nhập tài khoản và mật khẩu mà CyberPanel đã cung cấp sau khi quá trình cài đặt thành công trên cửa sổ dòng lệnh Terminal

![image.png](/Images/tuan_4_controlpanel/image%2050.png)

Sau đó ta sẽ vào được giao diện quản lý của CyberPanel

![image.png](/Images/tuan_4_controlpanel/image%2051.png)

### Thao tác với CyberPanel

**1. Thao tác với domain và sub - domain**

Trước khi muốn làm việc với **domain** và **sub - domain** thì ta cần tạo 1 **website** mới

Ở phần **Websites** → chọn mục **Create Website**

Sau đó điền các thông tin cần thiết để tạo 1 **website** mới  → sau đó nhấn nút **Create Website**

![image.png](/Images/tuan_4_controlpanel/image%2052.png)

Sau khi tạo xong website mới thì ta tiến hành tạo domain và sub - domain 

Ở phần **Websites** → chọn mục **Create Sub/Addon Domain →** chọn loại **Domain** 

![image.png](/Images/tuan_4_controlpanel/image%2053.png)

Tiếp đến là chọn **Website** → nhập **domain name** muốn thêm cho **website** này → tùy chọn nhập **Documen Root Path** → chọn **PHP Version**

![image.png](/Images/tuan_4_controlpanel/image%2054.png)

Sau khi tạo thành công **domain** thêm thì có thể xem lại các **domain** đã có ở **List Sub/Addon Domains**

![image.png](/Images/tuan_4_controlpanel/image%2055.png)

**2. Thao tác với tạo FTP Account**

ở phần **FTP** → chọn mục **Create FTP Account** → điền đầy đủ thông tin của tài khoản FTP muốn tạo → cuối cùng là nhấn **Create FTP Account**

![image.png](/Images/tuan_4_controlpanel/image%2056.png)

Ta có thể xem thông của tài khoản FTP đã tạo : chọn **List FTP Accounts** → chọn **Domain** → hiện **danh sách tài khoản FTP**

![image.png](/Images/tuan_4_controlpanel/image%2057.png)

Kiểm tra lại bằng FireZilla:

![image.png](/Images/tuan_4_controlpanel/image%2058.png)

![image.png](/Images/tuan_4_controlpanel/image%2059.png)

**3. Thao tác cài chứng chỉ SSL** 

**Đối với cài đặt chứng chỉ SSL miễn phí (Let’s Encrypt)**

Ở phần SSL → chọn Manage SSL → chọn Website

![image.png](/Images/tuan_4_controlpanel/image%2060.png)

Hiển thị ra trạng thái hiện có của chứng chỉ SSL nếu có; nếu chưa có bạn có thể nhấn nút Issue SSL để cấp phát chứng chỉ SSL miễn phí (Let’s Encrypt)

![image.png](/Images/tuan_4_controlpanel/image%2061.png)

Sau khi cấp phát thành công thì sẽ hiện thông báo

![image.png](/Images/tuan_4_controlpanel/image%2062.png)

**Đối với cài đặt chứng chỉ SSL trả phí (DV, OV, …)**

Ở phần **Website** → chọn mục **List Websites** → nhấn vào nút **Manage** của **Website** cần cài chứng chỉ SSL

![image.png](/Images/tuan_4_controlpanel/image%2063.png)

Sẽ được chuyển sang trang quản lý của website đó → kéo xuống phần **Configurations → Add SSL**

![image.png](/Images/tuan_4_controlpanel/image%2064.png)

![image.png](/Images/tuan_4_controlpanel/image%2065.png)

Dán nội dung của file .key và file .crt vào 2 ô hiện bên dưới và nhấn **Save**

![image.png](/Images/tuan_4_controlpanel/image%2066.png)

Kiểm tra lại chứng chỉ đã cài được hay chưa tài web [SSL Checker](https://www.sslshopper.com/ssl-checker.html)

**4. Thao tác Backup và Restore dữ liệu**

**Backup dữ liệu**

Ở phần Backup → chọn Create Backup → chọn Website muốn backup dữ liệu - chọn thư mục lưu trữ file backup → nhấn nút Create Backup

Sau khi back up xong thì sẽ hiện thông tin file backup ở dưới phần **Existing Backups**

![image.png](/Images/tuan_4_controlpanel/image%2067.png)

Theo mặc định file nằm trong đường dẫn `/home/domain_cua_ban/backup`. Ở đây bạn sẽ tìm được file có tên theo định dạng sau:

```
backup-ten_domain-thoi_gian.tar.gz
```

Bạn dowload file này bằng cách chuột phải và chọn nút **Download**.

Bạn gõ các lệnh sau để thực hiện tạo di chuyển file sao lưu sang file **/home/backup**:

```bash
mkdir -p /home/backup // tạo file backup

mv /home/domain-cua-ban/backup/tên-file-backup.tar.gz /home/backup // di chuyển file sao lưu đến vị trí vừa tạo

cd /home/backup // kiểm tra kết quả
ls -la
```

![image.png](/Images/tuan_4_controlpanel/image%2068.png)

**Restore dữ liệu**

Đầu tiên ta cần xóa **Website** cần **Restore** dữ liệu để tránh xung đột , ở phần **Websites** → **Delete Website** → chọn **Website** cần xóa → nhấn **Delete Website**

![image.png](/Images/tuan_4_controlpanel/image%2069.png)

Vào mục **Restore** trong phần **Backup** → chọn file backup trước đó để restore dữ liệu và nhấn **Start Restoration**

![image.png](/Images/tuan_4_controlpanel/image%2070.png)

Vào lại **Websites  → List Websites ,** xem đã thấy website đã restore chưa

![image.png](/Images/tuan_4_controlpanel/image%2071.png)

**5. Thao tác quản lý Databases**

Ở phần **Databases** → mục **Create Database** → nhập thông tin database và tài khoản sử dụng database

![image.png](/Images/tuan_4_controlpanel/image%2072.png)

Ngoài ra cũng có các chức năng như xóa , xem danh sách, và truy cập trang quản lý của PHPMYAdmin

![image.png](/Images/tuan_4_controlpanel/image%2073.png)