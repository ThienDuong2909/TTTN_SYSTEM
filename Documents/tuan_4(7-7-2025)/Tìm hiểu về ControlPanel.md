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
  - [aaPanel](#aapanel)
    - [Giới thiệu về aaPanel](#giới-thiệu-về-aapanel)
    - [Ưu điểm](#ưu-điểm-của-aapanel)
    - [Nhược điểm](#nhược-điểm-của-aapanel)
    - [Cài đặt aaPanel](#cài-đặt-aapanel)
    - [Thao tác với aaPanel](#thao-tác-với-aapanel)
  - [FastPanel](#fastpanel)
    - [Giới thiệu về FastPanel](#giới-thiệu-về-fastpanel)
    - [Cài đặt FastPanel](#cài-đặt-fastpanel)
    - [Thao tác với FastPanel](#thao-tác-với-fastpanel)
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


# aaPanel

### Giới thiệu về aaPanel

**aaPanel** là một bảng điều khiển hosting (control panel) mã nguồn mở, miễn phí, được thiết kế để giúp người dùng quản lý máy chủ web một cách dễ dàng và hiệu quả hơn. Nó cung cấp một giao diện đồ họa thân thiện, giúp bạn thực hiện các tác vụ quản lý máy chủ mà không cần phải sử dụng các lệnh phức tạp.

![image.png](/Images/tuan_4_controlpanel/image%2074.png)

![image.png](/Images/tuan_4_controlpanel/image%2075.png)

### **Ưu điểm của aaPanel**

- **Miễn phí và mã nguồn mở:** Đây là ưu điểm lớn nhất của aaPanel, cho phép bạn sử dụng và tùy chỉnh hoàn toàn miễn phí.
- **Giao diện trực quan, dễ sử dụng:** Giao diện được thiết kế đơn giản, thân thiện với người dùng, ngay cả những người mới bắt đầu cũng có thể dễ dàng làm quen.
- **Tính năng đa dạng:** aaPanel hỗ trợ quản lý nhiều dịch vụ như web server, database, FTP, email, DNS, … giúp bạn quản lý toàn bộ máy chủ một cách tập trung.
- **Cài đặt ứng dụng một click:** Dễ dàng cài đặt các CMS phổ biến như WordPress, Joomla, Laravel,…
- **Cộng đồng lớn:** Có một cộng đồng người dùng đông đảo, sẵn sàng hỗ trợ khi bạn gặp khó khăn.
- **Tùy biến cao:** Bạn có thể tùy chỉnh nhiều cấu hình để phù hợp với nhu cầu của mình.
- **Hiệu năng tốt:** aaPanel hoạt động khá mượt mà trên các VPS có cấu hình thấp.

### **Nhược điểm của aaPanel**

- **Cộng đồng còn khá mới:** So với các control panel khác như cPanel, cộng đồng của aaPanel còn tương đối mới, do đó tài liệu và hỗ trợ có thể chưa đầy đủ.
- **Tính năng chưa đa dạng bằng các sản phẩm thương mại:** Một số tính năng nâng cao có thể chưa được tích hợp sẵn trong aaPanel, bạn cần phải cài đặt thêm các plugin hoặc module.
- **Bảo mật:** Mặc dù có các tính năng bảo mật cơ bản, nhưng bạn cần tự mình cấu hình và cập nhật các bản vá để đảm bảo an toàn cho máy chủ.
- **Tài liệu tiếng Việt còn hạn chế:** Phần lớn tài liệu hướng dẫn của aaPanel được viết bằng tiếng Anh, có thể gây khó khăn cho người dùng không thành thạo ngoại ngữ.

### Cài đặt aaPanel

**Bước 1: Cập nhập các gọi** 

```bash
sudo apt update && sudo apt upgrade
```

**Bước 2. Tải xuống script install aaPanel**

Tải xuống script cài đặt aaPanel trước với lệnh sau:

```
wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh
```

Sau khi script được tải xuống, thực thi với lệnh dưới đây:

```
chmod +x install.sh
```

**Bước 3. Bắt đầu quá trình install aaPanel Ubuntu 22.04**

Để bắt đầu quá trình install aaPanel, thực hiện các bước sau:

```
bash install.sh
```

Sau khi thực hiện lệnh này, bạn sẽ được hỏi liệu bạn muốn cài đặt aaPanel vào **/var/www/** hay không. Nhập "`y`", nhấn Enter và đợi một chút để hoàn tất quá trình cài đặt.

![image.png](/Images/tuan_4_controlpanel/image%2076.png)

Sau khi cài đặt thành công, bạn sẽ nhận được thông báo sau

![image.png](/Images/tuan_4_controlpanel/image%2077.png)

Ta sẽ nhận được địa chỉ truy cập , username và password để đăng nhập vào trang quản lý aaPanel

Cuối cùng ta truy cập vào đường dẫn mà aaPanel cung cấp và đăng nhập vào trang quản lý chính của aaPanel

![image.png](/Images/tuan_4_controlpanel/image%2078.png)

Nhập tài khản mật khẩu được cấp để vào giao diện quản lý

![image.png](/Images/tuan_4_controlpanel/image%2079.png)

### Thao tác với aaPanel

**1. Thao tác với domain và sub - domain**

Ở mục **Websites** → nhấn vào **Add site**

![image.png](/Images/tuan_4_controlpanel/image%2080.png)

Tại giao diện bên dưới, bạn thực hiện thêm vào domain mới nhé. Mình sẽ chú thích một vài thông số quan trọng để bạn nắm.

- **Domain**: Tên miền website bạn cần sử dụng, bạn nên thêm cả www và non-www (mỗi tên miền là một dòng khác nhau).
- **Description**: Mô tả của website, mặc định nó ghi tên miền website vào và bạn có thể sửa lại.
- **Root directory**: Thư mục gốc của website này trên máy chủ, bạn nên để nguyên.
- **FTP**: Nếu bạn muốn tạo một tài khoản riêng FPT cho website này thì chọn Create.
- **Database**: Nếu bạn muốn tạo một database riêng cho website này thì chọn Create, hoặc có thể tạo sau.
- **PHP Version**: Phiên bản PHP dành cho website, bạn có thể cài thêm phiên bản PHP tại mục App Store trong AAPanel.

![image.png](/Images/tuan_4_controlpanel/image%2081.png)

![image.png](/Images/tuan_4_controlpanel/image%2082.png)

Sau khi nhập đầy đủ thông tin cần thiết thì nhấn **Confirm, chờ 1 lát bạn sẽ nhận được thông báo tạo thành công nếu không xẩy ra lỗi gì**

![image.png](/Images/tuan_4_controlpanel/image%2083.png)

Để thêm nhiều tên miền mới cho cùng 1 site thì Nhấn vào **dấu 3 chấm** của site đó → **Security Scan** → chọn tab **Domain Manager** → nhập **Domain mới** → nhấn **Add**

![image.png](/Images/tuan_4_controlpanel/image%2084.png)

![image.png](/Images/tuan_4_controlpanel/image%2085.png)

![image.png](/Images/tuan_4_controlpanel/image%2086.png)

**2. Thao tác với FTP Account**

Nhấn vào mục FTP, ta sẽ vào giao diện quản lý những thao tác liên quan đến FTP 

![image.png](/Images/tuan_4_controlpanel/image%2087.png)

Để thêm 1 tài khoản FTP thì ở giao diện này → nhấn **Add FTP**

![image.png](/Images/tuan_4_controlpanel/image%2088.png)

Nhập username, password và chọn Document root cho tài khoản FTP mới đó → xong nhấn **Confirm**

Chờ 1 lát để tạo tài khoản, xong sẽ hiển thị thông tin tài khoản vừa tạo ở trang quản lý FTP

![image.png](/Images/tuan_4_controlpanel/image%2089.png)

**3. Thao tác cài chứng chỉ SSL**

**Đối với chứng chỉ miễn phí (Let’s Encrypt)**

Để cài chứng chỉ SSL miễn phí cho một website nào đó trên AAPanel, bạn vào **AAPanel => Website => Conf** trên website mà bạn cần cài đặt.

![image.png](/Images/tuan_4_controlpanel/image%2090.png)

Chọn tab **SSL** → **Let’s Encrypt** → chọn **File Verification** → tích vào những **domain** muốn cài chứng chỉ → nhấn **Apply** 

![image.png](/Images/tuan_4_controlpanel/image%2091.png)

Chờ 1 lát để cài chứng chỉ 

![image.png](/Images/tuan_4_controlpanel/image%2092.png)

**Đối với chứng chỉ trả phí(EV, OV, …)**

Cũng ở mục SSL trong **Conf** → nhấn tab **Current Cert** (Other certificate) → dán nội dung của 2 file .key và .crt mà bạn có lúc đã mua chứng chỉ → nhấn **Save and enable SSL** 

![image.png](/Images/tuan_4_controlpanel/image%2093.png)

**4. Thao tác Backup và Restore dữ liệu**

**Backup dữ liệu**

*Backup dữ liệu cách thủ công*

**Đối với dữ liệu website**

Ở mục Website → theo cột Backup, nhân vào chữ (Today) hoặc số (2)

![image.png](/Images/tuan_4_controlpanel/image%2094.png)

Nhấn vào nút **Backup →** hiện thông báo backup thành công → kiểm tra bảng bên dưới xem có bản backup mới chưa

![image.png](/Images/tuan_4_controlpanel/image%2095.png)

**Đối với dữ liệu Database**

Ở mục **Databases → theo cột Backup, nhấn vào tương ứng database muốn backup** 

![image.png](/Images/tuan_4_controlpanel/image%2096.png)

Nhấn **Backup** → xem lại bảng dữ liệu **Backup** bên dưới

![image.png](/Images/tuan_4_controlpanel/image%2097.png)

*Backup dữ liệu tự động*

Ở mục **Cron** → **Add Task**

Tại mục “Task type”, bạn có thể tùy chọn các thao tác backup như:

***Backup Site:** sao lưu trang web muốn hoặc cũng có thể tùy chọn sao lưu tất cả các trang web hiện đang được quản lý trên aaPanel.*

***Backup Database:** sao lưu cơ sở dữ liệu của 1 trang web, bạn cũng có thể sao lưu tất cả các cơ sở dữ liệu.*

***Backup Directory:** sao lưu một file/thư mục cụ thể.*

![image.png](/Images/tuan_4_controlpanel/image%2098.png)

![image.png](/Images/tuan_4_controlpanel/image%2099.png)

![image.png](/Images/tuan_4_controlpanel/image%20100.png)

![image.png](/Images/tuan_4_controlpanel/image%20101.png)

*Trong đó:*

- **Execution cycle**: bạn có thể tùy chọn set thời gian cụ thể thực hiện backup tự động theo tuần, ngày giờ backup.
- **Backup to:** Chọn nơi lưu trữ file backup, bạn có thể cài đặt các plugin lưu trữ như: Google Cloud, AWS S3, dung lượng lưu trữ FTP, Google Drive hoặc có thể lưu file backup trên ổ đĩa máy chủ hiện tại (Disk of server). Nếu lưu vào ổ đĩa máy chủ thì đường dẫn mặc định là: /www/backup/”database_or_site”
- **Retain the latest:** số lượng bản sao lưu dự trữ. Hệ thống mặc định là 3, bạn có thể thay đổi theo nhu cầu của mình.
- **Backup reminder:** Set thông báo khi hệ thống thực hiện backup thành công.
- Nhấp vào nút "**Add Task**" ****để hoàn thành.

**Lưu ý:** Bạn nên kiểm tra xem tác vụ sao lưu tự động đã hoàn tất chưa. Việc không đủ dung lượng ổ đĩa, lỗi mật khẩu cơ sở dữ liệu hoặc mạng không ổn định…sẽ dễ dẫn đến việc sao lưu không đầy đủ các tệp.

**Restore dữ liệu**

Ở phần Website → giống theo cột Backup, nhấn vào website cần **Restore** 

![image.png](/Images/tuan_4_controlpanel/image%20102.png)

Dưới phần bảng hiễn thị những file backup trước đó → nhấn **Restore** ở file backup muốn restore dữ liệu về 

![image.png](/Images/tuan_4_controlpanel/image%20103.png)

Chờ 1 lát để tiến hành restore dữ liệu Database, sau khi xữ lý xong sẽ hiện thông báo thành công

![image.png](/Images/tuan_4_controlpanel/image%20104.png)

# FastPanel

### Giới thiệu về FastPanel

**FastPanel** là ****phần mềm quản trị máy chủ miễn phí được thiết kế để đơn giản hóa việc quản lý hosting. Tương tự như các công cụ nổi tiếng như cPanel hay DirectAdmin, FastPanel nổi bật với giao diện thân thiện và cấu trúc tinh gọn, giúp người dùng dễ dàng thao tác ngay cả khi không có nhiều kinh nghiệm kỹ thuật. Đây là giải pháp lý tưởng cho quản trị viên hệ thống và chủ website muốn tiết kiệm chi phí nhưng vẫn đảm bảo hiệu suất cao.

### Cài đặt FastPanel

**Bước 1: Cập nhật các gói phần mềm**

```bash
sudo apt update && sudo apt upgrade
```

**Bước 2: Cài đặt các gói cần thiết**

```bash
sudo apt install -y wget curl
```

**Bước 3: Tải xuống và cài đặt Trình cài đặt FastPanel**

```bash
wget http://repo.fastpanel.direct/install_fastpanel.sh -O - | sudo bash
```

Chờ 1 khoảng thời gian, kết quả sẽ la:

![image.png](/Images/tuan_4_controlpanel/image%20105.png)

**Chú ý:** sẽ có tài khoản và mật khẩu để đăng nhập vào trình quản lý của FastPanel, bạn nên ghi chú lại.

Bước 4: Kiểm tra hoạt động của FastPanel

Sau khi cài đặt hoàn tất, bạn có thể truy cập FastPanel thông qua trình duyệt web của mình. URL mặc định là:

```
http://your_server_ip:8888
```

Sau đó nhập email để xác nhận, bạn sẽ nhận được mail có gắn link xác nhận chỉ cần nhấn vào link đó là bạn đã xác nhận thành công.

![image.png](/Images/tuan_4_controlpanel/image%20106.png)

Trở về URL mặc định [`http://your_server_ip:8888`](http://your_server_ip:8888) bạn sẽ vào được trang **License Agreement → nhấn Accept**

![image.png](/Images/tuan_4_controlpanel/image%20107.png)

Cuối cùng sẽ vào được giao diện quản lý của FastPanel

![image.png](/Images/tuan_4_controlpanel/image%20108.png)

### Thao tác với FastPanel

**1. Thao tác với domain và sub - domain**

Ở trang chủ của FastPanel → nhấn nút Create site

![image.png](/Images/tuan_4_controlpanel/image%20109.png)

Lúc này sẽ có 2 tuỳ chọn gồm:.

- **Create a CMS based site**: Thêm tên miền và cài sẵn WordPress.
- **Create a site manually**: Chỉ thêm tên miền (domain).

Trong nội dung bài hướng dẫn này là thêm tên miền vào FastPanel nên mình sẽ nhấn **Create a site manually** (Chỉ thêm tên miền).

![image.png](/Images/tuan_4_controlpanel/image%20110.png)

Đến đây, bạn điền tên miền cần thêm ở mục **Which domain to bind?** và bấm **Next step.**

![image.png](/Images/tuan_4_controlpanel/image%20111.png)

FastPanel cũng sẽ tự động tạo ra các thông tin liên quan cho bạn gồm:

- **User**: Mỗi tên miền sẽ có 1 user riêng giúp tăng cường bảo mật.
- **PHP**: Lựa chọn phiên bản PHP phù hợp với website.
- **Database**: FastPanel cung cấp 1 Database gồm nhiều ký tự tương ứng cho website
- **Backup copies**: Sao lưu dữ liệu trên FastPanel
- **FPT account**: Tài khoản sử dụng để kết nối FTP

Sau khi kiểm tra các thông tin trên, bạn nhấn **Create site**.

![image.png](/Images/tuan_4_controlpanel/image%20112.png)

Sau đó bạn sẽ nhận thông báo thêm tên miền (domain) vào FastPanel thành công như hình minh họa dưới đây. 

![image.png](/Images/tuan_4_controlpanel/image%20113.png)

Để tạo **sub - domain cho website thì tại trang chủ - chọn website muốn thêm domain →  Site card**

![image.png](/Images/tuan_4_controlpanel/image%20114.png)

Trong **Site managing** → chọn **Subdomains** → vào được giao diện quản lý subdomains rồi → chọn **Create**

![image.png](/Images/tuan_4_controlpanel/image%20115.png)

![image.png](/Images/tuan_4_controlpanel/image%20116.png)

Nhập **sub - domain** → chọn thư mục chứa **Subdirectory →** nhấn **Save**

![image.png](/Images/tuan_4_controlpanel/image%20117.png)

Chờ 1 lát để tạo mới, kết quả sẽ là 

![image.png](/Images/tuan_4_controlpanel/image%20118.png)

**2. Thao tác tạo FTP Account**

Mở **sidebar** → Chọn **Management** → tìm **FTP account** và chọn → nhập đầy đủ thông tin → nhấn **Save**

![image.png](/Images/tuan_4_controlpanel/image%20119.png)

Trong đó:

- **Login:** Bạn sẽ cần nhập username sẽ sử dụng. Không được viết dấu và không được có khoảng trắng.
- **Password:** Bạn cần nhập mật khẩu tối thiểu 8 ký tự, có ít nhất một chữ viết hoa, một chữ viết thường và một ký tự đặc biệt. Ví dụ một mật khẩu hợp lệ sẽ là: Azd!giNo1. Hoặc đơn giản hơn chúng ta có thể chọn nút **generate** để FASPANEL tự tạo mật khẩu.
- **Owner:** Tài khoản FPT sắp tạo sẽ được quản lý/sở hữu bởi người dùng nào, nếu bạn cần tạo cho bạn sử dụng thì có thể bỏ qua phần này.
- **Site:** Nếu bạn cần tạo tài khoản FTP này cho website nào thì hãy chọn một website ở ô này.
- **Home directory:** Tại đây bạn sẽ cấp quyền nơi tài khoản FTP này có thể truy cập đến. Còn nếu bạn đã chọn một website ở Site thì có thể bỏ qua mục này.
- **Permissions:** Tại đây sẽ có hai tùy chọn **Allow all**(Cho phép tất cả các quyền) hoặc **Read only**(Chỉ cho xem mà không được xóa hay chỉnh sửa gì cả) tùy theo nhu cầu của bạn mà bạn chọn, tuy nhiên hầu hết chúng ta sẽ cần **Allow All**.

**3. Thao tác cài chứng chỉ SSL**

**Đối với chứng chỉ miễn phí (Let’s Encrypt)**

Mở **Sidebar** → chọn **SSL certificates** → nhấn **New certificate**

Chọn mục **Let’s Encrypt** → chọn **Site** cần cài SSL → chọn **Key length** 

**Lưu ý:** Key length càng dài sẽ càng bảo mật cao, nhưng thời gian xác thực sẽ dài hơn →Nhập email xác thực và Save

![image.png](/Images/tuan_4_controlpanel/image%20120.png)

![image.png](/Images/tuan_4_controlpanel/image%20121.png)

**Đối với chứng chỉ trả phí (EV, OV, …)**

Chọn **New certificate** → **Existing**

Ta mở các file mà nhà cung cúng SSL đã gửi sau đó dán các phần cần thiết vào:

- File .key – Private Key
- File .crt - Certificate
- Đối với Chain sẽ có hoặc không tùy theo nhà cung cấp SSL

![image.png](/Images/tuan_4_controlpanel/image%20122.png)

Sau đó chờ để xác thực và hoàn thành cài chứng chỉ

**4. Backup và Restore dữ liệu**

Mở **Sidebar** → chọn **Management** → chọn **Backup copies →** trong tap **Accounts** nhấn nút **New account**

![image.png](/Images/tuan_4_controlpanel/image%20123.png)

Có nhiều tùy chọn để lưu trữ các file backup, ta sẽ bắt đầu với ví dụ local trước. Nhập thông tin → **Save**

![image.png](/Images/tuan_4_controlpanel/image%20124.png)

Sau khi hoàn tất tạo tài khoản backup, ta sẽ di chuyển quan tab **Plans** → Chọn **New plan**

![image.png](/Images/tuan_4_controlpanel/image%20125.png)

Ta tiến hành cấu hình với các thông số cần lưu ý như sau:

- **Account:** tài khoản backup đã tạo ở trên
- **Backup copies limit:** Số lượng backup lưu trữ
- **Sites**: Website cần backup
- **Databases:** Database cần backup
- **Start time:** Thời điểm chạy backup, chọn template cấu hình sẵn hoặc có thể tự tinh chỉnh

![image.png](/Images/tuan_4_controlpanel/image%20126.png)

Hoàn tất, có thể bấm Run now để kiểm tra khả năng backup

![image.png](/Images/tuan_4_controlpanel/image%20127.png)

![image.png](/Images/tuan_4_controlpanel/image%20128.png)

Thành quả 

![image.png](/Images/tuan_4_controlpanel/image%20129.png)

Để Restore dữ liệu từ bản backup thì đầu tiên, truy cập danh sách các bản copy hiện có

![image.png](/Images/tuan_4_controlpanel/image%20130.png)

Chọn vào bản copy muốn restore

![image.png](/Images/tuan_4_controlpanel/image%20131.png)

Xác nhận (lưu ý và kiểm tra kỹ lưỡng trước khi thực hiện)

![image.png](/Images/tuan_4_controlpanel/image%20132.png)

Thông báo Restore thành công

![image.png](/Images/tuan_4_controlpanel/image%20133.png)

**5. Thao tác quản lý Database**

**Tạo Database mới**

Mở Sidebar → chọn Management → chọn Databases

![image.png](/Images/tuan_4_controlpanel/image%20134.png)

Chọn New database để tiến hành tạo database

![image.png](/Images/tuan_4_controlpanel/image%20135.png)

Ta sẽ điền các thông tin liên quan gồm:

- **Name:** Tên Database
- **Character Set:** Bộ ký tự (nên để mặc định)
- **Owner:** User sở hữu database (hay còn gọi là user database, có thể thay đổi sau)
- **Website:** Website cần gán database
- **User:** Lựa chọn tạo mới
- **Login:** Database user
- **Password:** Mật khẩu của Database user
- **Confirm the password:** Xác nhận lại mật khẩu

![image.png](/Images/tuan_4_controlpanel/image%20136.png)

**Import/Export bằng giao diện phpMyAdmin**

Chọn Open phpMyAdmin hoặc nút mũi tên chéo để mở thẳng trang quản lý cho database chỉ định

![image.png](/Images/tuan_4_controlpanel/image%20137.png)

Chọn đúng database cần thao tác

![image.png](/Images/tuan_4_controlpanel/image%20138.png)

Chọn Import nếu ta cần nhập databse

![image.png](/Images/tuan_4_controlpanel/image%20139.png)

Các định dạng cơ bản hỗ trợ gồm:

- .sql
- .sql.zip
- .sql.gzip

Kéo xuống dưới cùng → Import

![image.png](/Images/tuan_4_controlpanel/image%20140.png)

Hoàn tất import, thời gian thực thi sẽ dựa theo kích thước của file database

![image.png](/Images/tuan_4_controlpanel/image%20141.png)

Đối với Export, ta chọn database và qua tab Export, Chọn định dạng mong muốn => Export

![image.png](/Images/tuan_4_controlpanel/image%20142.png)

**Import/Export bằng giao diện FastPanel**

Chọn vào nút 3 chấm của database cần thao tác => Upload SQL-dump

![image.png](/Images/tuan_4_controlpanel/image%20143.png)

Chọn file → Chờ → Hoàn tất import

![image.png](/Images/tuan_4_controlpanel/image%20144.png)

Đối với Export, ta sẽ chọn Create SQL dump via dump để tạo file trước nếu chưa có tùy chọn Download SQL dump

![image.png](/Images/tuan_4_controlpanel/image%20145.png)

Hoàn tất dump

![image.png](/Images/tuan_4_controlpanel/image%20146.png)

Sau đó chọn Download SQL dump để tải file .sql về máy

![image.png](/Images/tuan_4_controlpanel/image%20147.png)

![image.png](/Images/tuan_4_controlpanel/image%20148.png)

**LƯU Ý**: 1 số bước không thể hoàn tác, cần hết sức cẩn thận khi thực hiện

**Chỉnh sửa Database**

Chọn Edit database nếu ta cần gán lại user sở hữu database và website

![image.png](/Images/tuan_4_controlpanel/image%20149.png)

![image.png](/Images/tuan_4_controlpanel/image%20150.png)

Các thao tác liên quan đến user database, ta chọn User Management

![image.png](/Images/tuan_4_controlpanel/image%20151.png)

Có 3 thao tác cơ bản gồm:

- **Edit:** Thay đổi mật khẩu hoặc cho phép user kết nối từ xa vào database
    
    ![image.png](/Images/tuan_4_controlpanel/image%20152.png)
    
    ![image.png](/Images/tuan_4_controlpanel/image%20153.png)
    
- **Unbind:** Gỡ liên kết user khỏi 1 database (vui lòng cẩn thận khi thao tác nếu không nắm rõ)
    
    ![image.png](/Images/tuan_4_controlpanel/image%20154.png)
    
    ![image.png](/Images/tuan_4_controlpanel/image%20155.png)
    

- Delete user: Xóa user
    
    ![image.png](/Images/tuan_4_controlpanel/image%20156.png)
    
    ![image.png](/Images/tuan_4_controlpanel/image%20157.png)
    

**Kết nối Database từ xa**

Đầu tiên, cần đảm bảo user của database có quyền kết nối từ xa

![image.png](/Images/tuan_4_controlpanel/image%20158.png)

![image.png](/Images/tuan_4_controlpanel/image%20159.png)

![image.png](/Images/tuan_4_controlpanel/image%20160.png)

Tiếp theo, ta sẽ cần cho phép MySQL lắng nghe kết nối từ mọi nguồn (0.0.0.0). Truy cập Database servers

![image.png](/Images/tuan_4_controlpanel/image%20161.png)

Chọn Config Variables

![image.png](/Images/tuan_4_controlpanel/image%20162.png)

Nhập bind ở thanh tìm kiếm và Enter, ta sẽ thấy MySQL đang lắng nghe localhost (127.0.0.1). Cần chỉnh thành 0.0.0.0 để kết nối từ xa

![image.png](/Images/tuan_4_controlpanel/image%20163.png)

![image.png](/Images/tuan_4_controlpanel/image%20164.png)

Chọn Save để lưu thông số

![image.png](/Images/tuan_4_controlpanel/image%20165.png)

Thông tin kết nối sẽ bao gồm:

- User database:
- Password:
- Host: Địa chỉ IP của VPS

![image.png](/Images/tuan_4_controlpanel/image%20166.png)

Sau khi nhập đầu đủ, chính xác thông tin thì nhấn **Test Connection** và kết quả là thông báo kết nối thành công

![image.png](/Images/tuan_4_controlpanel/image%20167.png)

Nhấn ok và kết nối vào Database, ta có thể thấy Database như ở trên PHPMyAdmin

![image.png](/Images/tuan_4_controlpanel/image%20168.png)

---

THE END