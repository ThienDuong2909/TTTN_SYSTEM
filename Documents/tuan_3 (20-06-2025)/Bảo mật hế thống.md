# Bảo mật hệ thống

# Mục lục
  - [1. Tổng quan về Bảo mật](#tổng-quan-về-bảo-mật)
  - [2. Tìm hiểu về SSH](#tìm-hiểu-về-ssh)
    - [Giới thiệu](#giới-thiệu)
    - [Các Tính Năng Chính Của SSH](#các-tính-năng-chính-của-ssh)
    - [Cài đặt SSH](#cài-đặt-ssh)
    - [Bảo mật SSH](#bảo-mật-ssh)
  - [3. Tìm hiểu về SSL](#tìm-hiểu-về-ssl)
    - [Giới thiệu](#giới-thiệu-1)

Bảo mật hệ thống là quá trình bảo vệ tài nguyên hệ thống (máy chủ, ứng dụng, dữ liệu) khỏi các truy cập trái phép, tấn công mạng, và các mối đe dọa bảo mật. Một số nguyên tắc cơ bản:

- **Tính bí mật (Confidentiality):** Đảm bảo dữ liệu chỉ được truy cập bởi những người có quyền.
- **Tính toàn vẹn (Integrity):** Bảo vệ dữ liệu khỏi bị thay đổi trái phép.
- **Tính sẵn sàng (Availability):** Đảm bảo hệ thống và dữ liệu luôn sẵn sàng cho người dùng hợp pháp.
- **Xác thực (Authentication):** Xác minh danh tính người dùng hoặc hệ thống.
- **Ủy quyền (Authorization):** Kiểm soát quyền truy cập vào tài nguyên.

Các mối đe dọa phổ biến: tấn công brute force, khai thác lỗ hổng, đánh cắp dữ liệu, DDoS, v.v.

## 2. Tìm hiểu về SSH

### Giới thiệu

**SSH (Secure Shell)** là một giao thức mạng cho phép người dùng kết nối và quản lý các máy tính từ xa một cách an toàn. Đặc biệt, trong hệ điều hành Linux, SSH được sử dụng phổ biến để điều khiển các máy chủ và thực hiện các tác vụ quản trị hệ thống mà không cần phải có mặt trực tiếp tại máy chủ.

### **Các Tính Năng Chính Của SSH**

- **Kết nối từ xa an toàn:** SSH giúp kết nối giữa máy client và server qua một kênh mã hóa, bảo vệ thông tin đăng nhập và các lệnh thực thi.
- **Quản lý hệ thống:** Qua SSH, người quản trị có thể thực hiện mọi tác vụ quản lý hệ thống như cài đặt phần mềm, thay đổi cấu hình hoặc sao lưu dữ liệu.
- **Chuyển tệp an toàn:** SSH không chỉ giúp điều khiển máy từ xa mà còn hỗ trợ chuyển tệp tin qua giao thức SCP hoặc SFTP, đảm bảo an toàn cho dữ liệu trong suốt quá trình truyền tải.

### Cài đặt SSH

Đầu tiên, ta cần cài đặt gói OpenSSH Server trên hệ thống Linux để có thể sử dụng SSH. Tùy thuộc vào bản phân phối Linux bạn đang sử dụng, có các bước cài đặt khác nhau.

- Ở đây mình thực sử dụng Ubuntu 20.04 thì sẽ thực hiện lệnh sau:
    
    ```
    sudo apt update
    sudo apt install openssh-server
    ```
    
    ![image.png](/Images/tuan_3_bmht/image.png)
    

- Sau khi cài đặt thành công, ta cần khởi động dịch vụ:
    
    ```
    sudo systemctl start sshd
    ```
    

- Có thể kiểm tra lại trạng thái dịch vụ:
    
    ```
    sudo systemctl status sshd
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%201.png)
    

- Đảm bảo SSH tự động khởi động cùng hệ thống bằng cách chạy lệnh sau:
    
    ```
    sudo systemctl enable sshd
    ```
    

- Thử sử dụng lệnh SSH từ 1 máy khác vào máy vừa cài đặt SSH:
    
    ![image.png](/Images/tuan_3_bmht/image%202.png)
    

Tới bước này thì chúng ta đã hoàn thành cài đặt và sử dụng cơ bản SSH rồi.

### Bảo mật SSH

- **Sử dụng cổng SSH tùy chỉnh**
    
    Mặc định, SSH lắng nghe trên cổng 22 nên  thay đổi cổng này giúp giảm khả năng bị tấn công bằng các phương pháp quét cổng tự động.
    
    **Cách thay đổi cổng SSH:** 
    
    Mở tệp cấu hình SSH `/etc/ssh/sshd_config` và thay đổi dòng `Port` thành một cổng khác (ví dụ đổi thành 2223):
    
    ![image.png](/Images/tuan_3_bmht/image%203.png)
    
    Lưu và khởi động lại dịch vụ SSH:
    
    ```
    sudo systemctl restart sshd
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%204.png)
    
    Nếu như có sử dụng tường lửa thì ta cần cho phép giao tiếp qua port  mà ta vừa thay đổi cũng như chặn port mặc định  của ssh (port 22) lại vì không còn sử dụng nữa:
    
    ![image.png](/Images/tuan_3_bmht/image%205.png)
    
    ![image.png](/Images/tuan_3_bmht/image%206.png)
    
    Sau khi thực hiện các bước thay đổi port tùy chọn trên, muốn dùng lệnh SSH để truy cập vào máy chủ thì thực hiện lệnh sau:
    
    ```
    ssh -p <PORT> username@hostname
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%207.png)
    
- **Sử dụng Key -bases authentication**
    
    Để nâng cao bảo mật, thay vì cho phép đăng nhập qua mật khẩu, bạn có thể yêu cầu người dùng sử dụng khóa SSH. 
    
    Việc sử dụng khóa SSH sẽ bảo vệ hệ thống khỏi các cuộc tấn công brute-force, vốn nhắm vào mật khẩu yếu.
    
    **Bước 1: Tạo cặp khóa SSH**
    
    Người dùng cần tạo một cặp khóa SSH (khóa công khai và khóa riêng tư) trên máy yêu cầu liên kết (ví dụ máy thật muốn ssh vào máy ảo trong Virtualbox) bằng lệnh:
    
    ```
    ssh-keygen -t rsa -b 4096
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%208.png)
    
    Khi thực hiện lệnh trên, ta sẽ phải chọn nơi mà key lưu trữ, nếu nhấn `enter`thì sẽ chọn mặc định là `C:\Users\User\.ssh\id_rsa…` . Ở trường hợp này mình sẽ lưu key vào `C:\Users\User\.ssh\id_rsa_th29k03`
    
    **Bước 2: Chép public key lên máy chủ (máy ảo Ubuntu)**
    Giả sử IP server là `192.168.0.100`, user là `th29k03`, port SSH là `2223` và key được lưu ở `C:\Users\User\.ssh\id_rsa_th29k03`
    
    Dùng `ssh-copy-id` trên Windows (nếu có Git Bash hoặc WSL), ở đây tôi chạy bằng Git lệnh như sau:
    
    ```
    ssh-copy-id -i ~/.ssh/id_rsa_th29k03.pub tk29k03@192.168.0.100
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%209.png)
    
    Sau khi thực thi xong lệnh trên ta tiếp tục chạy lệnh:
    
    ```
    ssh -i C:/Users/User/.ssh/id_rsa_th29k03 -p 2223 th29k03@192.168.0.100
    ```
    
    Kiểm tra bên máy chủ (máy ảo Ubuntu xem có) có file `.ssh/authorized_key` bằng  tác như dưới:
    
    ![image.png](/Images/tuan_3_bmht/image%2010.png)
    
    Vậy là đã xong phần tạo Key và chép Key qua máy chủ (máy ảo Ubuntu).
    
    **Bước 3: Cấu hinh SSH chỉ cho phép xác thực bằng Key.**
    
    Trên Ubuntu, mở file cấu hình SSH:
    
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Tìm và sửa các dòng sau:
    
    ```
    PasswordAuthentication no   //không cho đăng nhập bằng mật khẩu
    PubkeyAuthentication yes    //cho đang nhập bằng Public Key
    ```
    
    ![image.png](/Images/tuan_3_bmht/image%2011.png)
    
    Sau đó khởi động lại :
    
    ```bash
    sudo systemctl restart ssh
    ```
    
    Kiểm tra lại bằng tài khoản chưa có Key:
    
    ![image.png](/Images/tuan_3_bmht/image%2012.png)
    
    Nếu như hiện thông báo Permission denied (publickey)  như trên là chũng ta đã tắt tính năng đăng nhập bằng mật khẩu thành công.
    
    Tiếp theo ta đăng nhập bằng tài khoản mới tạo Key trước đó:
    
    ![image.png](/Images/tuan_3_bmht/image%2013.png)
    
    Nếu như khi đăng nhập mà không cần nhập mật khẩu của tài khoản do đã tạo Key trước đó thì ta đã thành công bảo mật SSH bằng cách  ****sử dụng Key 
    

## 3. Tìm hiểu về SSL

### Giới thiệu

**SSL**

**SSL (Secure Sockets Layer)** là một giao thức bảo mật giúp mã hóa dữ liệu truyền tải giữa trình duyệt của người dùng và máy chủ của website. Chứng chỉ SSL là một tệp kỹ thuật số được cấp phát bởi các tổ chức chứng thực (Certificate Authority - CA), dùng để xác minh danh tính của website và kích hoạt giao thức HTTPS (Hypertext Transfer Protocol Secure).