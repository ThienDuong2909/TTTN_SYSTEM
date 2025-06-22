# Quản Lý Phân Vùng Ổ Đĩa Trên Linux
---
## Mục Lục
- [1. Giới Thiệu](#1-giới-thiệu)
- [2. Các Khái Niệm Cơ Bản](#2-các-khái-niệm-cơ-bản)
  - [2.1 File System](#21-file-system)
  - [2.2 Bảng Phân Vùng](#22-bảng-phân-vùng)
- [3. Quản Lý Phân Vùng Đĩa Cứng Bằng `fdisk`](#3-quản-lý-phân-vùng-đĩa-cứng-bằng-fdisk)
  - [3.1 Hiểu Về `fdisk`](#31-hiểu-về-fdisk)
  - [3.2 Các Bước Làm Việc Với `fdisk`](#32-các-bước-làm-việc-với-fdisk)
- [4. Quản Lý LVM - Logical Volume Management](#4-quản-lý-lvm---logical-volume-management)
  - [4.1 Giới Thiệu](#41-giới-thiệu)
  - [4.2 Cấu Trúc của LVM](#42-cấu-trúc-của-lvm)
  - [4.3 Các Bước Quản Lý và Tạo LVM](#43-các-bước-quản-lý-và-tạo-lvm)

---

## 1. Giới Thiệu
**Phân vùng ổ đĩa** là quá trình chia nhỏ ổ đĩa cứng để phục vụ các nhu cầu cụ thể. Ví dụ:  
- Ổ **C:\** trên Windows thường chứa các file của **hệ điều hành**.  
- Các phân vùng khác dùng để lưu trữ **dữ liệu cá nhân**, **công việc**.  

**Lợi ích** của việc phân vùng:  
- **Tổ chức dễ dàng**: Giúp quản lý dữ liệu hiệu quả hơn.  
- **Cài nhiều hệ điều hành**: Hỗ trợ **Dual Boot** (ví dụ: Linux và Windows).  
- **Sao lưu tiện lợi**: Dễ dàng sao lưu và khôi phục dữ liệu.

---

## 2. Các Khái Niệm Cơ Bản

### 2.1 File System
**File System** là *tập hợp logic* của các file trên một phân vùng hoặc ổ đĩa. **Hệ điều hành** sử dụng File System để **lưu trữ** và **truy xuất dữ liệu**. Nếu không có File System, dữ liệu sẽ được lưu dưới dạng một khối chung, không xác định được điểm **bắt đầu** hay **kết thúc**.

- **Windows**: Sử dụng chủ yếu `FAT32` và `NTFS`.  
- **Linux**: Hỗ trợ nhiều định dạng như `ext2`, `ext3`, `ext4`, `xfs`, `vfat`, `swap`, `ZFS`, và `GlusterFS`.  


### 2.2 Bảng Phân Vùng
**Bảng phân vùng** (*partition table*) chứa thông tin về **kích thước** và **vị trí** của các phân vùng trên ổ đĩa cứng. Hai loại bảng phân vùng phổ biến là **MBR** và **GPT**.

- **Master Boot Record (MBR)**:  
  - Là **boot sector đặc biệt**, chứa thông tin về **hệ điều hành** và được tải vào **RAM**.  
  - Còn gọi là **partition sector** hoặc **master partition table**, lưu vị trí các phân vùng.  
  - **Hạn chế**:  
    - Chỉ hỗ trợ ổ đĩa dưới **2TB**.  
    - Tối đa **4 phân vùng chính**.  
    - Cần chuyển đổi sang **extended partition** để tạo thêm phân vùng.  

- **GUID Partition Table (GPT)**:  
  - Chuẩn mới, thuộc **UEFI** (thay thế BIOS).  
  - **Ưu điểm**:  
    - Hỗ trợ **không giới hạn số phân vùng** (giới hạn bởi hệ điều hành).  
    - Giao diện **UEFI** hiện đại, dễ sử dụng.  

---

## 3. Quản Lý Phân Vùng Đĩa Cứng Bằng `fdisk`

### 3.1 Hiểu Về `fdisk`
**`fdisk`** là một **tiện ích dòng lệnh** (*command-line utility*) dùng để quản lý phân vùng ổ đĩa cứng. Chức năng:  
- **Phân chia**, **thay đổi**, **thêm**, hoặc **xóa** phân vùng.  
- **Ưu điểm**: Nhanh, gọn, tích hợp sẵn trên hầu hết các **hệ điều hành Linux**.  

**Lưu ý**:  
- **Sao lưu dữ liệu** trước khi thay đổi phân vùng vì `fdisk` có thể xóa dữ liệu.  
- `fdisk` **không hỗ trợ** bảng phân vùng **GPT** và phân vùng lớn hơn **2TB**.

### 3.2 Các Bước Làm Việc Với `fdisk`

**#.Xem Tất Cả Các Phân Vùng**  
- Sử dụng lệnh **`fdisk -l`** để liệt kê thông tin chi tiết về các phân vùng trong hệ thống:  

![List Partitions](/Images/fdisk-l.png)  

- Kết quả ví dụ: Hiển thị **2 disk** (*VBOX HARDDISK*, dung lượng **25 GiB**) và 4 thiết bị (**/dev/sda1**, **/dev/sda2**, **/dev/sda5**, **/dev/sdb1**).  

**Bảng mô tả các cột trong kết quả `fdisk -l`**:  

| Cột         | Ý Nghĩa                                                                 |
|-------------|-------------------------------------------------------------------------|
| `Device`    | Tên thiết bị được gán cho phân vùng (ví dụ: `/dev/sda1`).                |
| `Start`     | Số **sector bắt đầu** của phân vùng.                                    |
| `End`       | Số **sector kết thúc** của phân vùng.                                   |
| `Sectors`   | **Tổng số sector** trong phân vùng (nhân với kích thước sector để tính dung lượng). |
| `Size`      | Kích thước phân vùng ở dạng **human-readable** (ví dụ: GiB).             |
| `Type`      | Loại phân vùng (ví dụ: Linux, Swap).                                    |

> **Thông tin**: Để xem bảng phân vùng của một ổ đĩa cụ thể, thêm địa chỉ vào lệnh, ví dụ: **`fdisk -l /dev/sdb`**.

**#.Vào Chế Độ Chỉnh Sửa Phân Vùng**  
- Sử dụng lệnh **`fdisk [address]`** (ví dụ: **`fdisk /dev/sda`**) để vào chế độ chỉnh sửa:  

![Enter fdisk](/Images/enter-fdisk.png)  

- Khi thấy thông báo *“Welcome to fdisk....Command (m) for help:”*, nhập **`m`** để hiển thị menu lệnh:  

| Lệnh | Mô Tả                                   |
|------|-----------------------------------------|
| `a`  | Bật/tắt cờ bootable cho phân vùng.      |
| `b`  | Chỉnh sửa nhãn đĩa BSD.                 |
| `c`  | Bật/tắt cờ tương thích DOS.             |
| `d`  | Xóa một phân vùng.                      |
| `g`  | Tạo bảng phân vùng GPT mới.             |
| `G`  | Tạo bảng phân vùng IRIX (SGI).          |
| `l`  | Liệt kê các loại phân vùng.             |
| `m`  | Hiển thị menu lệnh.                     |
| `n`  | Thêm phân vùng mới.                     |
| `o`  | Tạo bảng phân vùng DOS mới.             |
| `p`  | Hiển thị bảng phân vùng hiện tại.       |
| `q`  | Thoát mà không lưu thay đổi.            |
| `s`  | Tạo nhãn đĩa Sun mới.                   |
| `t`  | Thay đổi ID hệ thống của phân vùng.     |
| `u`  | Thay đổi đơn vị hiển thị/nhập liệu.     |
| `v`  | Kiểm tra bảng phân vùng.                |
| `w`  | Lưu bảng phân vùng và thoát.            |
| `x`  | Chức năng nâng cao (dành cho chuyên gia). |

**#. Kiểm Tra Không Gian Trống**  
- Để kiểm tra không gian trống trên ổ đĩa (ví dụ: `/dev/sdb`), nhập lệnh **`F`** trong chế độ `fdisk`:  

![Check Free Space](/Images/fdisk-f.png)

**Bước 4: Tạo Một Phân Vùng Mới**  
- Nhập lệnh **`n`** để tạo phân vùng mới:  

![Create New Partition](/Images/fdisk-new-partition.png)  

- Chọn loại phân vùng:  
  - **`p`**: Tạo phân vùng **Primary**.  
  - **`e`**: Tạo phân vùng **Extended**.  

> **Thông tin**: Chọn **Primary** nếu phân vùng chứa **hệ điều hành** hoặc ổ đĩa có dưới **4 phân vùng**. Chọn **Extended** nếu cần tạo hơn **4 phân vùng**.

- Trong ví dụ này, mình chọn phân vùng là Primary bằng cách chọn p.
- Nhập **First sector** (mặc định, ví dụ: `2048`) và **Last sector** (ví dụ: `+512MB`).  

![Set Partition Size](/Images/fdisk-set-partition-size.png)  

- Nhấn **`w`** để lưu và chờ hệ thống tạo phân vùng:  

![Partition Created](/Images/fdisk-created-success.png)

**Bước 5: Xóa Một Phân Vùng**  
- Vào chế độ chỉnh sửa bằng **`fdisk [address]`**, sau đó nhập **`d`** để xóa phân vùng:  
  - Nếu chỉ có một phân vùng, nó sẽ tự động bị xóa.  
  - Nếu có nhiều phân vùng, chọn phân vùng cần xóa.  

![Delete Partition](/Images/fdisk-d.png)  

- Nhấn **`w`** để lưu, sau đó kiểm tra lại bằng **`fdisk -l`**:  

![Verify Deletion](/Images/verify-fdisk-d.png)

**Bước 6: Format Một Phân Vùng**  
- Sử dụng lệnh **`mkfs.ext4`** để định dạng phân vùng (ví dụ: `/dev/sda1`) sang file system **`ext4`**:  

![Format Partition](/Images/mkfs.ext4.png)  

> **Lưu ý**: Format sẽ **xóa toàn bộ dữ liệu** trên phân vùng, hãy chọn đúng phân vùng.

**Bước 7: Gắn và Gỡ Phân Vùng**  
- Đảm bảo có **quyền root** hoặc sử dụng **`sudo`**.  
- Tạo thư mục **mount point** (ví dụ: `/data`):  
  ```bash
  mkdir /data
  ```
- Gắn phân vùng (ví dụ: `/dev/sdb1`) vào thư mục:  
  ```bash
  mount /dev/sdb1 /data
  ```
- Kiểm tra phân vùng đã được gắn bằng:  
  ```bash
  df -h
  ```

![Mount Partition](/Images/mount-partition.png)  

- Xem thông tin chi tiết về lệnh **`mount`**:  
  ```bash
  man mount
  ```

**Bảng các tùy chọn quan trọng của lệnh `mount`**:  

| Tùy Chọn | Mô Tả                                      |
|----------|--------------------------------------------|
| `-l`     | Liệt kê mọi **file system** đã được mount. |
| `-h`     | Hiển thị các **tùy chọn** của lệnh.        |
| `-V`     | Hiển thị **thông tin phiên bản**.          |
| `-a`     | Mount mọi thiết bị trong `/etc/fstab`.     |
| `-t`     | Chỉ định **loại file system**.             |
| `-T`     | Mô tả file **fstab** thay thế.             |
| `-r`     | Mount ở chế độ **chỉ đọc** (*read-only*).  |

- Gỡ phân vùng bằng lệnh:  
  ```bash
  umount /dev/sdb1
  ```

## 4. Quản Lý LVM - Logical Volume Management

### 4.1 Giới Thiệu
**Logical Volume Management (LVM)** là một công cụ quản lý đĩa có sẵn trên hầu hết các **bản phân phối Linux**, mang lại nhiều lợi thế so với quản lý phân vùng truyền thống. LVM cho phép:  
- Tạo nhiều **ổ đĩa logic** (*logical volumes*).  
- **Thay đổi kích thước** (tăng hoặc giảm) các bộ phận logic theo ý muốn của người quản trị.  

### 4.2 Cấu Trúc của LVM
- **Physical Volume (PV)**: Một hoặc nhiều **đĩa cứng** hoặc **phân vùng** được định cấu hình thành Physical Volume.  
- **Volume Group (VG)**: Được tạo bằng cách sử dụng một hoặc nhiều **Physical Volume**.  
- **Logical Volume (LV)**: Nhiều **Logical Volume** có thể được tạo trong một **Volume Group**.

### 4.3 Các Bước Quản Lý và Tạo LVM

**Bước 1: Xem Thông Tin Các Thiết Bị Lưu Trữ Hiện Tại**  
- Sử dụng lệnh **`lsblk -f`** để xem thông tin các thiết bị lưu trữ và hệ thống tệp tin:  

![List Storage Devices](/Images/lsblk-f.png)

**Bước 2: Tạo Physical Volume, Volume Group, và Logical Volume**  
- **Tạo Physical Volume**:  
  - Chạy lệnh **`pvcreate /dev/sdb1 /dev/sdc1`** để tạo **Physical Volume**.  
  - Nếu phân vùng đã được định dạng hệ thống tệp tin, bạn sẽ thấy cảnh báo. Nhập **Y** và nhấn **Enter** để tiếp tục:  

![Create Physical Volume](/Images/create-physical-volume.png)  

- **Liệt kê Physical Volume**:  
  - Chạy lệnh **`pvs`** để xem các **Physical Volume** vừa tạo:  

![List Physical Volumes](/Images/pvs.png)  

**Bảng ý nghĩa các trường của `pvs`**:  

| Trường   | Ý Nghĩa                              |
|----------|--------------------------------------|
| `PV`     | Đĩa được sử dụng (*Physical Volume*). |
| `PFree`  | Kích thước đĩa vật lý còn trống.     |

- **Xem chi tiết Physical Volume**:  
  - Chạy **`pvdisplay /dev/sdb1`** để xem thông tin chi tiết về **Physical Volume**:  

![Physical Volume Details](/Images/pvdisplay.png)

**Bước 3: Tạo Volume Group**  
- Tạo **Volume Group** với tên `vg0` bằng cách sử dụng `/dev/sdb1` và `/dev/sdc1`:  
  ```bash
  vgcreate vg0 /dev/sdb1 /dev/sdc1
  ```

![Create Volume Group](/Images/vgcreate.png)  

- Xem thông tin **Volume Group** vừa tạo:  
  ```bash
  vgdisplay vg0
  ```

![Volume Group Details](/Images/vgdisplay.png)  

**Bảng ý nghĩa các trường của `vgdisplay`**:  

| Trường      | Ý Nghĩa                                                                 |
|-------------|-------------------------------------------------------------------------|
| `VG Name`   | Tên của **Volume Group**.                                               |
| `Format`    | Kiến trúc **LVM** được sử dụng.                                         |
| `VG Access` | Trạng thái **Volume Group** (có thể đọc/ghi, sẵn sàng sử dụng).          |
| `VG Status` | Trạng thái thay đổi kích thước (**resizable** nếu có thể mở rộng).       |
| `PE Size`   | Kích thước **Physical Extent** (mặc định 4MB).                           |
| `Total PE`  | Tổng dung lượng **Physical Extent** của **Volume Group**.                |
| `Alloc PE`  | Tổng **Physical Extent** đã sử dụng.                                    |
| `Free PE`   | Tổng **Physical Extent** chưa sử dụng.                                  |

- Kiểm tra số lượng **Physical Volume** trong **Volume Group**:  
  ```bash
  vgs
  ```

![List Volume Groups](/Images/vgs.png)  

**Bảng ý nghĩa các trường của `vgs`**:  

| Trường   | Ý Nghĩa                                                        |
|----------|----------------------------------------------------------------|
| `VG`     | Tên **Volume Group**.                                          |
| `#PV`    | Số lượng **Physical Volume** trong **Volume Group**.            |
| `VFree`  | Không gian trống có sẵn trong **Volume Group**.                 |
| `VSize`  | Tổng kích thước của **Volume Group**.                           |
| `#LV`    | Số lượng **Logical Volume** trong **Volume Group**.             |
| `#SN`    | Số lượng **Snapshot** của **Volume Group**.                     |
| `Attr`   | Trạng thái (**writable**, **readable**, **resizable**, ...).    |

**Bước 4: Tạo Logical Volume**  
- Tạo hai **Logical Volume**:  
  - `projects` với dung lượng **500MB**.  
  - `backups` sử dụng toàn bộ dung lượng còn lại của **Volume Group**.  
  - Lệnh:  
    ```bash
    lvcreate -L 500M -n projects vg0
    lvcreate -l 100%FREE -n backups vg0
    ```

![Create Logical Volumes](/Images/lvcreate.png)  

**Bảng ý nghĩa các tùy chọn của `lvcreate`**:  

| Tùy Chọn | Ý Nghĩa                                                 |
|----------|--------------------------------------------------------|
| `-n`     | Chỉ định tên của **Logical Volume**.                    |
| `-L`     | Chỉ định kích thước cố định (ví dụ: `500M`).            |
| `-l`     | Chỉ định phần trăm không gian còn lại trong **Volume Group**. |

- Xem danh sách **Logical Volume**:  
  ```bash
  lvs
  ```

![List Logical Volumes](/Images/lvs.png)  

**Bảng ý nghĩa các trường của `lvs`**:  

| Trường     | Ý Nghĩa                                                  |
|------------|---------------------------------------------------------|
| `LV`       | Tên **Logical Volume**.                                 |
| `%Data`    | Phần trăm dung lượng **Logical Volume** đã sử dụng.      |
| `LSize`    | Kích thước của **Logical Volume**.                       |

- Định dạng **Logical Volume** với file system **`ext4`** (hỗ trợ tăng/giảm kích thước, không như `xfs` chỉ hỗ trợ tăng):  
  ```bash
  mkfs.ext4 /dev/vg0/projects
  mkfs.ext4 /dev/vg0/backups
  ```

![Format Logical Volumes](/Images/mkfs.ext4-lvm.png)

**#. Mở Rộng Volume Group**  
- Tạo thư mục và gắn (**mount**) **Logical Volume**:  
  ```bash
  mkdir /mnt/projects
  mkdir /mnt/backups
  mount /dev/vg0/projects /mnt/projects
  mount /dev/vg0/backups /mnt/backups
  ```

![Mount Logical Volumes](/Images/mkdir.png)  

![Mount Logical Volumes](/Images/mount-lvm.png)  

- Kiểm tra không gian đĩa:  
  ```bash
  df -TH
  ```

![Check Disk Usage](/Images/df-TH.png)  

- Mở rộng **Volume Group** bằng cách thêm `/dev/sdc2` vào `vg0`:  
  ```bash
  vgextend vg0 /dev/sdc2
  ```

![Extend Volume Group](/Images/vgextend.png)


---
THE END