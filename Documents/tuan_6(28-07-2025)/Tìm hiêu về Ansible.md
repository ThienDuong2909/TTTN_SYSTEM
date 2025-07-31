# Tìm hiểu về Ansible

# Mục lục

- [Tìm hiểu về Ansible](#tìm-hiểu-về-ansible)
- [Giới thiệu về Ansible](#giới-thiệu-về-ansible)
  - [Khái niệm](#khái-niệm)
  - [Các tính năng chính](#các-tính-năng-chính)
  - [Cách thức hoạt động](#cách-thức-hoạt-động)
- [Cài đặt và cấu hình Ansible](#cài-đặt-và-cấu-hình-ansible)
- [Thực hành ứng dụng Ansible](#thực-hành-ứng-dụng-ansible)
  - [Thực hành cài đặt LEMP Stack với Ansible](#thực-hành-cài-đặt-lemp-stack-với-ansible)
  - [Thực hành cài đặt LAMP Stack với Ansible](#thực-hành-cài-đặt-lamp-stack-với-ansible)
# Giới thiệu về Ansible

## Khái niệm

**Ansible** là một công cụ mã nguồn mở, được phát triển để tự động hóa các tác vụ trong hệ thống công nghệ thông tin. Được sử dụng phổ biến trong việc quản lý cấu hình, triển khai ứng dụng và tự động hóa các tác vụ hành chính, Ansible cho phép người dùng tự động hóa các công việc phức tạp và giảm thiểu lỗi do thao tác thủ công.

Điều đặc biệt về Ansible là khả năng **không cần cài đặt agent** trên các máy chủ từ xa, điều này làm cho nó trở thành một công cụ nhẹ nhàng và dễ dàng triển khai. Ansible sử dụng giao thức **SSH** (cho Linux/Unix) hoặc **WinRM** (cho Windows) để kết nối và thực thi các tác vụ trên các hệ thống từ xa.

**Inventory (Danh Sách Máy Chủ)**

Trong Ansible, **Inventory** là danh sách các máy chủ hoặc nút mà bạn muốn quản lý. Inventory có thể được định nghĩa tĩnh (trong một tệp tin cấu hình) hoặc động (sử dụng dịch vụ đám mây như AWS).

**Playbook**

Playbook là một tệp tin **YAML** chứa các bước (plays) mà Ansible sẽ thực hiện. Mỗi play có thể chứa nhiều tác vụ (tasks), mỗi tác vụ sẽ thực hiện một công việc cụ thể trên các máy chủ mục tiêu.

**Roles (Vai Trò)**

Ansible hỗ trợ **Roles**, một cách để tổ chức các tác vụ thành các phần có thể tái sử dụng. Điều này giúp cấu trúc playbook trở nên rõ ràng và dễ bảo trì.

**Mô-đun**

Mỗi tác vụ trong playbook sẽ sử dụng một mô-đun cụ thể của Ansible để thực hiện công việc. Ví dụ, mô-đun yum dùng để cài đặt phần mềm trên hệ thống sử dụng **Red Hat** hoặc **CentOS**, trong khi mô-đun service dùng để quản lý trạng thái của các dịch vụ.

**Idempotent (Không Thay Đổi Nếu Không Cần Thiết)**

Ansible đảm bảo tính **idempotent**, nghĩa là nếu bạn chạy lại một playbook nhiều lần, kết quả cuối cùng sẽ không thay đổi trừ khi có sự thay đổi thực sự trong hệ thống.

## Các tính năng chính

Ansible sở hữu nhiều tính năng nổi bật giúp tự động hóa quản lý hệ thống và các tác vụ hành chính. Dưới đây là một số tính năng chính:

- **Quản Lý Cấu Hình**

Ansible cho phép bạn quản lý cấu hình của nhiều máy chủ đồng thời, giúp đảm bảo rằng tất cả các hệ thống đều có cùng một cấu hình chính xác. Bạn có thể dễ dàng cài đặt phần mềm, cấu hình hệ thống và đảm bảo tính nhất quán trên các máy chủ.

- **Tự Động Hóa Tác Vụ**

Ansible giúp tự động hóa các tác vụ hành chính thường xuyên như cài đặt phần mềm, cập nhật hệ thống, quản lý tường lửa, và thậm chí triển khai các ứng dụng trên nhiều máy chủ chỉ với vài dòng lệnh.

- **Tích Hợp Dễ Dàng với Hệ Thống CI/CD**

Ansible có thể được tích hợp vào các hệ thống **CI/CD** (Continuous Integration/Continuous Deployment) để tự động hóa quy trình triển khai ứng dụng. Điều này giúp các nhóm phát triển phần mềm triển khai các bản cập nhật một cách nhanh chóng và hiệu quả.

- **Không Cần Cài Đặt Agent**

Một trong những ưu điểm lớn nhất của Ansible là khả năng **không yêu cầu cài đặt agent** trên các máy chủ mục tiêu. Điều này giúp giảm thiểu việc sử dụng tài nguyên hệ thống và giảm thiểu độ phức tạp trong quá trình triển khai.

- **Ngôn Ngữ Khai Báo YAML**

Ansible sử dụng **YAML** (Yet Another Markup Language) để viết các playbook, giúp người dùng dễ dàng tạo ra các cấu hình và tác vụ tự động mà không cần kiến thức lập trình sâu. Ngôn ngữ YAML dễ đọc và dễ hiểu, ngay cả đối với những người không phải lập trình viên.

- **Hỗ Trợ Nhiều Mô-đun**

Ansible đi kèm với một bộ mô-đun phong phú, cho phép bạn thực hiện các tác vụ từ quản lý hệ thống, quản lý mạng, đến triển khai ứng dụng và nhiều tác vụ khác.

## Cách thức hoạt động

Ansible hoạt động trên kiến trúc client-server, với một máy chủ điều khiển (control node) và nhiều máy khách được quản lý (managed nodes). Máy chủ điều khiển chứa kho lưu trữ trung tâm của các playbook - các tập lệnh mô tả các tác vụ cần thực hiện trên các máy khách. Sau khi triển khai, Ansible sử dụng giao thức SSH để kết nối với các máy khách và thực hiện các tác vụ được định nghĩa trong playbook theo cách thức không cần agent.

# Cài đặt và cấu hình Ansible

Mình thực hành trên môi trường máy ảo, mình đã chuẩn bị sẵn 3 máy ảo gồm 1 máy làm máy chủ điều khiển (control machine) và 2 máy làm node với IP như sau:

- Control machine IP: **192.168.0.136**
- Máy Node 1 IP: **192.168.0.102**
- Máy Node 2 IP: **192.168.0.167**

Ta tiến hành Cài đặt và cấu hình Ansible trên máy chủ điều khiển (Controll machine)

**Bước 1: Cập nhật các gói hiện có trong hệ thống hiện tại:**

```
sudo apt update
sudo apt upgrade
```

**Bước 2: Cài đặt Anisible**

Theo mặc định, Phiên bản mới nhất của Ansible không có sẵn trong kho lưu trữ mặc định của Ubuntu. Vì vậy, bạn sẽ cần thêm Ansible PPA vào máy chủ của mình. Bạn có thể làm điều này bằng cách chạy lệnh sau:

```
sudo apt install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
```

Tiếp theo cập nhật lại và cài đặt Ansible bằng lệnh:

```
sudo apt update -y
sudo apt install ansible -y
```

Có thể xem phiên bản Ansible vừa cài bằng lệnh:

```
ansible --version
```

![image.png](/Images/tuan_6_ansible/image.png)

**Bước 3: Xác định hệ thống máy khách mà bạn muốn quản lý trong tệp máy chủ**

Bạn có thể thực hiện việc này bằng cách chỉnh sửa tệp `/etc/ansible/hosts`:

```
sudo /etc/ansible/hosts
```

Trong ví dụ này mình sẽ đặt tên nhóm 2 máy **node** là **webserver** bởi vì mục địch của mình là muốn cài **webserver** trên 2 máy **node** này thông qua **ansible và phía dưới tên nhóm là nơi khai báo những máy node,** cấu trúc tổng quát để khai báo có thể theo là:

```
[<ten_nhom>]
<ten_server_1> ansible_ssh_host=<node_ip_1> ansible_user=<username_login_node_1>
<ten_server_2> ansible_ssh_host=<node_ip_2> ansible_user=<username_login_node_2>
```

Ví dụ như này:

![image.png](/Images/tuan_6_ansible/image%201.png)

Ngoài ra có thể thêm `ansible_password=<passwork_login_node_1>` để khai báo mật khẩu luôn nhưng mình sẽ thực hiện SSH Key để đảm bảo tính bảo mật.

**Bước 4: Tạo SSH Key**

Ta cần tạo SSH Key ở Controll machine để khi SSH vào các máy node không cần nhập mật khẩu cũng như không cần khai báo ansible_password ở file **/etc/ansible/hosts**

Chạy lệnh sau để tạo key:

```
ssh-keygen -t rsa 
```

Khi chạy lệnh trên bạn sẽ được hỏi tên file và mật khẩu passphrase thì cứ nhập bình thường:

![image.png](/Images/tuan_6_ansible/image%202.png)

Sau khi cấu tạo có thể kiểm tra lại bằng lệnh:

```
cd ~/.ssh && ll
```

![image.png](/Images/tuan_6_ansible/image%203.png)

Tiếp theo, sao chép public key này vào những máy Node bằng lệnh sau:

```
ssh-copy-id -i ~/.ssh/ansible.pub user@node_IP
```

![image.png](/Images/tuan_6_ansible/image%204.png)

Để cho chắc thì mình qua máy Node đó và chạy lệnh sau xem đã có nội dung khóa vừa chép chưa:

```
cat ~/.ssh/authorized_keys
```

![image.png](/Images/tuan_6_ansible/image%205.png)

Vào file cấu hình /etc/ansible/ansible.cfg

```
sudo nano /etc/ansible/ansible.cfg
```

Và thêm đường dẫn tới Private Key:

![image.png](/Images/tuan_6_ansible/image%206.png)

**Bước 5: Kiểm tra Ansible đã cấu hình**

Ansible hiện đã được cài đặt và cấu hình. Đã đến lúc thử nghiệm Ansible.

Trên máy chủ, hãy thử ping hệ thống Máy khách của mình bằng lệnh sau:

```
ansible webserver -m ping
```

Ví dụ như:

![image.png](/Images/tuan_6_ansible/image%207.png)

# Thực hành ứng dụng Ansible

## Thực hành cài đặt LEMP Stack với Ansible

Ở ví dụ này mình sẽ ứng dụng Ansible để cài đặt LEMP Stack cho 2 máy chủ Node đã đưa ra ở phần **Cài Đặt và cấu hình Ansible** bên trên.

Trên máy Control, ta thực hiện các bước tuần tự

**Bước 1: Thiết lập dự án Ansible Playbook**

Thư mục dự án chính sẽ được lưu trữ tất cả các thư mục, tệp và cấu hình playbook của chúng ta

```
mkdir project-lemp/
cd project-lemp
```

Bây giờ hãy tạo tệp cấu hình mới ‘hosts’ và ‘site.yml’, sau đó tạo một thư mục mới có tên là ‘roles’.

```
touch hosts site.yml
mkdir -p roles/
```

Trong đó:

- **hosts** – Đó là một tệp kiểm kê chứa các mẩu thông tin về các máy chủ được quản lý bởi ansible. Nó cho phép bạn tạo một nhóm máy chủ giúp bạn dễ dàng quản lý và mở rộng quy mô tệp kiểm kê hơn. Tệp kiểm kê có thể được tạo bằng nhiều định dạng khác nhau, bao gồm định dạng INI và YAML.
- **site.yml**  – Tệp sổ tay chính chứa nhóm máy chủ nào sẽ được quản lý bằng các vai trò có sẵn của chúng tôi.
- **roles**  – đó là một nhóm sách hướng dẫn Ansible sẽ được sử dụng để cung cấp máy chủ. Các vai trò ansible có cấu trúc thư mục riêng, mỗi vai trò sẽ chứa các thư mục như task, handler, vars, v.v.

**Bước 2 – Tạo role ansible cho cấu trúc thư mục**

Trong thư mục ‘project-lemp’, hãy chuyển đến thư mục ‘roles’.

```
cd roles/
```

Tạo thư mục và tệp cấu trúc roles cho các vai trò ‘common’, ‘web’ và ‘db’ bằng cách chạy lệnh ansible-galaxy bên dưới. tất nhiên là bạn có thể đặt tên khác

```
ansible-galaxy init common
ansible-galaxy init web
ansible-galaxy init db
```

Trong đó:

- **common** là thư mục chưa những xử lý chung như update, add repository,…
- **web** là thư mục chứa những xử lý liên quan tới webserver như install nginx, apache, vhost,…
- **db** là thư mục chứa những xử lý liên quan tới Database như install mysql, create database,…

Kiểm tra lại cấu trúc thư mục sau khi khởi tạo bằng lệnh:

```
tree .
```

Nếu chưa có có thể cài đặt bằng:

```
apt install tree 
```

![image.png](/Images/tuan_6_ansible/image%208.png)

**Bước 3 – Thiết file hosts và site.yml**

Ở trong thư mục **/project-lemp,** thực hiện chỉnh sửa file hosts băng nano như sau:

```
nano hosts
```

Lưu và đóng.

Tiếp theo, chỉnh sửa tệp cấu hình site.yml.

```
nano site.yml
```

```
- hosts: webserver
  become: yes

  roles:
    - common
    - web
    - db
```

![image.png](/Images/tuan_6_ansible/image%209.png)

**Bước 4 – Thiết lập common role** 

Bên dưới danh sách các nhiệm vụ mà chúng ta sẽ thực hiện với các common role.

- Thay đổi kho lưu trữ
- Cập nhật kho lưu trữ
- Nâng cấp gói lên phiên bản mới nhất
- Thiết lập múi giờ máy chủ

Bây giờ hãy vào thư mục ‘common’ và chỉnh sửa cấu hình ‘task/main.yml’.

```
cd common/
nano tasks/main.yml
```

Tạo một tác vụ để thay đổi kho lưu trữ , mình sử dụng mô-đun ‘**copy**’ để sao chép ‘**sources.list**’ ở thư mục ‘**file**’ sang máy chủ từ xa và lưu vào ‘**/etc/apt/**’.

```
- name: Change repository Ubuntu 22.04Step 4 - Setup 'web' Roles
  copy:
    src: sources.list
    dest: /etc/apt/
    backup: yes
```

Tạo tác vụ cập nhật kho lưu trữ và nâng cấp tất cả các gói lên phiên bản mới nhất bằng mô-đun ‘**apt**’.

```
- name: Update repository and Upgrade packages
  apt:
    upgrade: dist
    update_cache: yes
```

Bây giờ hãy tạo tác vụ định cấu hình múi giờ hệ thống bằng mô-đun **timezone** ansible.

```
- name: Setup timezone to Asia/Jakarta
  timezone:
    name: Asia/Jakarta
    state: latest
```

Lưu và đóng.

Sau đó, tạo cấu hình kho lưu trữ mới ‘sources.list’ bên trong thư mục ‘files’

```
nano files/sources.list
```

Chọn kho lưu trữ gần nhất với vị trí máy chủ của bạn, bên dưới là của mình.

```
deb http://archive.ubuntu.com/ubuntu jammy main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu jammy-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu jammy-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu jammy-backports main restricted universe multiverse
```

Lưu và đóng.

Chúng ta đã cấu hình xong common role

**Bước 5: Thiết lập web role**

Dưới đây là chi tiết các nhiệm vụ mà chúng ta sẽ thực hiện với vai trò ‘web’:

- Cài đặt Nginx
- Cài đặt PHP-FPM
- Định cấu hình php.ini
- Tạo một máy chủ ảo
- Thêm tập tin phpinfo

Vào thư mục ‘web’ và chỉnh sửa tệp ‘task/main.yml’.

```
cd web/
nano tasks/main.yml
```

Tạo tác vụ đầu tiên để cài đặt nginx bằng mô-đun apt.

```
- name: Install Nginx
  apt:
    name: nginx
    state: latest
```

Bây giờ hãy tạo tác vụ cài đặt PHP-FPM với một số phần mở rộng cơ bản. Và để cài đặt nhiều gói, chúng ta có thể sử dụng định dạng ‘danh sách’ python như bên dưới.

```
- name: Instal PHP-FPM
  apt:
    name: ['php','php-fpm','php-common','php-cli','php-curl']
    state: latest
```

Tiếp theo, chúng tôi sẽ thêm dòng mới vào cấu hình php.ini bằng mô-đun ‘blockinfile’. Và ở cuối dòng, chúng tôi sẽ thông báo cho ansible khởi động lại dịch vụ php-fpm sau khi định cấu hình tệp php.ini.

```
- name: Configure php.ini
  blockinfile:
    dest: /etc/php/{{ php_version }}/fpm/php.ini
    block: |
      date.time = Asia/Jakarta
      cgi-fix_pathinfo = 0
    backup: yes
  notify: restart php-fpm
```

Bây giờ chúng ta sẽ sao chép cấu hình máy chủ ảo nginx bằng mô-đun ‘template’. Mô-đun mẫu sẽ sao chép cấu hình từ thư mục ‘templates’ sang máy chủ từ xa. Chúng tôi sẽ sao chép mẫu máy chủ ảo jinja2 ‘vhost.j2’ vào thư mục ‘/etc/nginx/sites-available/’ và cuối cùng chúng tôi sẽ thông báo cho ansible khởi động lại dịch vụ nginx.

```
- name: Create Nginx virtual host
  template:
    src: vhost.j2
    dest: /etc/nginx/sites-available/vhost-{{ domain_name }}
  notify: restart nginx
```

Xóa vhost file nếu đã tồn tại trong /etc/nginx/sites-enabled

```
- name: Remove old vhost file if exists
  file:
    path: /etc/nginx/sites-enabled/vhost-{{ domain_name }}
    state: absent
```

Tạo liên kết giữa vhost vừa tạo tới /etc/nginx/sites-enabled

```
- name: Enable virtual host
  file:
    src: /etc/nginx/sites-available/vhost-{{ domain_name }}
    dest: /etc/nginx/sites-enabled/vhost-{{ domain_name }}
    state: link
```

Tắt site default trong /etc/nginx/sites-enabled đi và khởi động lại 

```
- name: Disable default site
  file:
    path: /etc/nginx/sites-enabled/default
    state: absent
  notify: restart nginx
```

Sau đó, chúng ta sẽ tạo các tác vụ mới để tạo thư mục web-root bằng mô-đun ‘file’ và sao chép mẫu index.php vào đó.

```
- name: Create web-root directory
  file:
    path: /var/www/{{ domain_name }}
    state: directory

- name: Upload index.html and info.php files
  template:
    src: index.php.j2
    dest: /var/www/{{ domain_name }}/index.php
```

Lưu và đóng.

Bây giờ chúng ta sẽ định cấu hình trình xử lý để khởi động lại dịch vụ nginx và php-fpm. Chỉnh sửa cấu hình ‘handlers/main.yml’ bằng nano.

```
nano handlers/main.yml
```

Dán cấu hình bên dưới.

```
- name: restart nginx
  service:
    name: nginx
    state: restarted
    enabled: yes

- name: restart php-fpm
  service:
    name: php{{ php_version }}-fpm
    state: restarted
    enabled: yes
```

Lưu và đóng.

![image.png](/Images/tuan_6_ansible/image%2010.png)

Tiếp theo, chúng ta sẽ chỉnh sửa cấu hình ‘vars/main.yml’. Ở trong tasks/main.yml, bạn sẽ thấy các cấu hình biến ‘{{ php_version }}’ và ‘{{ domain_name }}’. Các biến đó là những biến môi trường cho phiên bản php và tên miền sẽ được sử dụng. Biến này giúp ansible có thể tái sử dụng nhiều hơn vì chúng ta chỉ cần chỉnh sửa cấu hình biến ‘vars/main.yml’ và không cần đi chỉnh sửa từng cái riêng lẽ.

Chỉnh sửa cấu hình biến ‘vars/main.yml’ bằng trình soạn thảo nano.

```
nano /vars/main.yml
```

Dán nội dung sau, bạn có thể thay đôi cho phù hợp:

```
php_version: 8.1
domain_name: thiendocker
```

Lưu và đóng.

Bây giờ chúng ta sẽ tạo cấu hình mẫu jinja2 ‘index.php.j2’ và ‘vhost.j2’ trên thư mục ‘templates/’.

```
nano templates/index.php.j2
```

Dán cấu hình bên dưới.

```
<html>
<body>

<h1><center>index.html for domain {{ domain_name }}</center></h1>

<p>
<p>

<?php
phpinfo();
?>

</body>
</html>
```

Lưu và đóng.

Sau đó, tạo mẫu cho cấu hình máy chủ ảo nginx ‘vhost.j2’.

```
nano templates/vhost.j2
```

Dán nội dung sau:

```
server {
    listen 80;
    listen [::]:80;

    root /var/www/{{ domain_name }};
    index index.php index.html index.htm index.nginx-debian.html;

    server_name {{ domain_name }};

    location / {
        try_files $uri $uri/ =404;
    }

    # pass PHP scripts to FastCGI server
    #
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/var/run/php/php{{ php_version }}-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

     #Neu ban co cai dat phpmyadmin
     location /phpmyadmin {
        root /usr/share/;
        index index.php index.html index.htm;

        location ~ ^/phpmyadmin/(.+\.php)$ {
            try_files $uri =404;
            root /usr/share/;
            fastcgi_pass unix:/var/run/php/php{{ php_version }}-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

        location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
            root /usr/share/;
        }
    }
}
```

Lưu và đóng cấu hình và chúng ta đã hoàn tất cấu hình vai trò web.

**Bước 6 – Thiết lập vai trò ‘db’**

Dưới đây là chi tiết các nhiệm vụ sẽ thực hiện với **db role**.

- Cài đặt mysql
- Tạo cơ sở dữ liệu MySQL
- Tạo người dùng MySQL
- Khởi động lại mysql

Đi tới thư mục ‘db’ và chỉnh sửa cấu hình ‘tasks/main.yml’.

```
cd db/
nano tasks/main.yml
```

Bây giờ hãy cài đặt các gói MySQL bằng mô-đun ‘apt’ và định dạng ‘danh sách’ python để cài đặt nhiều gói.

```
- name: Install MySQL and required packages
  apt:
    name:
      - mysql-server
      - mysql-client
      - python3-mysqldb
      - python3-pymysql
    state: present
    update_cache: yes
  notify: restart mysql
```

Tiếp theo là thiết lập mật khẩu và đổi phương thức đăng nhâp cho root.

```
- name: Set root password and change authentication method
  mysql_user:
    name: root
    host: localhost
    password: "{{ mysql_root_password }}"
    login_unix_socket: /var/run/mysqld/mysqld.sock
    plugin: mysql_native_password
    state: present
  become: true
```

Sau đó thêm task để tạo cơ sở dữ liệu MySQL và người dùng, sau đó cấp tất cả các đặc quyền của người dùng cho cơ sở dữ liệu.

```
- name: Create database
  mysql_db:
    name: '{{ db_name }}'
    state: present

- name: Create user for the database
  mysql_user:
    name: '{{ db_user }}'
    password: '{{ db_pass }}'
    encrypted: yes
    priv: '{{ db_name }}.*:ALL'
    state: present
```

Lưu và đóng.

![image.png](/Images/tuan_6_ansible/image%2011.png)

Tiếp theo, chỉnh sửa cấu hình ‘handlers/main.yml’.

```
nano handlers/main.yml
```

Dán cấu hình của tác vụ để khởi động lại dịch vụ MySQL.

```
- name: restart mysql
  service:
    name: mysql
    state: restarted
    enabled: yes
```

Lưu và đóng.

Sau đó, chỉnh sửa cấu hình biến vars ‘vars/main.yml’.

```
nano vars/main.yml
```

Dán các biến này cho cơ sở dữ liệu MySQL và cấu hình người dùng bên dưới.

```
db_name:  db_name
db_user: userpass
db_pass: dbpass
```

Lưu và đóng.

Biến ‘db_pass’ có mật khẩu được mã hóa MySQL và bạn có thể tạo mật khẩu MySQL được mã hóa bằng các công cụ trực tuyến.

**Bước 7 – Chạy Playbook Ansible**

Xem thư mục dự án Ansible.

```
cd project-lemp/
```

Chạy lệnh ansible-playbook bên dưới.

```
ansible-playbook -i hosts site.yml
```

Bây giờ ansible sẽ chạy tất cả các vai trò mà chúng ta gán cho máy chủ. Khi hoàn tất, bạn sẽ thấy kết quả như bên dưới.  

![image.png](/Images/tuan_6_ansible/image%2012.png)

![image.png](/Images/tuan_6_ansible/image%2013.png)

**Bước 8 –  Kiểm tra các dịch vụ đã cài trên máy Node**

Như đã làm những bước trên, chúng ta đã cài MySQL, Nginx, PHP và cấu hình vhost Nginx, ta sẽ kiểm tra lại xem đã cài đặt thành công hay chưa

Truy cập vào `http://your_ip` nếu kết quả giống bên dưới ảnh này thì đã cấu hình thành công Nginx và cài đăt xong php

![image.png](/Images/tuan_6_ansible/image%2014.png)

Tiếp là kiểm tra đăng nhập không mật khẩu và có mật khẩu ở mysql. Nếu như đúng thì sẽ không thể đăng nhập vào mysql mà không nhập mật khẩu, chỉ cho phép đăng nhập với đầy đủ username và password

![image.png](/Images/tuan_6_ansible/image%2015.png)

## Thực hành cài đặt LAMP Stack với Ansible

**Bước 1: Tạo thư mục dự án trên máy Control**

```
mkdir ansible_playbook && cd $_
```

**Bước 2: Tạo biến mặc định** 

Tạo tệp biến mặc định sẽ chứa thông tin như tên miền, mật khẩu gốc MariaDB, v.v.

Trong thư mục làm việc của chúng ta, hãy tạo một thư mục con có tên vars và thêm tệp cấu hình biến.

```
mkdir vars && cd vars
nano default.yml
```

Trong tệp **default.yml**, thêm thông tin bên dưới, thay thế các biến bằng thông tin chi tiết của bạn:

```
---
mysql_root_password: "P@ssw0rd"
app_user: "apache"
http_host: "lamp.example.com"
http_conf: "lamp.example.com.conf"
http_port: "80"
disable_default: true
```

Tạo file **hosts** trong `/ansible_playbook` , thêm thông tin các mấy Node và gom chúng thành 1 nhóm sau đó đặt tên như hình dưới, bạn có thể thay thế thông tin phù hợp:

```
[lampstack]
server1 ansible_ssh_host=192.168.0.104 ansible_user=thienduong
server2 ansible_ssh_host=192.168.0.118 ansible_user=thienduong
```

**Bước 3: Tạo Apache role**

Tạo thư mục **roles** từ thư mục `/ansible_playbook` và một thư mục con khác trong **roles** cho **Apache**.

```
mkdir -p roles/apache
```

Ở đây, chúng ta cần tạo role cho Apache và PHP, role này sẽ chứa tất cả các bước, mô-đun và mẫu cần thiết cho dịch vụ Apache.

**Bước 4: Tạo Apache & PHP tasks**

Tạo tác vụ thực thi chính cho Apache và PHP, bên trong thư mục Apache

```
mkdir tasks && cd tasks
nano main.yml
```

Thêm nội dung bên dưới vào tệp **main.yml**:

```
---
- name: Install prerequisites
  apt: name={{ item }} update_cache=yes state=latest force_apt_get=yes
  loop: [ 'aptitude' ]

  #Apache Configuration
- name: Install Apache and PHP Packages
  apt: name={{ item }} update_cache=yes state=latest
  loop: [ 'apache2', 'php', 'php-mysql', 'libapache2-mod-php' ]

- name: Create document root
  file:
    path: "/var/www/{{ http_host }}"
    state: directory
    owner: www-data
    mode: '0755'

- name: Set up Apache virtualhost
  template:
    src: "files/apache.conf.j2"
    dest: "/etc/apache2/sites-available/{{ http_conf }}"
    
- name: Enable new site
  shell: /usr/sbin/a2ensite {{ http_conf }}
  
- name: Disable default Apache site
  shell: /usr/sbin/a2dissite 000-default.conf
  when: disable_default
  notify: Reload Apache
# UFW Configuration
- name: "UFW - Allow HTTP on port {{ http_port }}"
  ufw:
    rule: allow
    port: "{{ http_port }}"
    proto: tcp

  # PHP Info Page
- name: Sets Up PHP Info Page
  template:
    src: "files/info.php.j2"
    dest: "/var/www/{{ http_host }}/info.php"

- name: Reload Apache
  service:
    name: apache2
    state: reloaded

- name: Restart Apache
  service:
    name: apache2
    state: restarted
```

**Bước 5: Tạo Apache handler**

Thêm Trình xử lý cho Apache. Điều này nên được thực hiện trong thư mục roles/apache/:

```
mkdir handlers && cd handlers
nano main.yml
```

Thêm nội dung sau cho Apache handler

```sql
---
- name: Reload Apache
  service:
    name: apache2
    state: reloaded

- name: Restart Apache
  service:
    name: apache2
    state: restarted
```

**Bước 6: Thêm Template cho Virtualhost và trang index.php** 

Tạo các tệp sẽ được sử dụng để tạo Virtualhost và tệp chỉ mục info.php. Điều này cũng được thực hiện trong thư mục  roles/apache/:

```
mkdir files && cd files
```

Tạo file Virtualhost

```
nano apache.conf.j2
```

Dán nội dung dưới vào file **apache.conf.j2**

```
<VirtualHost *:{{ http_port }}>
    ServerAdmin webmaster@localhost
    ServerName {{ http_host }}
    ServerAlias www.{{ http_host }}
    DocumentRoot /var/www/{{ http_host }}
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/{{ http_host }}>
          Options -Indexes
    </Directory>

    <IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl  index.xhtml index.htm
    </IfModule>

</VirtualHost>
```

Tạo file demo PHP chứa thông tin bản PHP đã cài đặt:

```
nano info.php.j2
```

Dán nội dung dưới vào file **info.php.j2**

```powershell
<?php
phpinfo();
```

Sau khi  thực hiện tới đây, kiểm tra lại đảm bảo cây thư mục trông như thế này:

```
../apache$ tree .
```

![image.png](/Images/tuan_6_ansible/image%2016.png)

**Bước 7: Tạo MariaDB role**

Trong thư mục **roles** tạo thư mục MariaDB và tạo thư  mục con **tasks** để chứa các tác vụ liên quan đến ****MariaDB

```
mkdir -p mariadb/tasks && cd mariadb/tasks
```

Tạo file cấu hình cho **MariaDB tasks**:

```
nanp main.yml
```

Dán nội dung dưới vào file main.yml, có thể thay đổi để phù hợp với như cầu của bạn:

```
---
- name: Install prerequisites
  apt: name={{ item }} update_cache=yes state=latest force_apt_get=yes
  loop: [ 'aptitude' ]

 #Install MariaDB server
- name: Install MariaDB Packages
  apt: name={{ item }} update_cache=yes state=latest
  loop: [ 'mariadb-server', 'python3-pymysql' ]

# Start MariaDB Service
- name: Start MariaDB service
  service:
    name: mariadb
    state: started
  become: true

 # MariaDB Configuration
- name: Sets the root password
  mysql_user:
    name: root
    password: "{{ mysql_root_password }}"
    login_unix_socket: /var/run/mysqld/mysqld.sock

- name: Removes all anonymous user accounts
  mysql_user:
    name: ''
    host_all: yes
    state: absent
    login_user: root
    login_password: "{{ mysql_root_password }}"

- name: Removes the MySQL test database
  mysql_db:
    name: test
    state: absent
    login_user: root
    login_password: "{{ mysql_root_password }}"
```

Sau khi thực hiện đến bước này thì đảm bảo rằng cấu trúc trong **/ansible_playbook/roles trông như thế này:**

![image.png](/Images/tuan_6_ansible/image%2017.png)

**Bước 8: Tạo Playbook cho Ansible**

Trong thư mục `/ansible_playbook` tạo file **lampstack.yml** để sử dụng các **roles** vừa khởi tạo ỏ những bước trên.

```
nano lampstack.yml
```

Dán nội dụng bên dưới vào:

```
---
- name: configure lamp
  hosts: lampstack
  become: yes
  become_method: sudo
  vars_files:
    - vars/default.yml
  roles:
    - apache
    - mariadb
```

Toàn bộ cây cấu hình từ thư mục làm việc sẽ trông như thế này:

![image.png](/Images/tuan_6_ansible/image%2018.png)

**Bước 9: Chạy Ansible Playbook để cài LAMP Stack** 

Chạy lệnh sau để cài đặt LAMP Stack dựa vào Playbook đã cấu hình ở bước trên 

```
ansible-playbook -i hosts lampstack.yml --ask-become-pass
```

Sau khi chạy lệnh trên thì bạn sẽ được hỏi mật khẩu, sau khi nhập mật khẩu sẽ tiến hành chạy các task đã cấu hình sẵn:

![image.png](/Images/tuan_6_ansible/image%2019.png)

![image.png](bff70608-e2bb-4e4f-a36f-c39f8df742c0.png)

**Bước 10: Kiểm tra trên các máy node**

Sau khi tiến hành cài đặt các dịch vụ thông qua Ansible thì chúng ta cần kiểm tra lại trên các máy được cài đặt đã hoạt động đúng hay chưa.

Vào một trình duyệt bất kỳ, truy cập `http://your_node_ip/info.php` nêu như cài đặt đúng thì sẽ hiển thị trang thông tin PHP như sau:

![image.png](/Images/tuan_6_ansible/image%2020.png)

Để kiêm tra xem MySQL/MariaDB đã cài đặt đúng chưa thì hãy thử đang nhập với 2 lệnh 

```
sudo mysql
```

và 

```
sudo mysql -u root -p
```

![image.png](/Images/tuan_6_ansible/image%2021.png)

---
THE END