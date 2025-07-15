# Tìm hiểu về HTTP

# Mục lục

  - [1. Tìm hiểu tổng quan](#1-tìm-hiểu-tổng-quan)
    - [Giới thiệu](#giới-thiệu)
    - [Đặc trưng cơ bản HTTP](#đặc-trưng-cơ-bản-http)
    - [Cấu trúc của HTTP](#cấu-trúc-của-http)
    - [Cách hoạt động của HTTP](#cách-hoạt-động-của-http)
    - [Sự khác biệt giữa HTTP và HTTPS](#sự-khác-biệt-giữa-http-và-https)
  - [2. HTTP Request và Response](#2-http-request-và-response)
    - [HTTP Request](#http-request)
    - [HTTP Response](#http-response)
  - [3. Thực hành sử dụng công cụ](#3-thực-hành-sử-dụng-công-cụ)
    - [Sử dụng DevTools](#sử-dụng-devtools)
    - [Sử dụng curl](#sử-dụng-curl)
    - [Sử dụng wget](#sử-dụng-wget)

## 1. Tìm hiểu tổng quan

### Giới thiệu

**HTTP (HyperText Transfer Protocol)** được biết đến như một giao thức truyền tải các siêu văn bản. Nó được dùng để liên hệ thông tin giữa Máy cung cấp dịch vụ (Web server) và Máy sử dụng dịch vụ (Web client), là giao thức Client/Server dùng cho World Wide Web – WWW

**HTTP** là một giao thức tầng ứng dụng của bộ giao thức **TCP/IP** (các giao thức nền tảng cho Internet).

**HTTPS** là giao thức HTTP có cài đặt chứng chỉ bảo mật **SSL**. Việc tích hợp thêm chứng chỉ mang lại cả ưu và nhược điểm

### Đặc trưng cơ bản HTTP

**Kết nối không liên tục:** Kết nối của HTTP không liên tục. Qua trình xử lý, phản hồi yêu cầu thông thường của HTTP là CLient  tạo yêu cầu _> dừng kết nối với Server để đợi phản hồi → Server tiến hành xử lý yêu cầu → thiết lập kết nối tới Client à gửi phản hồi

**Độc lập:**  Bạn có thể gửi mọi loại dữ liệu qua HTTP miễn sao máy chủ và Client có biện pháp kiểm soát các nội dung của dữ liệu. Client và Server cần xác định nội dung gửi đi thuộc kiểu gì để lựa chọn MIME phù hợp.

> ***MIME (Multipurpose Internet Mail Extensions)** là một chuẩn định dạng được dùng để xác định kiểu dữ liệu (content type) trong giao tiếp qua HTTP (và ban đầu là trong email).*
> 

**HTTP là stateless:** Là connectionless nên đặc trưng thứ ba của HTTP là Stateless. Máy chủ và Client chỉ biết nhau trong yêu cầu của hiện tại, ngay sau đó, chúng sẽ quên. Ngoài ra, Cả máy khách và server đều có thể lưu lại thông tin về những yêu cầu giữ các website. 

### Cấu trúc của HTTP

**Cấu trúc của HTTP** bao gồm 2 đối tượng là Client và Server. Có thể coi HTTP như giao thức gửi các yêu cầu và phản hồi giữa Client - Server. Tại giao thức này, mọi thiết bị tìm kiếm hay trình duyệt web sẽ đóng vai trò như máy khách, còn máy chủ web có vai trò như Server.

- **Client**: Client (máy khách) gửi yêu cầu cụ thể đến Server theo mẫu phương thức yêu cầu -> Các phiên bản giao thức cùng với URI gửi thông báo MIME (gồm thông tin máy khách, nội dung của đối tượng, bộ chỉnh sửa) đến server qua kết nối TCP/IP.
- **Server**: Server nhận được yêu cầu -> Phản hồi lại bằng một dòng trạng thái qua thông báo MIME có chứa thông tin máy chủ, thông tin về nội dung của đối tượng và thực thể của đa phương tiện.

### Cách hoạt động của HTTP

HTTP hoạt động theo nguyên lý: Client khởi tạo yêu cầu (tạo kết nối TCP tới cổng 80 hoặc một cổng khác trên server) -> server nhận yêu cầu -> gửi lại trạng thái đến cho client kèm theo thông điệp (thông điệp ở đây thường sẽ là thông tin yêu cầu, thông báo lỗi hay thông tin khác).

Sau khi phiên giao dịch hoàn thành, kết nối HTTP sẽ tự động bị đóng do nó là một stateless system.

![giao-thuc-http.jpg](/Images/tuan_3_http/giao-thuc-http.jpg)

Ngoài ra, khi các hệ thống trao đổi dữ liệu với nhau, chúng cũng sử dụng giao thức này nhưng 2 bên đều là server.

### **Sự khác biệt giữa HTTP và HTTPS**

Điểm khác nhau cơ bản giữa HTTP và HTTPS là sự xuất hiện của chứng chỉ bảo mật SSL. Cài đặt thêm chứng chỉ sẽ tăng khả năng bảo mật của website, giúp trang web được đánh giá cao hơn bởi cả công cụ tìm kiếm lẫn người dùng đồng thời việc Seo web sẽ hiệu quả hơn.

Tuy vậy, nó có nhược điểm là làm giảm tốc độ của việc tải trang. Các website sử dụng giao thức HTTPS sẽ mất nhiều thời gian tải trang hơn so với việc dùng HTTP. Dù vậy, bạn vẫn nên cài đặt SSL cho giao thức HTTP để bảo vệ dữ liệu cũng như thông tin khách hàng. Chứng chỉ sẽ giúp trang web của bạn tránh được các rủi ro từ phía hacker.

## 2. HTTP Request và Response

### HTTP Request

Một HTTP client (máy khách) gửi một HTTP request (yêu cầu) lên server (máy chủ) nhờ một thông điệp có định dạng như sau:

```
<method> <request-URL> <http-serverion>
<headers>
<body>
```

Ví dụ:

![http-request.jpg](/Images/tuan_3_http/http-request.jpg)

**1. Request Line**

Bắt đầu của HTTP Request sẽ là dòng Request-Line bao gồm 3 thông tin: *Method, Request URL, Request header, HTTP version*

**Method**

Báo cho Server rằng hành động sẽ phải xử lý với thông tin được gửi từ client lên

![cac-phuong-thuc-http.jpg](/Images/tuan_3_http/cac-phuong-thuc-http.jpg)

***Phương thức `GET`***

- Câu truy vấn thường được đính kèm vào đường dẫn HTTP request. Ví dụ: `https://thienduong.com**/get-product-id/2**`
- GET request có thể được cached, bookmark và lưu trong lịch sử của trình duyệt mà bị giới hạn về chiều dài (chiều dài của URL là có hạn).
- Lưu ý: Bạn không nên dùng GET request với dữ liệu quan trọng bởi vì khi gửi dữ liệu về server thì nội dung gửi đi sẽ hiện trên đường đẫn URL mà chỉ dùng để nhận dữ liệu, không có tính bảo mật.

***Phương thức `POST`***

- Câu truy vấn sẽ được gửi trong phần message body của HTTP request.
- POST không thể, cached, bookmark hay lưu trong lịch sử trình duyệt và cũng không bị giới hạn về độ dài.

***Phương thức `PUT`***

- Dữ liệu được gửi trong phần message body, giống như POST.
- Thường được dùng để cập nhật toàn bộ tài nguyên (ví dụ như chỉnh sửa thông tin người dùng, sản phẩm...).
- Idempotent (tính định danh): Gửi nhiều lần cùng một yêu cầu PUT với cùng dữ liệu → kết quả vẫn như nhau.
- Không được cache, không lưu trong lịch sử của trình duyệt.
- Thường yêu cầu xác thực, vì có thể ghi đè dữ liệu.

***Phương thức `DELETE`***

- Được dùng để xóa tài nguyên trên server.
- Gửi yêu cầu xóa qua URL (có thể có body nhưng thường không cần).
- Cũng là idempotent: Gửi nhiều lần → nếu lần đầu xóa thành công, các lần sau không thay đổi trạng thái.
- Không thể cache, không bookmark, không lưu trong lịch sử trình duyệt.
- Nhạy cảm vì thao tác xóa dữ liệu nên cần xác thực & phân quyền chặt chẽ.

**2. Request URL**

Một **URL** (*Uniform Resource Locator*) được sử dụng để xác định duy nhất một tài nguyên trên Web. Một URL có cấu trúc như sau:

`protocol://hostname:port/path-and-file-name`

Trong một **URL** có 4 thành phần:

- **Protocol**: giao thức tầng ứng dụng được sử dụng bởi client và server
- **Hostname**: tên DNS domain
- **Port**: Cổng TCP để server lắng nghe request từ client
- **Path-and-file-name**: Tên và vị trí của tài nguyên yêu cầu.

**3. HTTP version**

HTTP version là Phiên bản giao thức HTTP đang sử dụng.

### HTTP Response

HTTP response là dữ liệu trả về từ server sang client, trong đó sẽ có các trường thông tin mà request yêu cầu

Định dạng gói tin HTTP response cũng gồm 3 phần chính là: Status line, Header và Body.

Ví dụ:

![An-HTTP-response-message.jpg](/Images/tuan_3_http/An-HTTP-response-message.jpg)

**1. Response Status:**

Gồm 3 trường là phiên bản giao thức (HTTP version), mã trạng thái (Status code) và mô tả  trạng thái (Status text):

- Phiên bản giao thức (HTTP version): phiên bản của giao thức HTTP mà server hỗ trợ, thường là HTTP/1.0 hoặc HTTP/1.1
- Mã trạng thái (Status code): mô tả trạng thái kết nối dưới dạng số, mỗi trạng thái sẽ được biểu thị bởi một số nguyên. Ví dụ: 200, 404, 302,…
- Mô tả trạng thái (Status text): mô tả trạng thái kết nối dưới dạng văn bản một cách ngắn gọn, giúp người dùng dễ hiểu hơn so với mã trạng thái. Ví du: 200 OK, 404 Not Found, 403 Forbiden,…

Một số loại thông dụng mà server trả về cho client như sau:

| **Nhóm mã** | **Mã** | **Tên trạng thái** | **Ý nghĩa ngắn gọn** |
| --- | --- | --- | --- |
| **1xx** – Informational |  | *Thông tin tạm thời* | Client thường không cần quan tâm |
| **2xx** – Success | 200 | OK | Yêu cầu thành công |
|  | 202 | Accepted | Đã nhận, xử lý sau |
|  | 204 | No Content | Thành công, không có nội dung trả về |
|  | 205 | Reset Content | Như 204, nhưng yêu cầu client reset lại view |
|  | 206 | Partial Content | Trả về một phần tài nguyên (theo range) |
| **3xx** – Redirection | 301 | Moved Permanently | Chuyển hướng vĩnh viễn |
|  | 302 | Found (Moved Temporarily) | Chuyển hướng tạm thời |
|  | 303 | See Other | Chuyển hướng với GET khác URL |
|  | 304 | Not Modified | Dữ liệu chưa thay đổi → dùng cache |
| **4xx** – Client Error | 400 | Bad Request | Request sai cú pháp |
|  | 401 | Unauthorized | Cần xác thực |
|  | 403 | Forbidden | Không có quyền truy cập |
|  | 404 | Not Found | Không tìm thấy tài nguyên |
|  | 405 | Method Not Allowed | Phương thức không được hỗ trợ |
| **5xx** – Server Error | 500 | Internal Server Error | Lỗi xử lý nội bộ server |
|  | 501 | Not Implemented | Chưa hỗ trợ chức năng yêu cầu |
|  | 503 | Service Unavailable | Server quá tải hoặc đang bảo trì |

**2. Response Header**

Header của gói tin response có chức năng tương tựn như gói tin request, giúp server có thể truyền thêm các thông tin bổ sung đến client dưới dạng các cặp *“Name:Value”*.

**3. Response Body**

Là nơi đóng gói dữ liệu để trả về cho client, thông thường trong duyệt web thì dữ liệu trả về sẽ ở dưới dạng một trang HTML để trình duyệt có thể thông dịch được và hiển thị ra cho người dùng.

Hoặc trả về dạng JSON, XML khi giao tiếp bằng API.

## 3. Thực hành sử dụng công cụ

### Sử dụng DevTools

 1.Các mở giao điện DevTools

Để mở giao diện DevTools trên hầu hết các trình duyệt chúng ta có thể thao tác như sau:

- Nhấn `chuột phải` hoặc phím `F12`(đối với laptop thi là tổ hợp phím `Fn + F12`) **→** nhấn **Inspect** (kiểm tra) → giao diện **DevTools** sẽ mở ra

![image.png](/Images/tuan_3_http/image.png)

![image.png](/Images/tuan_3_http/image%201.png)

Nhìn chung có 8 nhóm Tool chính là có sẵn để view Developer Tools:

- *Elements:* cung cấp giao diện đồ họa để bạn có thể xem và chỉnh sửa HTML và CSS của trang web, bạn có thể kiểm tra cấu trúc DOM
    
    ![image.png](/Images/tuan_3_http/image%202.png)
    

- *Console: Nó cho phép lập trình viên in ra thông tin, cảnh báo, và lỗi từ mã nguồn.*
    
    ![image.png](/Images/tuan_3_http/image%203.png)
    

- *Network*: Tab Network giúp theo dõi tất cả các yêu cầu mạng mà trang web gửi và nhận. Điều này bao gồm các yêu cầu HTTP/HTTPS, các tệp tĩnh như hình ảnh và JavaScript, cũng như các dữ liệu API
    
    ![image.png](/Images/tuan_3_http/image%204.png)
    

       Nhấn vào 1 dòng bất thì thì thông tin ta có thể thấy:

- URL
- Phương thức (GET, POST)
- Mã phản hồi (Status code)
- Header, cookie, query string, body request/response
    
    ![image.png](/Images/tuan_3_http/image%205.png)
    

- *Performance:* giúp phân tích hiệu suất của trang web, bao gồm thời gian thực hiện JavaScript, tải trang, và các yếu tố khác ảnh hưởng đến tốc độ trang
    
    ![image.png](/Images/tuan_3_http/image%206.png)
    
- *Memory:* cung cấp các công cụ để phân tích và xử lý bộ nhớ
    
    ![image.png](/Images/tuan_3_http/image%207.png)
    
- *Application:* cho phép quản lý các tài nguyên của ứng dụng web, bao gồm Local Storage, Session Storage, Cookies, và các tài nguyên khác.
    
    ![image.png](/Images/tuan_3_http/image%208.png)
    

### Sử dụng curl

### Cách cài đặt `curl`

Trên hầu hết các hệ điều hành Linux, `curl` thường đã được cài đặt sẵn. Nếu không, bạn có thể dễ dàng cài đặt nó với các lệnh phù hợp với hệ điều hành của mình:

```bash
# Đối với hệ điều hành Ubuntu/Debian:
sudo apt-get install curl

# Đối với hệ điều hành CentOS/Fedora:
sudo yum install curl

# Đối với macOS:
brew install curl
```

### Tải một tập tin đơn giản bằng `curl`

Cú pháp cơ bản để tải một tập tin bằng `curl` như sau:

```bash
curl -O <URL>
```

- `O` sẽ lưu tập tin với tên giống như tên tập tin trên máy chủ.

Ví dụ:

```bash
curl -O https://httpbin.org/image/png
```

![image.png](/Images/tuan_3_http/image%209.png)

### Sử dụng `curl` để lấy dữ liệu

```bash
curl -X GET "URL của endpoint" -H "HTTP header"
```

Ví dụ:

![image.png](/Images/tuan_3_http/image%2010.png)

### Sử dụng `curl` để gửi dữ liệu

```bash
curl -X POST "URL của endpoint" -d "data"
```

Ví dụ:

![image.png](/Images/tuan_3_http/image%2011.png)

### **Sử dụng curl để xóa dữ liệu**

```bash
curl -X DELETE "URL của endpoint" -H "HTTP header"
```

Ví dụ:

![image.png](/Images/tuan_3_http/image%2012.png)

### Sử dụng wget

### **Cách cài đặt `wget`**

Thường 

Trên nhiều hệ điều hành Linux, `wget` thường đã có sẵn. Nếu chưa, bạn có thể cài đặt như sau:

```bash
# Đối với Ubuntu/Debian:
sudo apt-get install wget

# Đối với CentOS/Fedora:
sudo yum install wget

# Đối với macOS (qua Homebrew):
brew install wget
```

### **Tải một tập tin đơn giản bằng `wget`**

```bash
wget <URL>
```

Mặc định `wget` sẽ tải tập tin và lưu cùng tên như trên máy chủ.

Ví dụ:

![image.png](/Images/tuan_3_http/image%2013.png)

## **Sử dụng `wget` để lấy dữ liệu**

`wget` mặc định là gửi **GET** request:

```bash
wget "URL enpoint"
```

Nếu muốn thêm header:

```bash
wget --header="Accept: application/json" "URL enpoint"
```

Ví dụ:

![image.png](/Images/tuan_3_http/image%2014.png)

### **Sử dụng `wget` để gửi dữ liệu**

```bash
wget --method=POST --body-data="name=Thanh" <URL enpoint>
```

📌 Có thể thêm header nếu cần:

```bash
wget --method=POST --body-data="name=Thanh" \
     --header="Content-Type: application/x-www-form-urlencoded" \
     https://httpbin.org/post
```

![image.png](/Images/tuan_3_http/image%2015.png)

### **Sử dụng `wget` để xóa dữ liệu**

```bash
wget --method=DELETE "URL enpoint"
```

📌 Nếu cần thêm header:

```bash
wget --method=DELETE --header="Accept: application/json" 
       <URL enpoint>
```

![image.png](/Images/tuan_3_http/image%2016.png)

---
THE END