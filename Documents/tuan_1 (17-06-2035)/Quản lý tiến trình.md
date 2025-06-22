# Hướng Dẫn Quản Lý Tiến Trình Trên Linux

## Mục Lục
- [1. Giới Thiệu Về Tiến Trình Trong Linux](#1-giới-thiệu-về-tiến-trình-trong-linux)
- [2. Theo Dõi Tiến Trình](#2-theo-dõi-tiến-trình)
  - [2.1 Lệnh `ps` (Process Status)](#21-lệnh-ps-process-status)
  - [2.2 Lệnh `top`](#22-lệnh-top)
  - [2.3 Lệnh `htop`](#23-lệnh-htop)
- [3. Kiểm Soát Tiến Trình](#3-kiểm-soát-tiến-trình)
  - [3.1 Lệnh `kill`](#31-lệnh-kill)
  - [3.2 Lệnh `killall`](#32-lệnh-killall)
  - [3.3 Lệnh `pkill`](#33-lệnh-pkill)
- [4. Quản Lý Dịch Vụ (Service) Với `systemctl`](#4-quản-lý-dịch-vụ-service-với-systemctl)

---

## 1. Giới Thiệu Về Tiến Trình Trong Linux

Trong Linux, **tiến trình** (*process*) là một chương trình đang thực thi. Mỗi tiến trình có một **PID** (*Process ID*) duy nhất để nhận diện. Tiến trình có thể chạy ở **foreground** (tiền cảnh) hoặc **background** (nền). Các công cụ quản lý tiến trình giúp:

- Theo dõi tài nguyên (**CPU**, **RAM**).
- Kiểm soát trạng thái tiến trình.
- Chấm dứt tiến trình khi cần.

---

## 2. Theo Dõi Tiến Trình

### 2.1 Lệnh `ps` (Process Status)

**Mục đích**: Hiển thị thông tin về các tiến trình đang chạy.

**Cấu trúc tổng quát**:
```bash
ps [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                   |
|---------------------|-------------------------------------------------------------|
| `-aux`              | Hiển thị tất cả tiến trình của mọi người dùng (UNIX format). |
| `-ef`               | Hiển thị tiến trình theo định dạng cây (full format).       |
| `-u <tên_người_dùng>` | Chỉ hiển thị tiến trình của một người dùng cụ thể.          |
| `--pid <PID>`       | Hiển thị thông tin về tiến trình có PID cụ thể.             |

**Ví dụ**:
```bash
ps aux
```
![PS-aux Interface](/Images/ps-aux.png)
Lệnh trên hiển thị tất cả tiến trình với thông tin chi tiết như người dùng, %CPU, %MEM, và lệnh khởi chạy.

### 2.2 Lệnh `top`

**Mục đích**: Theo dõi tiến trình theo thời gian thực.

**Cấu trúc tổng quát**:
```bash
top [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                   |
|---------------------|-------------------------------------------------------------|
| `-u <tên_người_dùng>` | Chỉ hiển thị tiến trình của người dùng cụ thể.             |
| `-d <giây>`         | Đặt khoảng thời gian làm mới (mặc định là 3 giây).          |
| `-p <PID>`          | Theo dõi tiến trình với PID cụ thể.                         |

**Ví dụ**:
```bash
top -u user1
```
![Top Interface](/Images/top.png)

Lệnh trên hiển thị tiến trình của người dùng `user1` trong giao diện tương tác.

**Lưu ý**: Trong giao diện `top`, nhấn `q` để thoát, `k` để gửi tín hiệu chấm dứt một tiến trình.

### 2.3 Lệnh `htop`

**Mục đích**: Cung cấp giao diện tương tác thân thiện hơn `top`.

**Cấu trúc tổng quát**:
```bash
htop [tùy chọn]
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                   |
|---------------------|-------------------------------------------------------------|
| `-u <tên_người_dùng>` | Chỉ hiển thị tiến trình của người dùng cụ thể.             |
| `-s <cột>`          | Sắp xếp theo cột (ví dụ: CPU, MEM).                         |
| `--delay <giây>`    | Đặt khoảng thời gian làm mới.                               |

**Ví dụ**:
```bash
htop
```

Lệnh trên mở giao diện `htop` hiển thị tiến trình:

![Htop Interface](/Images/htop.png)

**Lưu ý**: `htop` không được cài đặt sẵn trên nhiều hệ thống, cần cài đặt bằng lệnh như:
```bash
sudo apt install htop
```

---

## 3. Kiểm Soát Tiến Trình

### 3.1 Lệnh `kill`

**Mục đích**: Gửi tín hiệu để chấm dứt hoặc điều khiển tiến trình.

**Cấu trúc tổng quát**:
```bash
kill [tùy chọn] <PID>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `-<tín_hiệu>`   | Chỉ định tín hiệu (ví dụ: `-SIGTERM`, `-SIGKILL`).          |
| `-9`            | Gửi tín hiệu `SIGKILL` (chấm dứt ngay lập tức).             |
| `-15`           | Gửi tín hiệu `SIGTERM` (chấm dứt nhẹ nhàng, mặc định).      |
| `-l`            | Liệt kê tất cả tín hiệu có sẵn.                             |

**Ví dụ**:
```bash
kill -9 1234
```

Lệnh trên chấm dứt ngay tiến trình có PID `1234`.

### 3.2 Lệnh `killall`

**Mục đích**: Chấm dứt tiến trình theo tên.

**Cấu trúc tổng quát**:
```bash
killall [tùy chọn] <tên_tiến_trình>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**        | **Mô tả**                                                   |
|---------------------|-------------------------------------------------------------|
| `-i`                | Yêu cầu xác nhận trước khi chấm dứt.                        |
| `-s <tín_hiệu>`     | Chỉ định tín hiệu (ví dụ: `SIGTERM`, `SIGKILL`).            |
| `-u <người_dùng>`   | Chỉ chấm dứt tiến trình của người dùng cụ thể.              |

**Ví dụ**:
```bash
killall -s SIGTERM firefox
```

Lệnh trên chấm dứt tất cả tiến trình `firefox` với tín hiệu `SIGTERM`.

### 3.3 Lệnh `pkill`

**Mục đích**: Chấm dứt tiến trình dựa trên tiêu chí như tên hoặc mẫu.

**Cấu trúc tổng quát**:
```bash
pkill [tùy chọn] <mẫu>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**            | **Mô tả**                                                   |
|-------------------------|-------------------------------------------------------------|
| `-u <người_dùng>`       | Chỉ chấm dứt tiến trình của người dùng cụ thể.              |
| `-f`                    | Khớp toàn bộ dòng lệnh, không chỉ tên tiến trình.           |
| `--signal <tín_hiệu>`   | Chỉ định tín hiệu.                                          |

**Ví dụ**:
```bash
pkill -u user1
```

Lệnh trên chấm dứt tất cả tiến trình của người dùng `user1`.

---

## 4. Quản Lý Dịch Vụ (Service) Với `systemctl`

**Mục đích**: Quản lý các dịch vụ hệ thống (thường là tiến trình chạy nền lâu dài).

**Cấu trúc tổng quát**:
```bash
sudo systemctl [tùy chọn] <tên_dịch_vụ>
```

**Bảng các tùy chọn phổ biến**:

| **Tùy chọn**    | **Mô tả**                                                   |
|-----------------|-------------------------------------------------------------|
| `start`         | Khởi động dịch vụ.                                          |
| `stop`          | Dừng dịch vụ.                                               |
| `restart`       | Khởi động lại dịch vụ.                                      |
| `status`        | Hiển thị trạng thái dịch vụ.                                |
| `enable`        | Kích hoạt dịch vụ khởi động cùng hệ thống.                  |
| `disable`       | Vô hiệu hóa khởi động tự động.                              |

**Ví dụ**:
```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```

Lệnh trên khởi động lại và xem trạng thái của dịch vụ web Apache:

![Systemctl Apache Status](/Images/systemctl-apache2.png)

---
THE END