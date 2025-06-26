# Tìm Hiểu và Cấu Hình DNS Trên Linux
## Mục Lục
- [1. Giới Thiệu về DNS](#1-giới-thiệu-về-dns)
- [2. Các Loại Bản Ghi DNS](#2-các-loại-bản-ghi-dns)
  - [2.1 A Record](#21-a-record)
  - [2.2 CNAME Record](#22-cname-record)
  - [2.3 MX Record](#23-mx-record)
  - [2.4 TXT Record](#24-txt-record)
  - [2.5 NS Record](#25-ns-record)
  - [2.6 SRV Record](#26-srv-record)
- [3. Cơ Chế Hoạt động của DNS](#3-cơ-chế-hoạt-động-của-dns)
- [4. Cấu Hình DNS Trên Máy Chủ Linux](#4-cấu-hình-dns-trên-máy-chủ-linux)
- [5. Xác Minh Máy Chủ DNS Từ Máy Khách](#5-xác-minh-máy-chủ-dns-từ-máy-khách)

---

## 1. Giới Thiệu về DNS

**[DNS](https://en.wikipedia.org/wiki/Domain_Name_System)** (*Domain Name System*) là hệ thống phân giải tên miền, chuyển đổi từ tên miền dạng `www.tenmien.com` sang địa chỉ IP tương ứng và ngược lại. DNS đóng vai trò quan trọng trong việc liên kết các thiết bị mạng, định vị và gán địa chỉ cụ thể cho thông tin trên **Internet**.

---

## 2. Các Loại Bản Ghi DNS

Dưới đây là **bảy loại bản ghi DNS** phổ biến:

### 2.1 A Record
- **Mô tả**: Ánh xạ tên miền sang địa chỉ IP, là bản ghi cơ bản để truy cập website.
- **Vai trò**: Nếu thiếu, URL không phân giải được, dẫn đến lỗi "không phân giải".
- **Ví dụ**: Bản ghi `A` cho `example.com` trả về địa chỉ IP `103.168.1.100`.
- **Trường Data**: Luôn là địa chỉ IP.

### 2.2 CNAME Record
- **Mô tả**: Cho phép tên miền có nhiều bí danh (*alias*), trỏ về tên miền chính.
- **Yêu cầu**: Cần khai báo bản ghi `A` trước.
- **Ví dụ**: `www.example.com` trỏ về `example.com` qua `CNAME`, sau đó `example.com` phân giải thành IP qua `A`.
- **Ký tự đại diện**: Có thể dùng `*` để đáp ứng mọi yêu cầu cho subdomain chưa khai báo.

### 2.3 MX Record
- **Mô tả**: Xác định máy chủ xử lý email cho tên miền hoặc subdomain.
- **Đặc điểm**: Có thể trỏ đến dịch vụ email bên thứ ba (ví dụ: Gmail).
- **Trường Data**:
  - **Priority**: Số ưu tiên (số nhỏ hơn = ưu tiên cao hơn).
  - **Exchange**: Máy chủ email đích.
- **Ví dụ**: `MX` trỏ email `example.com` đến `mail.google.com`.

### 2.4 TXT Record
- **Mô tả**: Liên kết văn bản tùy ý với tên miền, chủ yếu dùng để xác thực máy chủ.
- **Ví dụ**: Xác minh quyền sở hữu tên miền cho Google Workspace.

### 2.5 NS Record
- **Mô tả**: Xác định máy chủ DNS quản lý tên miền (*Name Server*).
- **Nội dung**: Địa chỉ IP của DNS server và thông tin về tên miền.
- **Ví dụ**: `NS` trỏ `example.com` đến `ns1.thienduong.com`.

### 2.6 SRV Record
- **Mô tả**: Xác định vị trí dịch vụ đặc biệt (ví dụ: tên máy chủ, cổng dịch vụ).
- **Các trường**:
  - Tên dịch vụ (*service*).
  - Giao thức (*protocol*).
  - Tên miền (*domain name*).
  - TTL, Class, Ưu tiên (*Priority*), Trọng lượng (*Weight*), Cổng (*Port*), Máy chủ đích (*Target*).
- **Ví dụ**: `SRV` cho dịch vụ VoIP chỉ định cổng và máy chủ.

---

## 3. Cơ Chế Hoạt động của DNS

1. **Gửi yêu cầu**: Máy người dùng gửi truy vấn tên miền (ví dụ: `example.com`) đến **name server** cục bộ.
2. **Kiểm tra cục bộ**: Name server cục bộ kiểm tra cache hoặc cơ sở dữ liệu:
   - Nếu có, trả về địa chỉ IP.
   - Nếu không, chuyển yêu cầu lên cấp cao hơn.
3. **Truy vấn root server**: Name server cục bộ hỏi **root server**, được hướng dẫn đến máy chủ quản lý tên miền cấp cao (ví dụ: `.com`).
4. **Truy vấn máy chủ tên miền**: Name server cục bộ hỏi máy chủ quản lý tên miền (ví dụ: máy chủ `.com`) để lấy địa chỉ máy chủ của tên miền cụ thể (ví dụ: `example.com`).
5. **Lấy địa chỉ IP**: Máy chủ tên miền trả về địa chỉ IP (ví dụ: `192.168.0.100`) cho name server cục bộ.
6. **Trả kết quả**: Name server cục bộ truyền địa chỉ IP về máy người dùng, kết nối được thiết lập với server chứa website.

---

## 4. Cấu Hình DNS Trên Máy Chủ Linux

### Chuẩn bị
- **Môi trường**: 2 máy chủ Ubuntu 20.04:
  - **DNS Server**: IP `192.168.0.100`.
  - **Client**: IP `192.168.56.4` (dùng để kiểm tra).
- **Mục tiêu**: Cấu hình DNS server với tên miền `thienduong.com` và kiểm tra từ máy khách.

### Các bước thực hiện

**Bước 1: Cài đặt BIND9**  
Cập nhật hệ thống và cài đặt gói **BIND9**:
```bash
sudo apt update
sudo apt install bind9 bind9utils bind9-doc
```
![Install BIND9](/Images/apt-update-new.png)
![Install BIND9](/Images/install-bind9.png)

**Bước 2: Cấu hình chế độ IPv4**  
Chỉnh sửa tệp `/etc/default/named` để chạy BIND9 chỉ với IPv4:
```bash
sudo nano /etc/default/named
```
Thêm dòng:
```plaintext
OPTIONS="-u bind -4"
```
![Named Config](/Images/option-bind-4.png)

**Bước 3: Khởi động lại dịch vụ BIND9**  
Khởi động lại và kiểm tra trạng thái dịch vụ:
```bash
sudo systemctl restart named
sudo systemctl status named
```
![Named Status](/Images/named-restart.png)

**Bước 4: Cấu hình tùy chọn BIND9**  
Chỉnh sửa tệp `/etc/bind/named.conf.options`:
```bash
sudo nano /etc/bind/named.conf.options
```
Cấu hình như sau:
```plaintext
options {
    directory "/var/cache/bind";
    recursion yes;
    allow-recursion { 192.168.0.0/24; };
    listen-on port 53 { 192.168.0.100; };
    allow-transfer { none; };
    allow-query { localhost; 192.168.0.0/24; };
    forwarders { 8.8.8.8; 1.1.1.1; };
};
```
![Named Options Config](/Images/named-option-config.png)

**Bảng giải thích cấu hình**:

| **Tùy chọn**             | **Giá trị**                     | **Mô tả ngắn gọn**                                                                 |
|--------------------------|---------------------------------|-----------------------------------------------------------------------------------|
| `directory`              | `"/var/cache/bind"`             | Thư mục lưu file zone và cache của BIND9.                                         |
| `recursion`              | `yes`                           | Bật truy vấn đệ quy, cho phép xử lý tên miền ngoài zone.                          |
| `allow-recursion`        | `{ 192.168.0.0/24; }`           | Chỉ mạng `192.168.0.0/24` được phép dùng truy vấn đệ quy.                         |
| `listen-on port`         | `port 53 { 192.168.0.100; }`    | BIND9 lắng nghe trên cổng 53, IP `192.168.0.100`.                                 |
| `allow-transfer`         | `{ none; }`                     | Cấm truyền zone, tăng bảo mật.                                                    |
| `allow-query`            | `{ localhost; 192.168.0.0/24; }`| Cho phép truy vấn từ localhost và mạng `192.168.0.0/24`.                          |
| `forwarders`             | `{ 8.8.8.8; 1.1.1.1; }`         | Chuyển tiếp truy vấn ngoài zone đến Google DNS (`8.8.8.8`) và Cloudflare (`1.1.1.1`). |

**Bước 5: Thiết lập vùng**  
Chỉnh sửa tệp `/etc/bind/named.conf.local`:
```bash
sudo nano /etc/bind/named.conf.local
```
Cấu hình:
```plaintext
zone "thienduong.com" {
    type master;
    file "/etc/bind/zones/db.thienduong.com";
    allow-update { none; };
};

zone "0.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/db.192.168.0";
    allow-update { none; };
};
```
![Named Local Config](/Images/config-zones.png)

**Bảng giải thích cấu hình**:

| **Zone**                        | **Tùy chọn**                        | **Mô tả ngắn gọn**                                                                 |
|---------------------------------|-------------------------------------|-----------------------------------------------------------------------------------|
| `zone "thienduong.com"`         | `type master`                       | Zone chính cho `thienduong.com`, máy chủ quản lý toàn quyền.                       |
|                                 | `file "/etc/bind/zones/db.thienduong.com"` | File chứa các bản ghi DNS (`A`, `CNAME`, `MX`, v.v.).                     |
|                                 | `allow-update { none; }`            | Cấm cập nhật động, tăng bảo mật.                                                  |
| `zone "0.168.192.in-addr.arpa"` | `type master`                       | Reverse zone cho mạng `192.168.0.0/24`, ánh xạ IP → tên miền (`PTR`).             |
|                                 | `file "/etc/bind/zones/db.192.168.0"` | File chứa bản ghi `PTR`.                                                  |
|                                 | `allow-update { none; }`            | Cấm cập nhật động, tăng bảo mật.                                                  |

**Bước 6: Tạo file zone**  
Tạo thư mục `/etc/bind/zones`:
```bash
sudo mkdir -p /etc/bind/zones/
```
Sao chép và chỉnh sửa file zone forward:
```bash
sudo cp /etc/bind/db.local /etc/bind/zones/db.thienduong.com
sudo nano /etc/bind/zones/db.thienduong.com
```
Cấu hình file `/etc/bind/zones/db.thienduong.com`:
```plaintext
$TTL    86400
@       IN      SOA     ns1.thienduong.com. admin.thienduong.com. (
                              2025062401 ; Serial
                              3600       ; Refresh
                              1800       ; Retry
                              604800     ; Expire
                              86400      ; Minimum TTL
                        )
@       IN      NS      ns1.thienduong.com.
@       IN      A       192.168.0.100
ns1     IN      A       192.168.0.100
```
![Forward Zone Config](/Images/forward-zone-db-config.png)

**Bảng giải thích cấu hình**:

| **Bản ghi/Tùy chọn**         | **Giá trị**                     | **Mô tả ngắn gọn**                                                                 |
|------------------------------|---------------------------------|-----------------------------------------------------------------------------------|
| `$TTL 86400`                 | `86400` (24 giờ)                | Thời gian sống mặc định cho các bản ghi.                                          |
| `@ IN SOA ...`               | `ns1.thienduong.com. admin.thienduong.com.` | Bản ghi SOA, chỉ định máy chủ DNS và email quản trị.                      |
|                              | `2025062401`                    | **Serial**: Phiên bản zone, tăng khi cập nhật.                                    |
|                              | `3600`, `1800`, `604800`, `86400` | **Refresh**, **Retry**, **Expire**, **Minimum TTL**.                            |
| `@ IN NS ns1.thienduong.com.`| `ns1.thienduong.com.`           | Bản ghi NS, chỉ định máy chủ DNS cho `thienduong.com`.                            |
| `@ IN A 192.168.0.100`       | `192.168.0.100`                 | Bản ghi A, ánh xạ `thienduong.com` thành IP `192.168.0.100`.                      |
| `ns1 IN A 192.168.0.100`     | `192.168.0.100`                 | Bản ghi A, ánh xạ `ns1.thienduong.com` thành IP `192.168.0.100`.                  |

**Ký hiệu @**: Đại diện cho tên miền gốc của zone (`thienduong.com` trong file này).

**Bước 7: Tạo file reverse zone**  
Sao chép và chỉnh sửa file reverse zone:
```bash
sudo cp /etc/bind/db.127 /etc/bind/zones/db.192.168.0
sudo nano /etc/bind/zones/db.192.168.0
```
Cấu hình file `/etc/bind/zones/db.192.168.0`:
```plaintext
$TTL    86400
@       IN      SOA     ns1.thienduong.com. root.thienduong.com. (
                              2024072301 ; Serial
                              3600       ; Refresh
                              1800       ; Retry
                              1209600    ; Expire
                              86400      ; Negative Cache TTL
                        )
@       IN      NS      ns1.thienduong.com.
100     IN      PTR     ns1.thienduong.com.
```
![Reverse Zone Config](/Images/reverse-zone-db-config.png)

**Bảng giải thích cấu hình**:

| **Bản ghi/Tùy chọn**         | **Giá trị**                     | **Mô tả ngắn gọn**                                                                 |
|------------------------------|---------------------------------|-----------------------------------------------------------------------------------|
| `$TTL 86400`                 | `86400` (24 giờ)                | Thời gian sống mặc định cho các bản ghi.                                          |
| `@ IN SOA ...`               | `ns1.thienduong.com. root.thienduong.com.` | Bản ghi SOA, thông tin quản trị cho reverse zone.                         |
|                              | `2024072301`                    | **Serial**: Phiên bản zone.                                                       |
|                              | `3600`, `1800`, `1209600`, `86400` | **Refresh**, **Retry**, **Expire**, **Negative Cache TTL**.                    |
| `@ IN NS ns1.thienduong.com.`| `ns1.thienduong.com.`           | Bản ghi NS, chỉ định máy chủ DNS cho `0.168.192.in-addr.arpa`.                    |
| `100 IN PTR ns1.thienduong.com.` | `ns1.thienduong.com.`       | Bản ghi PTR, ánh xạ `192.168.0.100` thành `ns1.thienduong.com`.                   |

**Bước 8: Kiểm tra cấu hình**  
Kiểm tra cấu hình BIND9:
```bash
sudo named-checkconf
```
Kiểm tra file zone:
```bash
sudo named-checkzone thienduong.com /etc/bind/zones/db.thienduong.com
sudo named-checkzone 0.168.192.in-addr.arpa /etc/bind/zones/db.192.168.0
```
Kết quả nếu không có lỗi: **OK**.  
![Zone Check](/Images/zone-check.png)

---

## 5. Xác Minh Máy Chủ DNS Từ Máy Khách

### Các bước thực hiện

**Bước 1: Xóa và tạo mới `/etc/resolv.conf`**  
Xóa liên kết mặc định và tạo tệp mới:
```bash
sudo unlink /etc/resolv.conf
sudo nano /etc/resolv.conf
```
Thêm nội dung:
```plaintext
nameserver 192.168.0.100
```
![Resolv Conf Client](/Images/resolv-conf-client.png)

**Bước 2: Khóa tệp `/etc/resolv.conf`**  
Ngăn ghi đè bởi **NetworkManager**:
```bash
sudo chattr +i /etc/resolv.conf
```
Khởi động lại **NetworkManager**:
```bash
sudo systemctl restart NetworkManager
```

**Bước 3: Cài đặt tiện ích DNS**  
Cài đặt `dnsutils` và `bind9-utils`:
```bash
sudo apt install dnsutils bind9-utils
```

**Bước 4: Kiểm tra bản ghi DNS**  
- Kiểm tra tên miền:
  ```bash
  dig thienduong.com +short
  dig thienduong.com
  ```
  Kết quả:  
  ![Dig Result](/Images/dig-result.png)

- Kiểm tra reverse zone:
  ```bash
  nslookup 192.168.0.100
  ```
  Kết quả:  
  ![Nslookup Result](/Images/nslookup-result.png)

---
THE END