# Tìm hiểu về Object Storage

# Mục lục
- [Giới thiệu](#giới-thiệu)
  - [Khái niệm](#khái-niệm)
  - [Các thành phần](#các-thành-phần)
- [Nguyên lý hoạt động](#nguyên-lý-hoạt-động)
- [Các cách thức giao tiếp](#các-cách-thức-giao-tiếp)
  - [RESTful API (HTTP)](#restful-api-http)
  - [S3 API](#s3-api)
  - [Swift API](#swift-api)
- [Sử dụng Object Storage](#sử-dụng-object-storage)

# Giới thiệu

## Khái niệm

**Object Storage** (lưu trữ đối tượng) là một kiến trúc lưu trữ dữ liệu, quản lý dữ liệu dưới dạng các đối tượng (**object**), khác biệt hoàn toàn so với các kiểu lưu trữ truyền thống như lưu trữ tệp (file storage) hay lưu trữ khối (block storage). Thay vì tổ chức dữ liệu theo cây thư mục, Object Storage lưu trữ mọi thứ như một “đối tượng” riêng biệt.

Mỗi **object** bao gồm chính **dữ liệu** đó (ví dụ: một file ảnh, một video, một tài liệu) và **metadata** (thông tin mô tả về dữ liệu, ví dụ: tên file, ngày tạo, loại file, kích thước). Điểm đặc biệt là mỗi object có một **định danh duy nhất** (identifier hay key), giống như “chứng minh thư” của dữ liệu vậy. Định danh này giúp truy cập dữ liệu một cách nhanh chóng.

## Các thành phần

Object Storage, về cơ bản, được cấu thành từ ba thành phần chính: **Object** (đối tượng), **Bucket** (thùng chứa), và **Identifier/Key** (định danh/khóa)

### **Object (Đối tượng)**

Object là đơn vị dữ liệu cơ bản nhất trong hệ thống. Mỗi object có thể là bất kỳ loại file nào: hình ảnh, video, tài liệu, hay mã nguồn. Quan trọng hơn, mỗi object đều có kèm **metadata** – tức là các thông tin mô tả như tên file, loại file, thời gian tạo, kích thước… giúp quản lý và tìm kiếm dễ dàng. Object Storage không quan tâm nội dung bên trong file là gì – mọi thứ chỉ là “đối tượng” dữ liệu.

### **Bucket (Thùng chứa)**

Bucket là không gian lưu trữ dùng để chứa các object. Có thể hình dung nó như một “thùng chứa lớn” không có giới hạn số lượng object. Không giống thư mục trong ổ đĩa, bucket không có cấu trúc phân cấp mà lưu các object ở cùng một mức. Mỗi bucket có một tên duy nhất trên toàn hệ thống và thường đi kèm region (khu vực địa lý) khi khởi tạo, ảnh hưởng đến tốc độ truy cập và chi phí.

### **Identifier/Key (Định danh hoặc Khóa)**

Mỗi object trong một bucket được gán một định danh duy nhất gọi là key. Key đóng vai trò như “địa chỉ” của object và được dùng để truy xuất dữ liệu. Ví dụ: `https://my-bucket.s3.amazonaws.com/my-image.jpg` – trong đó `my-image.jpg` là key.  có thể tự đặt key hoặc để hệ thống tự sinh ra.

# Nguyên lý hoạt động

Object Storage hoạt động bằng cách lưu dữ liệu thành từng object riêng biệt, mỗi object có một định danh duy nhất và được lưu trữ trong một bucket. Khi  upload một file lên,  gửi nó cùng metadata đến hệ thống. Hệ thống sẽ lưu nó thành object và gán cho một key. Việc truy cập sau này chỉ cần dùng đúng key là được.

Khác với hệ thống file truyền thống có thư mục cha – con, Object Storage loại bỏ cấu trúc phân cấp, giúp hệ thống linh hoạt hơn, dễ mở rộng và có thể xử lý hàng tỷ object mà không giảm hiệu năng.

Việc truy xuất hay xóa object đều thực hiện qua **API HTTP**. Muốn lấy dữ liệu,  gửi request kèm key; muốn xóa, cũng chỉ cần key. Tuy nhiên,  không thể chỉnh sửa nội dung object – nếu cần thay đổi,  phải upload lại object mới.

Cuối cùng, để đảm bảo độ bền dữ liệu, Object Storage thường sử dụng kiến trúc phân tán – lưu nhiều bản sao trên nhiều máy chủ. Điều này giúp  yên tâm rằng dữ liệu luôn có thể phục hồi nếu có phần cứng gặp lỗi. Ngoài ra, các dịch vụ Object Storage còn hỗ trợ tính năng quản lý phiên bản, kiểm soát truy cập, mã hóa, và tự động xóa file sau thời gian định trước (lifecycle).

# **Các cách thức giao tiếp**

## **RESTful API (HTTP)**

Nó dựa trên các phương thức HTTP chuẩn như GET, PUT, POST, DELETE, HEAD để thực hiện các thao tác với object. RESTful API được ưa chuộng vì tính đơn giản, dễ sử dụng, và khả năng tương thích cao với các nền tảng và ngôn ngữ lập trình khác nhau.

Ví dụ, để tải lên một object, ứng dụng sẽ sử dụng phương thức PUT. Để tải xuống một object, ứng dụng sẽ sử dụng phương thức GET. Để xóa một object, ứng dụng sẽ sử dụng phương thức DELETE. 

## **S3 API**

**S3 API** là một giao thức do Amazon Web Services (AWS) phát triển cho dịch vụ Amazon S3. Nó đã trở thành một tiêu chuẩn *de facto* trong ngành công nghiệp Object Storage. Nhiều dịch vụ Object Storage khác, bao gồm cả các giải pháp private cloud, cũng hỗ trợ S3 API để đảm bảo tính tương thích. Về cơ bản, S3 API là một dạng của RESTful API.

## **Swift API**

**Swift API** là một giao thức khác, được phát triển bởi OpenStack cho dự án Object Storage của họ (còn gọi là Swift). Swift API cũng dựa trên RESTful principles và sử dụng HTTP, nhưng có một số điểm khác biệt so với S3 API về cách tổ chức dữ liệu và các thao tác cụ thể. Swift API ít phổ biến hơn S3 API.

# Sử dụng Object Storage

Trong thực tế, đối với một web-app có số lượng file tĩnh (hình ảnh, video, log,...) lớn có thể sử dụng Object Storage để giải quyết nhiều vấn đề quan trọng về lưu trữ và phân phối nội dung

Như ví dụ sau đây, mình sẽ ây dựng hệ thống upload ảnh cho ứng dụng chat hoặc social media.

### Luồng hoạt động chính

`Frontend/Mobile App → Backend API → n8n Workflow → MinIO/S3`

### **Luồng xử lý chi tiết**

**Client gửi ảnh**:

- User chọn ảnh từ thiết bị
- Frontend convert ảnh thành base64
- Gửi POST request đến backend API

**Backend xử lý**:

- Nhận base64 image từ client
- Validate format và size
- Forward request đến n8n webhook endpoint

**n8n Workflow**:

![image.png](/Images/tuan_7_objectstorage/image.png)

- Nhận base64 data
- Convert thành binary format
- Generate unique filename (ví dụ: `chat-1754533573355.jpg`)
- Upload lên Object Storage với structured path (`thiendh/chat-xxx.jpg`)

Đây là cấu hình Webhook Node để nhận request từ Backend:

![image.png](/Images/tuan_7_objectstorage/image%201.png)

Đây là cấu hình của Function Node để xử lý hình ảnh dạng base64 sang dạng binary:

![image.png](/Images/tuan_7_objectstorage/image%202.png)

Cần tạo S3 account để sử dụng trong S3 Node:

![image.png](/Images/tuan_7_objectstorage/image%203.png)

![image.png](/Images/tuan_7_objectstorage/image%204.png)

![image.png](/Images/tuan_7_objectstorage/image%205.png)

![image.png](/Images/tuan_7_objectstorage/image%206.png)

Đây là cấu hình S3 Node để upload ảnh lên Minio Object Storage: 

Chọn S3 account vữa vào tạo ở bước trên → chọn cấu hình như trong ảnh (cần thay dữ liệu phù hợp với thông tin của bạn)

![image.png](/Images/tuan_7_objectstorage/image%207.png)

Sau khi cấu hình đầy đủ như trên, mình có thể chạy workflow này liên tục mà không cần nhấn **Excute workflow** bằng cách bật nút bên trên 

![image.png](/Images/tuan_7_objectstorage/image%208.png)

Giờ chỉ cần gọi api đã khai báo trong webhook với phương thức POST và truyền image theo cùng thôi

![image.png](/Images/tuan_7_objectstorage/image%209.png)

Đây là ví dụ đoạn code NodeJs gọi api trên:

![image.png](/Images/tuan_7_objectstorage/image%2010.png)

Có thể kiểm tra lại xem workflow chạy lỗi hay không ở ở tab **Executions**

![image.png](/Images/tuan_7_objectstorage/image%2011.png)

Lên Minio Object Storage xem đã lưu ảnh thành công hay chưa:

![image.png](/Images/tuan_7_objectstorage/image%2012.png)

Nếu như kết quả hiển thị như hình trên thì mình đã thực hành xong 1 ví dụ nhỏ về cách sử dụng dụng Object Storage kết hợp với n8n để có thể upload hình ảnh của một web-app chat.

---
THE END