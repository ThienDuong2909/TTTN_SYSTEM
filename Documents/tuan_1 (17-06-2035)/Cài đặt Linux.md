# Cài Đặt Linux
---

## Mục Lục
  - [1. Cài Đặt Trực Tiếp Trên Máy Tính (Dual Boot hoặc Thay Thế Windows)](#1-cài-đặt-trực-tiếp-trên-máy-tính-dual-boot-hoặc-thay-thế-windows)
    - [1.1 Chuẩn Bị](#11-chuẩn-bị)
    - [1.2 Các Bước Cài Đặt](#12-các-bước-cài-đặt)
  - [2. Cài Đặt Linux Trên Máy Ảo (Virtual Machine)](#2-cài-đặt-linux-trên-máy-ảo-virtual-machine)
    - [2.1 Chuẩn Bị](#21-chuẩn-bị)
    - [2.2 Các Bước Cài Đặt](#22-các-bước-cài-đặt)
  - [Lưu Ý Khi Sử Dụng File .md](#lưu-ý-khi-sử-dụng-file-md)

---

## 1. Cài Đặt Trực Tiếp Trên Máy Tính (Dual Boot hoặc Thay Thế Windows)

### 1.1 Chuẩn Bị
Để cài đặt **Ubuntu 20.04** theo phương pháp **Dual Boot** hoặc thay thế Windows, bạn cần:
1. Một máy tính cài sẵn **Windows 10** trở lên.
2. **USB** dung lượng tối thiểu **4GB**, không chứa dữ liệu quan trọng.
3. Kết nối **Internet** ổn định.

### 1.2 Các Bước Cài Đặt

**Bước 1: Sao Lưu Hệ Điều Hành Windows (Tùy Chọn)**  
- **Khuyến nghị**: Sao lưu **hệ điều hành Windows** và dữ liệu quan trọng để tránh rủi ro khi xử lý phân vùng đĩa.  
- Sao chép dữ liệu quan trọng lên **USB** hoặc **ổ cứng ngoài**.

**Bước 2: Tải File ISO Ubuntu**  
- Tải **Ubuntu 20.04 Desktop Image** từ trang chính thức: [releases.ubuntu.com/20.04](https://releases.ubuntu.com/20.04/).  

![Ubuntu 20.04 ISO](/Images/ubuntu_20.04.png)

**Bước 3: Tạo USB Boot Ubuntu**  
- Tải công cụ **Rufus** để tạo USB boot: [rufus.ie/en](https://rufus.ie/en/).  

![Rufus Interface](/Images/rufus-interface.png)  

- Cắm **USB** vào máy tính, đảm bảo không còn dữ liệu quan trọng.  
- Mở **Rufus**, chọn USB và file **ISO Ubuntu 20.04** đã tải.  
- Thiết lập thông số như hình dưới:  

![Rufus Settings](/Images/rufus-settings.png)  

- Nhấn **Start** và chờ quá trình tạo USB boot hoàn tất.

**Bước 4: Tạo Phân Vùng Trống Cho Ubuntu**  
- Mở **Disk Management** trên Windows:  
  - Tìm kiếm từ khóa **"disk partitions"** và chọn **Create and format hard disk partitions**.  

![Disk Management](/Images/disk-management.png)  

- Chuột phải vào ổ đĩa (ví dụ: **ổ C** chứa Windows), chọn **Shrink Volume**.  
- Thu nhỏ phân vùng để tạo **khoảng trống** cho Linux (ví dụ: 25GB trở lên).  

![Shrink Volume](/Images/shrink-volume.png)

**Bước 5: Khởi Động Từ USB Boot**  
- Khởi động lại máy tính, vào **Boot Menu** bằng cách nhấn **F2**, **F10**, hoặc **F12** (tùy mainboard).  
- Chọn **USB cài đặt Ubuntu** từ menu khởi động.  

![Ubuntu Boot Screen](/Images/ubuntu-boot-screen.png)  

- Chọn **Install Ubuntu** để bắt đầu quá trình cài đặt.

**Bước 6: Cài Đặt Ubuntu Cùng Windows**  
- Chọn **ngôn ngữ** và **layout bàn phím**:  

![Ubuntu Language Selection](/Images/ubuntu-language-selection.png)  

- Chọn **Normal installation** (không cần chọn các tùy chọn khác trong **Other options**). 
 
![Create Root Partition](/Images/create-root-partition.png) 
- Nếu thấy tùy chọn **"Install Ubuntu alongside Windows Boot Manager"**, chọn và nhấn **Continue**.

![Create Swap Partition](/Images/install-type.png)  
- Màn hình tiếp theo sẽ cung cấp cho bạn tùy chọn tạo phân vùng cho Ubuntu bằng cách kéo dải phân cách giờ bạn có thể chia không gian đĩa cho HĐH Linux tại đây. 

![Create Swap Partition](/Images/along-side-manager.png)  
**Chú ý** :Nếu Bạn không thấy tùy chọn '**Install Ubuntu alongside Windows Boot Manager"** hoặc nó bị mờ đi, bạn không cần phải lo, bạn vẫn có thể tiếp tục quá trình cài đặt

- Ở màn hình cài đặt, bạn chọn **Something Else**

![Create Home Partition](/Images/install_type_else.png)  

- Nó sẽ đưa bạn đến màn hình để chia phân vùng

- Bạn chọn **free space** và nhấn vào dấu +

![Free space](/Images/free_space.png)  

- Nó sẽ cung cấp tùy chọn tạo phân vùng Linux, lúc này bạn sẽ tạo phân vùng root, tầm 25GB là vừa đủ cho nó. Chọn Size, rồi chọn Ext 4 như hình dưới.


![Create Home Partition](/Images/create-partition.png)  

- Bấm vào OK sẽ đưa bạn đến màn hình tạo phân vùng cho Swap, như ở bước trước bạn bấm  +, lần này kiểu file sẽ là Swap area

![Create Home Partition](/Images/swap-partition.png)  


- Tương tự, đến bước tạo phân vùng cho Home, bạn sẽ phân bổ dung lượng tối đa cho nó vì đây là nơi bạn sẽ lưu nhạc, hình ảnh, và các file đã tải xuống.

![Create Home Partition](/Images/home-partition.png)  

- Nhấn **Install Now** để bắt đầu cài đặt.  

![Create Home Partition](/Images/install-partition-now.png)  
- Chọn **Timezone**:  

![Select Timezone](/Images/select-timezone.png)  

- Nhập **username**, **computer name**, và **password**:  

![Set User Info](/Images/who-are-u.png)  

- Chờ khoảng **10 phút** để hoàn tất cài đặt.  
- Khởi động lại hệ thống, tháo **USB** khi được yêu cầu.  
- Sau khi khởi động, bạn sẽ thấy màn hình **GRUB** để chọn **Ubuntu** hoặc **Windows Boot Manager**:  

![GRUB Menu](/Images/grub-menu.png)

- Chọn Ubuntu và enter thôi, hoàn thành cài đặt Ubuntu  trên máy tính (Dual Boot hoặc thay thế Window).
---

## 2. Cài Đặt Linux Trên Máy Ảo (Virtual Machine)

### 2.1 Chuẩn Bị
Để cài đặt **Ubuntu 20.04** trên máy ảo, bạn cần:  
1. Máy tính cài sẵn **Windows 10** trở lên.  
2. Kết nối **Internet** ổn định.

### 2.2 Các Bước Cài Đặt

**Bước 1: Cài Đặt VirtualBox**  
- Tải **VirtualBox** từ trang chính thức: [virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads).  

![VirtualBox Download](/Images/virtualbox.png)

**Bước 2: Tải File ISO Ubuntu**  
- Tải **Ubuntu 20.04 Desktop Image** từ: [releases.ubuntu.com/20.04](https://releases.ubuntu.com/20.04/).  

![Ubuntu 20.04 ISO](/Images/ubuntu_20.04.png)

**Bước 3: Tạo Máy Ảo Trong VirtualBox**  
- Mở **VirtualBox**, chọn **Machine** → **New**:  

![VirtualBox New Machine](/Images/virtualbox-new-machine.png)  

- Cấu hình máy ảo:  
  - **Name and Operating System**: Nhập tên, chọn thư mục lưu trữ, chọn file **ISO Ubuntu 20.04** vừa tải ở mục **ISO Image**.  

![VirtualBox ISO Selection](/Images/virtual-new-machine-b1.png)  

  - **Unattended Install**: Tạo tài khoản người dùng, cấu hình **hostname** và **domain name** (tùy chọn).  

![VirtualBox User Setup](/Images/virtual-new-machine-b2.png)  

  - **Cấp phát tài nguyên**: Gán **RAM** (khuyến nghị 2-4GB) và **CPU** (2-4 cores).  

![VirtualBox Resource Allocation](/Images/virtual-new-machine-b3.png)  

  - **Ổ cứng**: Tạo ổ cứng ảo, mặc định **25GB**, có thể tùy chỉnh.  

![VirtualBox Hard Disk](/Images/virtual-new-machine-b4.png)  

- Nhấn **Finish** để hoàn tất.

**Bước 4: Khởi Chạy Máy Ảo**  
- Chọn máy ảo vừa tạo, nhấn **Start**:  

![VirtualBox Start Machine](/Images/virtual-new-machine-start.png)

**Bước 5: Cài Đặt Hệ Điều Hành Ubuntu**  
- Tiếp theo chúng ta sẽ cài đặt hệ điều hành Ubuntu. Bạn có thể follow theo các hướng dẫn dưới đây.

![wellcome ubuntu](/Images/welcome-ubuntu.png)

![wellcome ubuntu](/Images/install-type-erase.png)

![Select Timezone](/Images/select-timezone.png)






- Chờ quá trình cài đặt hoàn tất (khoảng **10-15 phút**).  
- Sau khi cài đặt xong, **Ubuntu 20.04** sẽ hoạt động trên **VirtualBox**.
---
THE END