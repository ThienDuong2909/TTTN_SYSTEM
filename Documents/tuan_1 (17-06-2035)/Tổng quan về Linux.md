# Tổng Quan Về Linux

## Mục Lục

  - [1. Linux Là Gì](#1-linux-là-gì)
  - [2. Lịch Sử Hình Thành và Phát Triển](#2-lịch-sử-hình-thành-và-phát-triển)
  - [3. Ưu và Nhược Điểm của Linux](#3-ưu-và-nhược-điểm-của-linux)
    - [3.1 Ưu Điểm](#31-ưu-điểm)
    - [3.2 Nhược Điểm](#32-nhược-điểm)
  - [4. Ứng Dụng Linux Trong Các Lĩnh Vực](#4-ứng-dụng-linux-trong-các-lĩnh-vực)
---

## 1. Linux Là Gì
- **Linux** là một **hệ điều hành mã nguồn mở** (*open-source operating system*), được **Linus Torvalds** tạo ra vào năm **1991**. Nhờ sự đóng góp của cộng đồng, Linux đã trở thành một trong những hệ điều hành phổ biến nhất hiện nay.
- Về bản chất, Linux chỉ là **hạt nhân** (*kernel*) của hệ điều hành. Khi nói đến “hệ điều hành Linux”, người ta thường ám chỉ các **bản phân phối Linux** (*Linux distributions* hay *distros*), như Ubuntu, Fedora, hay Red Hat.

---

## 2. Lịch Sử Hình Thành và Phát Triển
- **Ngày 5/4/1991**: **Linus Torvalds**, một sinh viên 21 tuổi tại Đại học Helsinki, Phần Lan, bắt đầu viết những dòng lệnh đầu tiên cho Linux.
- **Tháng 8/1991**: Torvalds công bố trên mạng về dự án của mình với thông điệp nổi tiếng: *“Tôi đang làm một hệ điều hành miễn phí (chỉ là sở thích, không lớn và chuyên nghiệp)”*.
- **Tháng 9/1991**: Phiên bản **Linux 0.01** ra mắt với **10.239 dòng lệnh**, tiếp theo là phiên bản 0.02 một tháng sau.
- **Năm 1992**: Torvalds phát hành Linux dưới **giấy phép GPL** (*General Public License*), cho phép mọi người tải mã nguồn, chỉnh sửa và đóng góp. Đây là quyết định then chốt giúp Linux phổ biến.
- **Năm 1993**: **Slackware**, bản phân phối Linux đầu tiên, được ra đời. Đây là một trong những distro lâu đời nhất, với phiên bản mới nhất vào **tháng 5/2010**.
![Slackware_linux](/Images/slackware_linux.png)
- **Ngày 14/3/1994**: Sau 3 năm phát triển, **Linux 1.0** ra mắt với **176.250 dòng lệnh**. Một năm sau, phiên bản 1.2 có **310.950 dòng lệnh**.
- **Ngày 3/11/1994**: **Red Hat Linux 1.0**, một trong những distro thương mại hóa đầu tiên, được giới thiệu.
- **Năm 1996**: Linus Torvalds chọn hình ảnh **chim cánh cụt** (*Tux*) làm biểu tượng chính thức của Linux sau chuyến thăm công viên hải dương học.
- **Năm 1998**: Các tập đoàn lớn như **IBM** bắt đầu đầu tư vào Linux, với hàng tỷ USD và đội ngũ hơn **300 nhân viên** phát triển phần mềm/dịch vụ.
- **Năm 2005**: Linus Torvalds xuất hiện trên bìa tạp chí **BusinessWeek**, kể câu chuyện thành công của Linux.
- **Năm 2007**: Các hãng như **HP**, **ASUS**, **Dell**, **Lenovo** bán laptop cài sẵn Linux.
- **Tháng 1/2009**: Linux đạt mốc **10 triệu người dùng** toàn cầu.
- **Ngày 14/3/2011**: Phiên bản **Linux 2.6.38** ra mắt với **14.294.493 dòng lệnh**, đánh dấu 20 năm phát triển.
- **Hiện nay**: Linux có nhiều biến thể, nổi bật là **Android** của Google. Linux được dùng rộng rãi trên **máy chủ**, **thiết bị di động**, **ATM**, **siêu máy tính**. Linux là biểu tượng của **cộng đồng mã nguồn mở**, đối trọng với **Windows** của Microsoft.

---

## 3. Ưu và Nhược Điểm của Linux

### 3.1 Ưu Điểm
- **Mã nguồn mở**: Miễn phí, cho phép xem, chỉnh sửa, phân phối mã nguồn, hỗ trợ cộng đồng cải tiến liên tục.
- **Bảo mật cao**: Ít bị virus/phần mềm độc hại nhờ hệ thống **phân quyền chặt chẽ** và công cụ như **SELinux**, **AppArmor**.
- **Ổn định, đáng tin cậy**: Chạy lâu dài không cần khởi động lại, lý tưởng cho **máy chủ** và hệ thống yêu cầu cao.
- **Tùy biến linh hoạt**: Tùy chỉnh giao diện, cấu hình theo nhu cầu, phù hợp nhiều thiết bị.
- **Cộng đồng hỗ trợ mạnh**: Nhiều diễn đàn, tài liệu miễn phí giúp giải quyết vấn đề nhanh chóng.

### 3.2 Nhược Điểm
- **Khó dùng cho người mới**: Yêu cầu kiến thức cơ bản, đặc biệt khi dùng **dòng lệnh** hoặc cấu hình hệ thống.
- **Hạn chế phần mềm**: Thiếu một số phần mềm thương mại như **Adobe Photoshop**, **Microsoft Office**.
- **Tương thích phần cứng**: Một số thiết bị mới có thể thiếu **driver** hoặc phần mềm hỗ trợ.
- **Cập nhật phức tạp**: Bản cập nhật có thể gây xung đột với phần mềm/phần cứng hiện tại.
- **Hỗ trợ game hạn chế**: Ít game phát triển cho Linux, không lý tưởng cho **game thủ**.

---

## 4. Ứng Dụng Linux Trong Các Lĩnh Vực

### 4.1 Máy Chủ và Đám Mây
- Linux **thống trị** máy chủ và dịch vụ đám mây (**AWS**, **Google Cloud**, **Azure**) nhờ **ổn định**, **tiết kiệm chi phí**, dễ mở rộng.
- **Ứng dụng**: Máy chủ web, cơ sở dữ liệu, VPS, dịch vụ đám mây.

### 4.2 Thiết Bị Nhúng và IoT
- Linux phù hợp cho **thiết bị nhúng** và **IoT** (router, camera, nhà thông minh) nhờ **tối ưu hóa phần cứng hạn chế** và **tùy biến cao**.
- **Ứng dụng**: Thiết bị gia đình thông minh, giám sát, tự động hóa công nghiệp, y tế thông minh.

### 4.3 Phát Triển Phần Mềm
- Linux hỗ trợ **lập trình viên** với công cụ dòng lệnh, **IDE**, và đa dạng ngôn ngữ (**Python**, **C++**, **Java**).
- **Ứng dụng**: Phát triển ứng dụng web, phần mềm hệ thống, lập trình di động.

### 4.4 Máy Tính Cá Nhân
- Các distro như **Ubuntu**, **Mint**, **Fedora** cung cấp giao diện thân thiện, **miễn phí**, **bảo mật cao**.
- **Ứng dụng**: Lướt web, văn phòng, xem phim, chơi game (hạn chế).

### 4.5 Máy Tính Siêu Cấp và HPC
- Linux tối ưu cho **siêu máy tính** (**NASA**) và tính toán hiệu năng cao nhờ **xử lý đa nhiệm**, **tính toán song song**.
- **Ứng dụng**: Nghiên cứu khoa học, mô phỏng vật lý, phân tích dữ liệu lớn.

### 4.6 Giáo Dục
- Linux phổ biến trong **trường học** nhờ **chi phí thấp**, **tùy chỉnh dễ**, hỗ trợ học **lập trình** và công nghệ.
- **Ứng dụng**: Dạy lập trình, khoa học máy tính, mạng, bảo mật.

### 4.7 Công Nghệ Ô Tô
- Linux được dùng trong **xe tự lái** và hệ thống ô tô nhờ **tùy biến**, tích hợp tốt với công nghệ hiện đại.
- **Ứng dụng**: Hệ thống điều khiển, giải trí, cảm biến, an toàn thông minh.