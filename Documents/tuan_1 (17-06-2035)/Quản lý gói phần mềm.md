# Quản Lý Gói Phần Mềm Trên Linux

## Mục Lục
- [1. Giới Thiệu Về Quản Lý Gói Phần Mềm](#1-giới-thiệu-về-quản-lý-gói-phần-mềm)
- [2. Quản Lý Gói Phần Mềm Với `apt` (Debian/Ubuntu)](#2-quản-lý-gói-phần-mềm-với-apt-debianubuntu)
  - [2.1 Cập Nhật Thông Tin Kho Phần Mềm](#21-cập-nhật-thông-tin-kho-phần-mềm)
  - [2.2 Cài Đặt Gói Phần Mềm](#22-cài-đặt-gói-phần-mềm)
  - [2.3 Gỡ Bỏ Gói Phần Mềm](#23-gỡ-bỏ-gói-phần-mềm)
  - [2.4 Cập Nhật Gói Phần Mềm](#24-cập-nhật-gói-phần-mềm)
- [3. Quản Lý Gói Phần Mềm Với `dnf` (RHEL/CentOS/Fedora)](#3-quản-lý-gói-phần-mềm-với-dnf-rhelcentosfedora)
  - [3.1 Cập Nhật Thông Tin Kho Phần Mềm](#31-cập-nhật-thông-tin-kho-phần-mềm)
  - [3.2 Cài Đặt Gói Phần Mềm](#32-cài-đặt-gói-phần-mềm)
  - [3.3 Gỡ Bỏ Gói Phần Mềm](#33-gỡ-bỏ-gói-phần-mềm)
  - [3.4 Cập Nhật Gói Phần Mềm](#34-cập-nhật-gói-phần-mềm)
- [4. Quản Lý Gói Phần Mềm Với `pacman` (Arch Linux)](#4-quản-lý-gói-phần-mềm-với-pacman-arch-linux)
  - [4.1 Cập Nhật Thông Tin Kho Phần Mềm](#41-cập-nhật-thông-tin-kho-phần-mềm)
  - [4.2 Cài Đặt Gói Phần Mềm](#42-cài-đặt-gói-phần-mềm)
  - [4.3 Gỡ Bỏ Gói Phần Mềm](#43-gỡ-bỏ-gói-phần-mềm)
  - [4.4 Cập Nhật Gói Phần Mềm](#44-cập-nhật-gói-phần-mềm)

---

## 1. Giới Thiệu Về Quản Lý Gói Phần Mềm

Trong Linux, phần mềm được phân phối dưới dạng **gói** (*packages*), được quản lý bởi các công cụ quản lý gói. Mỗi bản phân phối Linux sử dụng công cụ riêng với định dạng gói khác nhau:

- **Debian/Ubuntu**: Sử dụng **`apt`** với định dạng gói `.deb`.
- **RHEL/CentOS/Fedora**: Sử dụng **`yum`** (cũ) hoặc **`dnf`** với định dạng gói `.rpm`.
- **Arch Linux**: Sử dụng **`pacman`** với định dạng gói `.pkg.tar.zst`.

Các công cụ này tự động xử lý **phụ thuộc** (*dependencies*) và cung cấp các lệnh để quản lý gói phần mềm.

---

## 2. Quản Lý Gói Phần Mềm Với `apt` (Debian/Ubuntu)

### 2.1 Cập Nhật Thông Tin Kho Phần Mềm

**Mục đích**: Đồng bộ danh sách gói từ các kho phần mềm.

**Cấu trúc tổng quát**:
```bash
sudo apt update
```

**Ví dụ**:
![Update APT Cache](/Images/apt-update.png)

### 2.2 Cài Đặt Gói Phần Mềm

**Mục đích**: Cài đặt một hoặc nhiều gói phần mềm.

**Cấu trúc tổng quát**:
```bash
sudo apt install [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**                | **Mô tả**                                           |
|-----------------------------|-----------------------------------------------------|
| `-y`                        | Tự động xác nhận (bỏ qua lời nhắc).                 |
| `--no-install-recommends`   | Không cài đặt các gói được đề xuất.                 |
| `--reinstall`               | Cài đặt lại gói đã có.                              |

**Ví dụ**:
![Install Package with APT](/Images/apt-install.png)

### 2.3 Gỡ Bỏ Gói Phần Mềm

**Mục đích**: Xóa gói phần mềm khỏi hệ thống.

**Cấu trúc tổng quát**:
```bash
sudo apt remove [tùy chọn] <tên_gói>
```

Hoặc:
```bash
sudo apt purge [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `-y`            | Tự động xác nhận.                                   |
| `--autoremove`  | Xóa các gói phụ thuộc không còn cần thiết.          |

**Ghi chú**:
- **`remove`**: Xóa gói nhưng giữ lại tệp cấu hình.
- **`purge`**: Xóa gói và cả tệp cấu hình.

**Ví dụ**:
![Remove Package with APT](/Images/apt-remove.png)

### 2.4 Cập Nhật Gói Phần Mềm

**Mục đích**: Nâng cấp các gói phần mềm đã cài đặt lên phiên bản mới.

**Cấu trúc tổng quát**:
```bash
sudo apt upgrade [tùy chọn]
```

Hoặc:
```bash
sudo apt dist-upgrade [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `-y`            | Tự động xác nhận.                                   |

**Ghi chú**:
- **`upgrade`**: Cập nhật gói mà không thay đổi phụ thuộc.
- **`dist-upgrade`**: Cập nhật gói và xử lý thay đổi phụ thuộc.

**Ví dụ**:
![Upgrade Packages with APT](/Images/apt-upgrade.png)

---

## 3. Quản Lý Gói Phần Mềm Với `dnf` (RHEL/CentOS/Fedora)

### 3.1 Cập Nhật Thông Tin Kho Phần Mềm

**Mục đích**: Đồng bộ danh sách gói từ các kho.

**Cấu trúc tổng quát**:
```bash
sudo dnf update
```

**Ví dụ**:
```bash
sudo dnf update
```

### 3.2 Cài Đặt Gói Phần Mềm

**Mục đích**: Cài đặt gói phần mềm.

**Cấu trúc tổng quát**:
```bash
sudo dnf install [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                           |
|---------------------|-----------------------------------------------------|
| `-y`                | Tự động xác nhận.                                   |
| `--nogpgcheck`      | Bỏ qua kiểm tra chữ ký GPG.                         |
| `--enablerepo=<kho>`| Kích hoạt kho cụ thể.                               |

**Ví dụ**:
```bash
sudo dnf install -y httpd
```

### 3.3 Gỡ Bỏ Gói Phần Mềm

**Mục đích**: Xóa gói phần mềm.

**Cấu trúc tổng quát**:
```bash
sudo dnf remove [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                           |
|---------------------|-----------------------------------------------------|
| `-y`                | Tự động xác nhận.                                   |
| `--noautoremove`    | Không xóa các gói phụ thuộc.                        |

**Ví dụ**:
```bash
sudo dnf remove -y httpd
```

### 3.4 Cập Nhật Gói Phần Mềm

**Mục đích**: Nâng cấp gói phần mềm.

**Cấu trúc tổng quát**:
```bash
sudo dnf upgrade [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `-y`            | Tự động xác nhận.                                   |

**Ví dụ**:
```bash
sudo dnf upgrade -y
```

---

## 4. Quản Lý Gói Phần Mềm Với `pacman` (Arch Linux)

### 4.1 Cập Nhật Thông Tin Kho Phần Mềm

**Mục đích**: Đồng bộ danh sách gói.

**Cấu trúc tổng quát**:
```bash
sudo pacman -Sy
```

**Ví dụ**:
```bash
sudo pacman -Sy
```

### 4.2 Cài Đặt Gói Phần Mềm

**Mục đích**: Cài đặt gói.

**Cấu trúc tổng quát**:
```bash
sudo pacman -S [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `--noconfirm`   | Tự động xác nhận.                                   |
| `--needed`      | Không cài lại nếu gói đã mới nhất.                  |

**Ví dụ**:
```bash
sudo pacman -S --noconfirm vim
```

### 4.3 Gỡ Bỏ Gói Phần Mềm

**Mục đích**: Xóa gói.

**Cấu trúc tổng quát**:
```bash
sudo pacman -R [tùy chọn] <tên_gói>
```

Hoặc:
```bash
sudo pacman -Rns [tùy chọn] <tên_gói>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `--noconfirm`   | Tự động xác nhận.                                   |
| `-n`            | Xóa tệp cấu hình.                                   |
| `-s`            | Xóa phụ thuộc không cần thiết.                      |

**Ví dụ**:
```bash
sudo pacman -Rns --noconfirm vim
```

### 4.4 Cập Nhật Gói Phần Mềm

**Mục đích**: Nâng cấp hệ thống.

**Cấu trúc tổng quát**:
```bash
sudo pacman -Syu [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                           |
|-----------------|-----------------------------------------------------|
| `--noconfirm`   | Tự động xác nhận.                                   |

**Ví dụ**:
```bash
sudo pacman -Syu --noconfirm
```

---
THE END