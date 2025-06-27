# Firewall

## Mục lục
- [1. Giới thiệu về Firewall](#1-giới-thiệu-về-firewall)
- [2. Tìm hiểu về Iptables](#2-tìm-hiểu-về-iptables)
- [3. Tìm hiểu về Fail2ban](#3-tìm-hiểu-về-fail2ban)
  - [Fail2ban](#fail2ban)
  - [Cấu hình Fail2ban](#cấu-hình-fail2ban)
- [4. Tìm hiểu về CSF](#4-tìm-hiểu-về-csf)
  - [CSF (ConfigServer Security & Firewall)](#csf-configserver-security--firewall)
  - [Cấu hình CSF](#cấu-hình-csf)

## 1. Giới thiệu về Firewall

### **Tường lửa (Firewall)**

 là một hệ thống bảo mật có chức năng giám sát và kiểm soát lưu lượng mạng máy tính ra vào dựa trên các quy tắc bảo mật đã được xác định trước. Tường lửa có thể là một thiết bị phần cứng, một phần mềm hoặc kết hợp cả hai, tạo thành một rào cản giữa mạng nội bộ an toàn và mạng bên ngoài không đáng tin cậy (như Internet).

Hãy hình dung tường lửa như một người bảo vệ chuyên nghiệp đứng ở cổng vào hệ thống mạng của bạn. Người bảo vệ này có một danh sách khách mời (bộ quy tắc) và sẽ kiểm tra thông tin của tất cả mọi người muốn đi vào hoặc đi ra. Ai có trong danh sách thì được qua, ai không có hoặc có hành vi đáng ngờ sẽ bị chặn lại ngay lập tức.

![image.png](/Images/tuan_2_firewall/image.png)

### **Tác dụng của tường lửa:**

- Ngăn chặn truy cập trái phép: Đây là chức năng cơ bản nhất. Tường lửa ngăn chặn hacker, botnet và các tác nhân xấu khác cố gắng xâm nhập vào hệ thống mạng của bạn để đánh cắp dữ liệu hoặc phá hoại.
- Bảo vệ khỏi phần mềm độc hại: Mặc dù không phải là một phần mềm diệt virus, tường lửa có thể chặn các kết nối đến những máy chủ độc hại đã biết, ngăn chặn việc tải về các loại mã độc, trojan, hoặc ransomware.
- Kiểm soát luồng dữ liệu: Tường lửa cho phép bạn kiểm soát chặt chẽ các dịch vụ và ứng dụng nào được phép truy cập Internet từ bên trong mạng của bạn, cũng như các kết nối từ bên ngoài vào.
- Đảm bảo an toàn cho dữ liệu và dịch vụ: Bằng cách chỉ mở các cổng cần thiết cho hoạt động (ví dụ cổng 80/443 cho web server), tường lửa giảm thiểu bề mặt tấn công (attack surface), khiến kẻ xấu có ít cơ hội để khai thác lỗ hổng hơn.

### **Ưu điểm**

- **Tuyến phòng thủ vững chắc:** Là lớp bảo vệ đầu tiên và quan trọng nhất, giúp ngăn chặn phần lớn các cuộc tấn công tự động và truy cập trái phép từ Internet.
- **Kiểm soát truy cập chi tiết:** Cho phép quản trị viên định nghĩa các quy tắc rất cụ thể về việc ai được truy cập vào tài nguyên nào, từ đâu và khi nào.
- **Ghi lại nhật ký (Logging):** Hầu hết các tường lửa đều có khả năng ghi lại nhật ký về các kết nối được cho phép và bị từ chối. Dữ liệu này rất quý giá cho việc phân tích bảo mật và điều tra sự cố.
- **Tăng cường chính sách bảo mật:** Giúp thực thi các chính sách bảo mật của tổ chức một cách nhất quán trên toàn bộ hệ thống mạng.

### **Nhược điểm**

- **Không chống lại tấn công nội bộ:** Tường lửa được thiết kế để chống lại các mối đe dọa từ bên ngoài. Chúng thường không hiệu quả trong việc ngăn chặn các cuộc tấn công xuất phát từ bên trong mạng.
- **Cấu hình phức tạp:** Một tường lửa được cấu hình sai có thể gây ra nhiều vấn đề, từ việc chặn nhầm các truy cập hợp lệ cho đến việc tạo ra các lỗ hổng bảo mật nghiêm trọng.
- **Ảnh hưởng đến hiệu năng:** Tường lửa phần mềm có thể tiêu tốn tài nguyên hệ thống. Tường lửa phần cứng hoặc đám mây nếu không đủ mạnh có thể trở thành một điểm nghẽn cổ chai (bottleneck) cho lưu lượng mạng.
- **Bỏ qua lưu lượng mã hóa:** Tường lửa truyền thống không thể kiểm tra nội dung của các lưu lượng đã được mã hóa (như HTTPS), tạo ra một điểm mù mà kẻ xấu có thể lợi dụng.

---
## 2. Tìm hiểu về Iptables

### Iptables

Iptables là một công cụ tường lửa miễn phí trong Linux, cho phép thiết lập các quy tắc riêng để kiểm soát truy cập, tăng tính bảo mật. IPtables có nhiều tùy chỉnh khác nhau, cho phép người dùng đáp ứng nhu cầu cụ thể của mình.

Trong hầu hết các phiên bản thương mại của Linux, IPTables sẽ được cài đặt sẵn. Tuy nhiên, nếu IPTables vẫn chưa được cài đặt thì có thể thực hiện theo lệnh sau:

```
sudo apt install iptables
```

![image.png](/Images/tuan_2_firewall/image%201.png)

Sau khi cài đặt xong iptable, để xem trạng thái của iptables bằng 1 trong 2 lệnh sau:

```
sudo iptables -L -v
sudo iptables-save
```

Trong đó:

- **L :** đây là lệnh dùng để liệt kê tất cả các rule – quy tắc đã được đặt.
- **v** : được sử dụng để hiện thêm danh sách bổ trợ.

![image.png](/Images/tuan_2_firewall/image%202.png)

**Các bảng trong Iptables:**

- **FILTER:** đây là bảng mặc định được sử dụng để lọc các gói và bao gồm các chuỗi như: INPUT, OUTPUT và FORWARD.
- **NAT:** bản có liên quan đến việc dịch địa chỉ mạng.
- **MANGLE:** bản được sử dụng để thay đổi các gói chuyên biệt
- **RAW:** dùng để cấu hình không theo dõi các kết nối
- **SECURITY**: thông thường, SELinux sử dụng để thiết lập chính sách bảo mật.

Các Target thường sử dụng:

| **Target** | **Ý nghĩa** | **Tùy chọn** |
| --- | --- | --- |
| `ACCEPT` | Dừng xử lý gói dữ liệu, chuyển tiếp đến ứng dụng cuối hoặc hệ điều hành để xử lý. | *Không có tùy chọn đặc biệt* |
| `DROP` | Gói dữ liệu bị loại bỏ, không được xử lý hay thông báo cho phía gửi. | *Không có tùy chọn đặc biệt* |
| `LOG` | Ghi thông tin gói vào `syslog` để kiểm tra. Iptables tiếp tục xử lý với quy tắc tiếp theo. | `--log-prefix "string"`: Thêm chuỗi định nghĩa vào thông điệp log. Thường dùng để chỉ lý do gói bị loại. |
| `REJECT` | Tương tự `DROP`, nhưng có gửi lại thông báo lỗi cho phía gửi. | `--reject-with <qualifier>`: Chỉ định loại thông báo trả về. Một số loại phổ biến: `icmp-port-unreachable` *(mặc định)*, `icmp-net-unreachable`, `icmp-host-unreachable`, `icmp-proto-unreachable`, `icmp-net-prohibited`, `icmp-host-prohibited`, `tcp-reset`, `echo-reply`. |
|  |  |  |

### **Cấu hình iptables**:

---

### **Thêm rule mới vào iptable**

```
sudo iptables -A <CHAIN> -p <PROTOCOL> [--dport <PORT>] -j <TARGET>
```

Trong đó:

- `-A <CHAIN>` → thêm (Append) rule vào chuỗi (`INPUT`, `OUTPUT`, `FORWARD`, v.v.)
- `-p <PROTOCOL>` → giao thức: `tcp`, `udp`, `icmp`, `all`,...
- `--dport <PORT>` → cổng đích (ví dụ: 22 cho SSH, 80 cho HTTP)
- `-j <TARGET>` → hành động : `ACCEPT`, `DROP`, `REJECT`, `LOG`, `DNAT`, `SNAT`,…

**Ví dụ:** 

***Cho phép kết nối SSH (port 22):*** mở cổng SSH để cho phép người khác truy cập vào server từ xa:

```
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

![image.png](/Images/tuan_2_firewall/image%203.png)

***Cho phép truy cập HTTP (port 80):*** cho phép người dùng truy cập website chạy trên máy bạn:

```
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

![image.png](/Images/tuan_2_firewall/image%204.png)

***Cho phép gửi mail (SMTP - port 25):*** Cho phép máy gửi email ra ngoài (dùng trong mail server):

```
sudo iptables -A OUTPUT -p tcp --dport 25 -j ACCEPT
```

![image.png](/Images/tuan_2_firewall/image%205.png)

---

### Bổ sung Rule

```
sudo iptables -I <CHAIN> <LINE_NUMBER> -p <PROTOCOL> [--dport <PORT>] -j <TARGET>
```

Trong đó:

- `I <CHAIN> <LINE_NUMBER>` → **Insert**: chèn rule vào chuỗi tại dòng số `<LINE_NUMBER>` (nếu bỏ số dòng thì mặc định là dòng 1).
- `p <PROTOCOL>` → Giao thức (`tcp`, `udp`, `icmp`, `all`,...).
- `-dport <PORT>` → Cổng đích cần áp dụng rule (ví dụ: `22`, `80`, `443`).
- `j <TARGET>` → Hành động khi rule khớp (`ACCEPT`, `DROP`, `REJECT`,...).

**Ví dụ:**
***Chặn FTP (port 21) ngay từ đầu chain `INPUT`***:

```
sudo iptables -I INPUT 1 -p tcp --dport 21 -j DROP
```

Đây là trước khi chèn lệnh chặn ở dòng 1:

![image.png](/Images/tuan_2_firewall/image%206.png)

Sau khi chèn theo lệnh trên thì:

![image.png](/Images/tuan_2_firewall/image%207.png)

---

### Xóa rule đã thêm vào iptables

**Cách 1: Xóa theo điều kiện cụ thể (giống với rule đã thêm):**

```
sudo iptables -D <CHAIN> -p <PROTOCOL> [--dport <PORT>] -j <TARGET>
```

Trong đó:

- `D <CHAIN>` → Xóa rule khỏi chuỗi (`INPUT`, `OUTPUT`, `FORWARD`,...).
- `p <PROTOCOL>` → Giao thức: `tcp`, `udp`, `icmp`, `all`,...
- `-dport <PORT>` → Cổng đích cần áp dụng (ví dụ: 22, 80,...).
- `j <TARGET>` → Hành động của rule: `ACCEPT`, `DROP`, `REJECT`,...

**Cách 2: Xóa theo số dòng trong chuỗi:**

Cần dùng thêm lệnh liệt kê để biết dòng nào chứa rule cần xóa:

```
sudo iptables -L <CHAIN> -n --line-numbers
```

```
sudo iptables -D <CHAIN> <LINE_NUMBER>
```

**Trong đó:**

- `D <CHAIN>` → Xác định chuỗi chứa rule cần xóa (`INPUT`, `OUTPUT`,...).
- `L <CHAIN>` → Liệt kê các rule hiện có
- `<LINE_NUMBER>` → Số thứ tự của rule cần xóa trong chuỗi.

**Ví dụ:** ở trường hợp này chúng ta sẽ xóa 1 lệnh chặn FTP (port 21)

Đây là trước khi chúng ta xóa lệnh chặn:

![image.png](/Images/tuan_2_firewall/image%208.png)

Sau khi xóa bằng cách 2 (xóa theo số dòng trong chuỗi):

![image.png](/Images/tuan_2_firewall/image%209.png)

---

## 3. Tìm hiểu về Fail2ban

### Fail2ban

Fail2ban là một công cụ bảo mật mã nguồn mở giúp bảo vệ hệ thống khỏi các cuộc tấn công brute-force (thử mật khẩu liên tục). Công cụ này hoạt động bằng cách giám sát các tệp nhật ký (log files) để phát hiện các dấu hiệu của những cuộc tấn công không mong muốn. Khi phát hiện có một địa chỉ IP thực hiện các hành vi đáng ngờ, Fail2ban sẽ thực hiện các biện pháp phòng ngừa như chặn IP đó bằng tường lửa (firewall), qua đó giảm thiểu nguy cơ bị xâm nhập.

Fail2ban hoạt động với hầu hết các dịch vụ kết nối từ xa như SSH, FTP, SMTP và HTTP.

Cài đặt fail2ban trên Ubuntu bằng lệnh sau:

```
sudo apt install fail2ban 
```

![image.png](/Images/tuan_2_firewall/image%2010.png)

Sau khi cài đặt xong tiến hành chạy và kiểm tra trạng thái:

![image.png](/Images/tuan_2_firewall/image%2011.png)

Nếu xãy ra lỗi hoặc có gì bất thường thì có thể kiêm tra bằng lệnh:

```
 sudo journalctl -u fail2ban 
```

![image.png](/Images/tuan_2_firewall/image%2012.png)

### Cấu hình Fail2ban

Ta có thể bắt đầu cấu hình Fail2ban để bảo vệ hệ thống khỏi các cuộc tấn công brute force  và những hoạt động đáng ngờ khác.

File cấu hình chính của Fail2ban nằm ở đường dẫn:`/etc/fail2ban/jail.conf`.

**Cách tốt nhất là không chỉnh sửa trực tiếp file này.** Hãy chỉ dùng nó như tài liệu tham khảo.

Mỗi khi bạn cập nhật Fail2ban, file `jail.conf` sẽ bị ghi đè. Để tránh mất cấu hình tùy chỉnh, hãy tạo một bản sao tên là `jail.local` trong cùng thư mục. Sử dụng lệnh `cp` như sau:

```
cd /etc/fail2ban && sudo cp jail.conf jail.local
```

![image.png](/Images/tuan_2_firewall/image%2013.png)

Tiếp theo, mở file `jail.local` bằng trình soạn thảo bạn yêu thích và điều chỉnh các thông số sau:

![image.png](/Images/tuan_2_firewall/image%2014.png)

- **ignoreip**: Dùng để chỉ định danh sách địa chỉ IP **không bị áp dụng** luật của Fail2ban (ví dụ: IP của quản trị viên).
- **bantime**: Thời gian một địa chỉ IP bị chặn sau khi vi phạm (ví dụ do đăng nhập sai nhiều lần). Đặt giá trị này thành **5 phút (5m)**.
- **maxretry**: Số lần đăng nhập sai cho phép trước khi chặn địa chỉ đó. Để thử nghiệm, bạn có thể đặt là **2**.

Cuối cùng, khởi động lại dịch vụ fail2ban để các thay đổi có hiệu lực. Sử dụng lệnh:

```
sudo systemctl restart fail2ban
```

---

## 4. Tìm hiểu về CSF

### **CSF (ConfigServer Security & Firewall)**

CSF là một ứng dụng firewall mã nguồn mở phổ biến được sử dụng để bảo mật server. CSF cung cấp giao diện quản lý firewall dựa trên iptables, giúp dễ dàng cấu hình các quy tắc bảo vệ server khỏi các cuộc tấn công như DDoS, brute-force, hoặc truy cập trái phép. Ngoài ra, CSF còn tích hợp các tính năng như phát hiện đăng nhập sai (Login Failure Daemon – LFD), bảo vệ email, và chặn IP độc hại. CSF thường được sử dụng trên các hệ điều hành Linux, đặc biệt là các server chạy CentOS hoặc Ubuntu.

Để cài đặt csf thực hiện các bước sau:

Bước 1: Tải file nén từ  Configuration Server website

Tường lửa **CSF** hiện không có sẵn trong kho Debian hoặc Ubuntu và phải được tải xuống từ trang web chính chủ .

```
wget [http://download.configserver.com/csf.tgz](http://download.configserver.com/csf.tgz)
```

![image.png](/Images/tuan_2_firewall/image%2015.png)

Bước 2: Giải nén thư mục vừa tải và kiểm tra:

```
tar -xzf csf.tgz
ls
```

![image.png](/Images/tuan_2_firewall/image%2016.png)

Bước 3: Cài đặt CSF:

Đi vào thư mục `/csf` và cài đặt bằng lệnh:

```
sudo sh install.sh
```

![image.png](/Images/tuan_2_firewall/image%2017.png)

![image.png](/Images/tuan_2_firewall/image%2018.png)

Bước 4: Kiểm tra khả năng tương thích của CSF với kernel và các module Netfilter

![image.png](/Images/tuan_2_firewall/image%2019.png)

## Cấu hình CSF

CSF có thể được định cấu hình bằng cách chỉnh sửa tệp cấu hình `csf.conf` trong `/etc/csf`:

```
nano /etc/csf/csf.conf
```

Và muốn lưu lại cấu hình thì dùng lệnh:

```
csf -r
```

Tắt chế độ kiểm tra *(Testing mode)*: Tìm dòng `TESTING = "1"` và đổi thành `TESTING = "0"` để kích hoạt tường lửa.

![image.png](/Images/tuan_2_firewall/image%2020.png)

Tìm dòng `TCP_IN` và thêm các cổng cần mở, ví dụ:

![image.png](/Images/tuan_2_firewall/image%2021.png)

Tương tự, cấu hình `TCP_OUT` và `UDP_IN`/`UDP_OUT` nếu cần:

![image.png](/Images/tuan_2_firewall/image%2022.png)

**Bảo vệ chống brute-force**: Tìm dòng `LF_SSHD` và đặt giá trị `LF_SSHD = "5"` để khóa IP sau 5 lần đăng nhập SSH thất bại.

![image.png](/Images/tuan_2_firewall/image%2023.png)

**Tối ưu hóa bảo mật**:: Kích hoạt `SYNFLOOD` trong `/etc/csf/csf.conf` để chống DDoS attack:

![image.png](/Images/tuan_2_firewall/image%2024.png)

Sau khi cấu hình xong thì lưu và thoát.

Để áp dụng nhưng gì đã thay đổi cở file cấu hình thi thực hiện lệnh:

```
csf -r
```

Kiểm tra trạng thái tường lửa bằng:

```
csf -l
```

![image.png](/Images/tuan_2_firewall/image%2025.png)

Tới đây là hoàn thành cấu hình cơ bản CSF

---

**THE END**