# Quản Lý Người Dùng và Nhóm Trên Linux

## Mục Lục
- [1. Phân Quyền `sudo`](#1-phân-quyền-sudo)
- [2. Quản Lý Người Dùng](#2-quản-lý-người-dùng)
  - [2.1 Thêm Người Dùng (`useradd`)](#21-thêm-người-dùng-useradd)
  - [2.2 Sửa Đổi Người Dùng (`usermod`)](#22-sửa-đổi-người-dùng-usermod)
  - [2.3 Xóa Người Dùng (`userdel`)](#23-xóa-người-dùng-userdel)
- [3. Quản Lý Nhóm](#3-quản-lý-nhóm)
  - [3.1 Thêm Nhóm (`groupadd`)](#31-thêm-nhóm-groupadd)
  - [3.2 Sửa Đổi Nhóm (`groupmod`)](#32-sửa-đổi-nhóm-groupmod)
  - [3.3 Xóa Nhóm (`groupdel`)](#33-xóa-nhóm-groupdel)
  - [3.4 Xem Nhóm của Người Dùng (`groups`)](#34-xem-nhóm-của-người-dùng-groups)
  - [3.5 Xem Các Nhóm Hiện Tại (`cat /etc/group`)](#35-xem-các-nhóm-hiện-tại-cat-etcgroup)

---

## 1. Phân Quyền `sudo`

**Mục đích**: Cấp quyền quản trị (*sudo*) cho người dùng để thực hiện các lệnh với quyền **root**.

**Cấu trúc tổng quát**:
```bash
usermod -aG sudo <tên_người_dùng>
```

Hoặc chỉnh sửa tệp `/etc/sudoers` bằng lệnh:
```bash
sudo visudo
```

**Bảng các phương pháp cấp quyền `sudo`**:

| **Phương pháp**               | **Mô tả**                                                                 |
|-------------------------------|---------------------------------------------------------------------------|
| Thêm vào nhóm `sudo`          | Sử dụng **`usermod -aG sudo <tên_người_dùng>`** để thêm người dùng vào nhóm `sudo` (phổ biến trên Ubuntu). |
| Chỉnh sửa `/etc/sudoers`      | Thêm dòng `<tên_người_dùng> ALL=(ALL:ALL) ALL` vào `/etc/sudoers` để cấp toàn quyền, hoặc tùy chỉnh quyền cụ thể. |
| Tùy chỉnh quyền cụ thể        | Trong `/etc/sudoers`, giới hạn lệnh, ví dụ: `<tên_người_dùng> ALL=/usr/bin/apt-get` chỉ cho phép chạy `apt-get`. |

**Ví dụ**:
```bash
sudo usermod -aG sudo newuser
```

---

## 2. Quản Lý Người Dùng

### 2.1 Thêm Người Dùng (`useradd`)

**Mục đích**: Tạo một tài khoản người dùng mới.

**Cấu trúc tổng quát**:
```bash
useradd [tùy chọn] <tên_người_dùng>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                                 |
|---------------------|---------------------------------------------------------------------------|
| `-m`                | Tạo thư mục chính (*home directory*) tại `/home/<tên_người_dùng>`.        |
| `-d <đường_dẫn>`    | Chỉ định thư mục chính cụ thể thay vì mặc định.                            |
| `-s <shell>`        | Chỉ định shell (ví dụ: `/bin/bash`, `/bin/sh`, `/bin/zsh`).               |
| `-u <uid>`          | Chỉ định UID (ID người dùng) cụ thể.                                      |
| `-g <nhóm>`         | Chỉ định nhóm chính cho người dùng.                                       |
| `-c "<mô tả>"`      | Thêm mô tả cho tài khoản người dùng.                                      |

**Ví dụ**:
- Thường kết hợp với **`passwd`** để tạo tài khoản và thiết lập mật khẩu:
```bash
sudo useradd -m -s /bin/bash -c "Người dùng mới" newuser
sudo passwd newuser
```

![Add User](/Images/useradd.png)

### 2.2 Sửa Đổi Người Dùng (`usermod`)

**Mục đích**: Sửa đổi thông tin của một tài khoản người dùng hiện có.

**Cấu trúc tổng quát**:
```bash
usermod [tùy chọn] <tên_người_dùng>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**                | **Mô tả**                                                                 |
|-----------------------------|---------------------------------------------------------------------------|
| `-d <đường_dẫn>`            | Thay đổi thư mục chính của người dùng.                                    |
| `-s <shell>`                | Thay đổi shell đăng nhập.                                                 |
| `-l <tên_đăng_nhập_mới>`    | Thay đổi tên đăng nhập của người dùng.                                    |
| `-aG <nhóm>`                | Thêm người dùng vào nhóm bổ sung (kết hợp với `-a` để không xóa nhóm cũ). |
| `-c "<bình luận>"`          | Thay đổi bình luận cho tài khoản người dùng.                              |
| `-u <uid>`                  | Thay đổi UID của người dùng.                                              |

**Ví dụ**:
```bash
sudo usermod -aG sudo -c "Người dùng đã sửa" newuser
```

### 2.3 Xóa Người Dùng (`userdel`)

**Mục đích**: Xóa một tài khoản người dùng.

**Cấu trúc tổng quát**:
```bash
userdel [tùy chọn] <tên_người_dùng>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn** | **Mô tả**                                                                 |
|--------------|---------------------------------------------------------------------------|
| `-r`         | Xóa cả thư mục chính và các tệp liên quan của người dùng.                 |
| `-f`         | Buộc xóa người dùng ngay cả khi đang đăng nhập.                           |

**Ví dụ**:
```bash
sudo userdel -r newuser
```

---

## 3. Quản Lý Nhóm

### 3.1 Thêm Nhóm (`groupadd`)

**Mục đích**: Tạo một nhóm mới.

**Cấu trúc tổng quát**:
```bash
groupadd [tùy chọn] <tên_nhóm>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn** | **Mô tả**                                                                 |
|--------------|---------------------------------------------------------------------------|
| `-g <gid>`   | Chỉ định GID (ID nhóm) cụ thể.                                            |
| `-r`         | Tạo nhóm hệ thống (GID trong khoảng hệ thống).                            |

**Ví dụ**:
```bash
sudo groupadd -g 1001 newgroup
```

### 3.2 Sửa Đổi Nhóm (`groupmod`)

**Mục đích**: Sửa đổi thông tin của một nhóm hiện có.

**Cấu trúc tổng quát**:
```bash
groupmod [tùy chọn] <tên_nhóm>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                                 |
|-----------------|---------------------------------------------------------------------------|
| `-n <tên_mới>`  | Đổi tên nhóm.                                                             |
| `-g <gid>`      | Thay đổi GID của nhóm.                                                    |

**Ví dụ**:
```bash
sudo groupmod -n newgroupname oldgroup
```

### 3.3 Xóa Nhóm (`groupdel`)

**Mục đích**: Xóa một nhóm.

**Cấu trúc tổng quát**:
```bash
groupdel <tên_nhóm>
```

**Ví dụ**:
```bash
sudo groupdel newgroup
```

### 3.4 Xem Nhóm của Người Dùng (`groups`)

**Mục đích**: Xem các nhóm mà một người dùng thuộc về.

**Cấu trúc tổng quát**:
```bash
groups <tên_người_dùng>
```

**Ví dụ**:
![List User Groups](/Images/groups.png)

### 3.5 Xem Các Nhóm Hiện Tại (`cat /etc/group`)

**Mục đích**: Xem danh sách các nhóm hiện có trong hệ thống.

**Cấu trúc tổng quát**:
```bash
cat /etc/group
```

**Ví dụ**:
![List All Groups](/Images/cat-etc-group.png)

---
THE END