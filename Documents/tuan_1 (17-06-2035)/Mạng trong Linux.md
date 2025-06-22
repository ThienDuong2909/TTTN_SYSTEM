# Hướng Dẫn Quản Lý và Cấu Hình Mạng Trong Linux

## Mục Lục
- [1. Kiểm Tra và Cấu Hình Địa Chỉ Mạng Với `ifconfig`](#1-kiểm-tra-và-cấu-hình-địa-chỉ-mạng-với-ifconfig)
- [2. Kiểm Tra và Giám Sát Kết Nối Mạng](#2-kiểm-tra-và-giám-sát-kết-nối-mạng)
  - [2.1 Lệnh `ping`](#21-lệnh-ping)
  - [2.2 Lệnh `traceroute`](#22-lệnh-traceroute)
  - [2.3 Lệnh `netstat`](#23-lệnh-netstat)
  - [2.4 Lệnh `ss`](#24-lệnh-ss)
- [3. Phân Tích Gói Tin Với `tcpdump`](#3-phân-tích-gói-tin-với-tcpdump)
- [4. Lab: Cấu Hình IP Tĩnh Trên Máy Ảo (Debian/Ubuntu)](#4-lab-cấu-hình-ip-tĩnh-trên-máy-ảo-debianubuntu)
- [5. Tìm Hiểu Về NAT (Network Address Translation)](#5-tìm-hiểu-về-nat-network-address-translation)
- [6. Cấu Hình DNS Client Trong `/etc/resolv.conf`](#6-cấu-hình-dns-client-trong-etcresolvconf)

---

## 1. Kiểm Tra và Cấu Hình Địa Chỉ Mạng Với `ifconfig`

**Mục đích**: Hiển thị và cấu hình thông tin giao diện mạng (*interface*) như địa chỉ IP, subnet mask, và trạng thái.

**Cấu trúc tổng quát**:
```bash
ifconfig [interface] [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**                     | **Mô tả**                                                   |
|----------------------------------|-------------------------------------------------------------|
| `<interface> up`                 | Kích hoạt giao diện mạng.                                   |
| `<interface> down`               | Vô hiệu hóa giao diện mạng.                                 |
| `<interface> <IP> netmask <mask>`| Gán địa chỉ IP và subnet mask cho giao diện.                |
| `-a`                             | Hiển thị tất cả giao diện, kể cả giao diện không hoạt động. |

**Ví dụ**:
- Hiển thị thông tin tất cả giao diện:
  ```bash
  ifconfig -a
  ```
- Gán địa chỉ IP `192.168.1.100` và subnet mask `255.255.255.0` cho giao diện `eth0`:
  ```bash
  sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0
  ```
- Kích hoạt giao diện `eth0`:
  ```bash
  sudo ifconfig eth0 up
  ```
![IP config result](/Images/ifconfig-up-gw.png)  
![IP config result](/Images/ifconfig-enp0s3.png)  



**Lưu ý**: `ifconfig` đã bị thay thế bởi lệnh `ip` trong nhiều bản phân phối Linux hiện đại. Cần cài đặt gói `net-tools` để sử dụng `ifconfig`:
```bash
sudo apt install net-tools
```

---

## 2. Kiểm Tra và Giám Sát Kết Nối Mạng

### 2.1 Lệnh `ping`

**Mục đích**: Kiểm tra kết nối đến một máy chủ bằng cách gửi gói ICMP.

**Cấu trúc tổng quát**:
```bash
ping [tùy chọn] <đích>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `-c <số>`      | Giới hạn số gói tin gửi đi.                                 |
| `-i <giây>`     | Khoảng thời gian giữa các gói tin.                          |
| `-t <TTL>`      | Đặt giá trị Time to Live.                                   |

**Ví dụ**:
```bash
ping -c 4 google.com

```

![IP config result](/Images/ping-gg.png) 
Lệnh trên gửi 4 gói tin đến `google.com` và hiển thị kết quả.

### 2.2 Lệnh `traceroute`

**Mục đích**: Theo dõi đường đi của gói tin đến đích.

**Cấu trúc tổng quát**:
```bash
traceroute [tùy chọn] <đích>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `-n`            | Không phân giải tên miền, chỉ hiển thị IP.                  |
| `-m <hops>`     | Giới hạn số bước nhảy tối đa.                               |
| `-w <giây>`     | Thời gian chờ cho mỗi bước nhảy.                            |

**Ví dụ**:
```bash
traceroute -I google.com
```
![IP config result](/Images/traceroute.png) 
**Lưu ý**: Cần cài đặt gói `traceroute`:
```bash
sudo apt install traceroute
```

### 2.3 Lệnh `netstat`

**Mục đích**: Hiển thị thông tin kết nối mạng, bảng định tuyến, và thống kê giao diện.

**Cấu trúc tổng quát**:
```bash
netstat [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `-t`            | Hiển thị kết nối TCP.                                       |
| `-u`            | Hiển thị kết nối UDP.                                       |
| `-l`            | Hiển thị các cổng đang lắng nghe.                           |
| `-r`            | Hiển thị bảng định tuyến.                                   |
| `-p`            | Hiển thị PID và tên chương trình.                           |

**Ví dụ**:
```bash
netstat -tulnp
```
Lệnh trên hiển thị các cổng TCP/UDP đang lắng nghe cùng với PID.

**Lưu ý**: `netstat` thuộc gói `net-tools`, có thể cần cài đặt:
```bash
sudo apt install net-tools
```

### 2.4 Lệnh `ss`

**Mục đích**: Thay thế `netstat`, cung cấp thông tin kết nối mạng nhanh và chi tiết hơn.

**Cấu trúc tổng quát**:
```bash
ss [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `-t`            | Hiển thị kết nối TCP.                                       |
| `-u`            | Hiển thị kết nối UDP.                                       |
| `-l`            | Hiển thị các cổng đang lắng nghe.                           |
| `-p`            | Hiển thị thông tin tiến trình.                              |
| `-n`            | Không phân giải tên miền.                                   |

**Ví dụ**:
```bash
ss -tulnp
```

---

## 3. Phân Tích Gói Tin Với `tcpdump`

**Mục đích**: Bắt và phân tích các gói tin mạng trên một giao diện.

**Cấu trúc tổng quát**:
```bash
sudo tcpdump [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                   |
|---------------------|-------------------------------------------------------------|
| `-i <interface>`    | Chỉ định giao diện mạng (ví dụ: `eth0`).                    |
| `-c <số>`           | Giới hạn số gói tin bắt được.                               |
| `-w <tệp>`          | Lưu gói tin vào tệp để phân tích sau.                       |
| `port <cổng>`       | Lọc gói tin theo cổng (ví dụ: `port 80`).                   |
| `host <IP>`         | Lọc gói tin theo địa chỉ IP.                                |

**Ví dụ**:
- Bắt 10 gói tin trên giao diện `eth0`:
  ```bash
  sudo tcpdump -i eth0 -c 10
  ```
- Lưu gói tin từ cổng 80 vào tệp `capture.pcap`:
  ```bash
  sudo tcpdump -i eth0 port 80 -w capture.pcap
  ```

**Lưu ý**: Cần cài đặt `tcpdump` và quyền quản trị:
```bash
sudo apt install tcpdump
```

---

## 4. Lab: Cấu Hình IP Tĩnh Trên Máy Ảo (Debian/Ubuntu)

**Mục đích**: Cấu hình địa chỉ IP tĩnh cho một giao diện mạng trên máy ảo.

**Các bước thực hiện**:

**Bước 1: Kiểm tra giao diện mạng**:
```bash
ip link show
```
Giả sử giao diện là `eth0`.

**Bước 2: Chỉnh sửa tệp cấu hình mạng**:
- Trên Debian/Ubuntu (sử dụng `netplan`):
  ```bash
  sudo nano /etc/netplan/01-netcfg.yaml
  ```
  Thêm nội dung sau:
  ```yaml
  network:
    version: 2
    ethernets:
      eth0:
        addresses:
          - 192.168.1.100/24
        gateway4: 192.168.1.1
        nameservers:
          addresses: [8.8.8.8, 8.8.4.4]
  ```

**Bước 3: Áp dụng cấu hình**:
```bash
sudo netplan apply
```

**Bước 4: Kiểm tra cấu hình**:
```bash
ip addr show eth0
ping 8.8.8.8
```

**Lưu ý**:
- Tệp cấu hình có thể khác nhau tùy bản phân phối (ví dụ: `/etc/network/interfaces` trên Debian cũ).
- Sao lưu tệp cấu hình trước khi chỉnh sửa.

---

## 5. Tìm Hiểu Về NAT (Network Address Translation)

**Mục đích**: Chuyển tiếp kết nối từ mạng nội bộ (*private*) ra mạng bên ngoài (*public*) hoặc giữa các mạng.

**Khái niệm**: NAT thay đổi địa chỉ nguồn/đích của gói tin để kết nối mạng nội bộ với mạng công cộng, thường được sử dụng trong firewall hoặc router.

**Cấu hình NAT cơ bản với `iptables`**:

**Bước 1: Kích hoạt chuyển tiếp gói tin**:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```
Để lưu vĩnh viễn, chỉnh sửa `/etc/sysctl.conf`:
```bash
net.ipv4.ip_forward=1
```

**Bước 2: Thêm quy tắc NAT**:
```bash
sudo iptables -t nat -A POSTROUTING -o <interface_ra> -j MASQUERADE
```
Ví dụ: Nếu giao diện ra là `eth0`:
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

**Bước 3: Kiểm tra quy tắc**:
```bash
sudo iptables -t nat -L -v
```

**Ví dụ ứng dụng**: Máy chủ Linux (có 2 giao diện: `eth0` nối Internet, `eth1` nối mạng nội bộ `192.168.1.0/24`) chuyển tiếp lưu lượng từ mạng nội bộ ra Internet.

**Lưu ý**: Cần cài đặt `iptables` và quyền quản trị. Trên các hệ thống mới, `nftables` có thể thay thế `iptables`.

---

## 6. Cấu Hình DNS Client Trong `/etc/resolv.conf`

**Mục đích**: Cấu hình máy khách DNS để phân giải tên miền.

**Tệp cấu hình**: `/etc/resolv.conf`

**Cấu trúc tệp**:
```plaintext
nameserver <địa_chỉ_IP_DNS>
search <tên_miền>
```

**Ví dụ**:
- Chỉnh sửa tệp:
  ```bash
  sudo nano /etc/resolv.conf
  ```
  Thêm nội dung:
  ```plaintext
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  search example.com
  ```

**Kiểm tra phân giải DNS**:
```bash
nslookup google.com
```

**Lưu ý**:
- `/etc/resolv.conf` có thể bị ghi đè bởi các dịch vụ như `systemd-resolved` hoặc `NetworkManager`.
- Để cấu hình vĩnh viễn, chỉnh sửa tệp cấu hình của `netplan` hoặc `NetworkManager`.

---
THE END