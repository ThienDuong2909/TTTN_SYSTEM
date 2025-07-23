# Tìm hiểu về Docker

# Giới thiệu về Docker

**Docker** là một dự án mã nguồn mở giúp tự động triển khai các ứng dụng Linux vào trong các container ảo hóa. Docker cung cấp một lớp trừu tượng và tự động ảo hóa dựa trên Linux. Docker sử dụng những tài nguyên cô lập của Linux như cgroups, kernel, quản lý tệp để cho phép các container chạy độc lập bên trong một thực thể Linux.

Các thay đổi được lưu trữ trong các Docker image, các lớp tệp hệ thống được tạo ra và lưu lại dựa theo từng lớp (layer). Điều này giúp cho Docker Image giảm dung lượng đáng kể so với máy ảo (VM).

Các ứng dụng muốn chạy bằng Docker phải là ứng dụng chạy được trên Linux. Gần đây, Docker có hỗ trợ thêm việc chạy ứng dụng Windows trong các Windows container.

**Các thành phần chính của Docker**

- **Docker Engine**: dùng để tạo ra Docker image và chạy Docker container.
- **Docker Hub**: dịch vụ lưu trữ giúp chứa các Docker image.
- **Docker Machine**: tạo ra các Docker engine trên máy chủ.
- **Docker Compose**: chạy ứng dụng bằng cách định nghĩa cấu hình các Docker container thông qua tệp cấu hình
- **Docker image**: một dạng tập hợp các tệp của ứng dụng, được tạo ra bởi Docker engine. Nội dung của các Docker image sẽ không bị thay đổi khi di chuyển. Docker image được dùng để chạy các Docker container.
- **Docker container**: một dạng runtime của các Docker image, dùng để làm môi trường chạy ứng dụng.

# Cài đặt Docker

**Bước 1: Cập nhật các gói trong hệ thống**

```bash
sudo apt update -y && apt upgrade -y
```

**Bước 2: Cài đặt một số gói cho phép sử dụng HTTPS**

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

**Bước 3: Thêm khóa GPG của kho lưu trữ Docker.**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

**Bước 4: Thêm kho lưu trữ Docker của Ubuntu 22.04 ( *jammy*) vào các *apt* sources.**

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**Bước 5: Cập nhật packages và thiết lập để cài đặt Docker từ kho lưu trữ chính thức.**

```
sudo apt update
sudo apt-cache policy docker-ce
```

![image.png](/Images/tuan_5_docker/image.png)

**Bước 6: Cài đặt Docker**

```bash
sudo apt install docker-ce -y
```

**Bước 7: Kiểm tra trạng thái của Docker**

```bash
sudo systemctl status docker
```

![image.png](/Images/tuan_5_docker/image%201.png)

**Bonus: Cấu hình quyền Sudo cho user sử dụng Docker**

```bash
sudo usermod -aG docker <<username>>
```

# Thao tác với Docker

Kiểm tra thông tin Docker đang sử dụng

```bash
docker info
```

![image.png](/Images/tuan_5_docker/image%202.png)

Tải một image từ Docker Hub (hoặc registry khác) về máy cục bộ.

```bash
docker pull <image>:tag
```

Kiểm tra các image đã tải xuống

```bash
docker images
```

![image.png](/Images/tuan_5_docker/image%203.png)

Tạo và khởi chạy một container từ một image.

```bash
docker run -d --name my-ubuntu ubuntu:latest sleep 3600
```

![image.png](/Images/tuan_5_docker/image%204.png)

Trong đó:

- `-d` : chạy nền
- `—name <tên container>` : đặt tên cho container

Xem các container đang chạy

```bash
docker ps
```

![image.png](/Images/tuan_5_docker/image%205.png)

Xem tất cả các container kể cả đã dừng

```bash
docker ps -a
```

![image.png](/Images/tuan_5_docker/image%206.png)

Dừng một container đang chạy một cách an toàn.

```bash
docker stop <tên container>
```

Tạo một image mới từ trạng thái hiện tại của container

```bash
docker commit <ten image hien tai> <ten imgae moi>
```

Đẩy một image lên Docker Hub hoặc registry

```bash
docker push yourusername/my-image
```

# Chạy một dự án thực tế

Ở ví dụ này mình sẽ chạy một dự án thực tế (ví dụ như backend Nodejs), đầu tiên bạn cần tạo file .env để chưa các biến môi trường bằng lệnh sau:

```bash
cd /backend
cat > .env <<EOF
PORT=5000
MONGODB_URI=mongodb://mongo:27017/myapp
JWT_SECRET=your_jwt_secret_key
EOF
```

Tiếp theo là tạo file .dockerignore:

```bash
cat > .dockerignore <<EOF
node_modules
.env
npm-debug.log
EOF
```

Tạo Dockerfile cho backend:

```bash
cat > Dockerfile <<'EOF'
# Sử dụng Node base image
FROM node:18-alpine

# Tạo thư mục app trong container
WORKDIR /app

# Copy package info và cài đặt trước (tối ưu cache)
COPY package*.json ./
RUN npm install

# Copy toàn bộ source vào trong container
COPY . .

# Chạy lệnh build nếu có (tùy app)

# Expose port
EXPOSE 5000

# Khởi động ứng dụng
CMD ["node", "src/index.js"]
EOF
```

Build image backend:

```bash
sudo docker build -t node-backend .
```

| Thành phần | Ý nghĩa |
| --- | --- |
| `sudo` | Chạy với quyền root (thường cần trên Ubuntu) |
| `docker build` | Lệnh build image |
| `-t node-backend` | Gán tag cho image là `node-backend` (tên image) |
| `.` | Chỉ định thư mục context (thư mục hiện tại) chứa `Dockerfile` |

Tạo và chạy container từ image vừa tạo:

```bash
docker run -p 3030:3030 --env-file .env node-backend
```

Trong đó:

- `-p 3030:3030` : port 3030 bên ngoài máy (host) sẽ map tới 3030 bên trong container
- -- env-file .env: truyền biến môi trường

# Tìm hiểu về Docker Compose

### Giới thiệu

**Docker Compose** là một công cụ mạnh mẽ được sử dụng để định nghĩa và quản lý các ứng dụng Docker multi-container. Docker Compose cho phép bạn sử dụng một file YAML để cấu hình các dịch vụ của ứng dụng, sau đó với một lệnh duy nhất, bạn có thể tạo và khởi động tất cả các dịch vụ từ cấu hình của bạn. Trong bài viết này, chúng ta sẽ tìm hiểu chi tiết về Docker Compose, cách cài đặt và sử dụng nó, và cung cấp các ví dụ cụ thể để minh họa.

### **Lợi ích của Docker Compose**

1. **Đơn Giản Hóa Quản Lý**: Docker Compose giúp bạn quản lý các ứng dụng multi-container một cách dễ dàng bằng cách sử dụng một file cấu hình duy nhất.
2. **Tính Nhất Quán**: Docker Compose đảm bảo rằng môi trường phát triển, kiểm thử và sản xuất của bạn sẽ nhất quán, giúp giảm thiểu các lỗi phát sinh do sự khác biệt giữa các môi trường.
3. **Tự Động Hóa**: Docker Compose cho phép bạn tự động hóa quá trình xây dựng, khởi động và dừng các dịch vụ của ứng dụng.
4. **Khả Năng Mở Rộng**: Docker Compose hỗ trợ việc mở rộng và thu nhỏ các dịch vụ của ứng dụng một cách dễ dàng.

### **Cài đặt Docker Compose**

Docker Compose là một tệp nhị phân độc lập, không đi kèm trong gói Docker Engine. Nhưng nếu như bạn đã cài Docker CE rồi thì có thể chạy lệnh sau để cài Docker Compose

```bash
sudo apt install docker-compose -y
```

Sau khi cài đặt xong có thể kiểm tra phiên bản đã cài đặt bằng lệnh sau:

```bash
docker-compose --version
```

![image.png](/Images/tuan_5_docker/image%207.png)

### Thực hành sử dụng Docker Compose

Ở phần [Chạy một dự án thức tế](https://www.notion.so/T-m-hi-u-v-Docker-233757a4e31f8098a9f3cace831abe29?pvs=21) , mình đã chạy độc lập 1 container chứa dữ liệu backend viết bằng Nodejs. 

Tiếp theo ở phần này mình sẽ thực hành chạy song song 2 container chính bao gồm 1 container chứa dữ liệu backend viết bằng Nodejs và 1 container chứa dữ liệu frontend viết bằng ReactJS

Đầu tiên như đã cấu hình Dockfile cho phần backend đã làm trước đó, thì ta làm tương tự bên frontend với nội dung `/frontend/Dockfile` như sau:

```bash
FROM node:18-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]
```

Sau khi có đầy đủ 2 Dockfile ở cả 2 phần backend và frontend rồi thì ta trở ra thư mục chứa 2 thư mục lớn này và tạo file `docker-compose.yml` với nội dung như sau:

```bash

version: "3.8"
services:
  backend:
    build: ./backend
    ports:
      - "3030:3030"
    env_file:
      - ./backend/.env
    networks:
      - appnet
    restart: unless-stopped

  frontend:
    build: ./frontend
    ports:
      - "5173:5173"
    env_file:
      - ./frontend/.env
    networks:
      - appnet
    restart: unless-stopped

networks:
  appnet:

```

Cuối cùng là tạo và chạy cùng lúc các dịch vụ đã khai báo trong file `docker-compose.yml` bằng lệnh sau:

```bash
sudo docker-compose up --build
```

### Một số lỗi trong quá trình thực hành:

Trong quá trình thực hiện lệnh:

```bash
sudo docker-compose up --build
```

Đôi khi sẽ gặp lỗi này:

```bash
Traceback (most recent call last):
  File "/usr/bin/docker-compose", line 33, in <module>
    sys.exit(load_entry_point('docker-compose==1.29.2', 'console_scripts', 'docker-compose')())
  File "/usr/lib/python3/dist-packages/compose/cli/main.py", line 81, in main
    command_func()
    ....
```

Lỗi này xuất hiện khi Docker Compose v1 cũ build ra không có trường `ContainerConfig`, và bản v1 này không xử lý được.

Cách xử lý la ta xóa bản Docker Compose cũ đi là cài lại bản Docker Compose mới, các bước lần lượt như sau:

```bash
sudo apt remove docker-compose -y
```

```bash
sudo apt update
sudo apt install docker-compose-plugin -y
```

Sau đó kiểm tra lại bằng lệnh:

```bash
docker compose version
```

![image.png](/Images/tuan_5_docker/image%208.png)

Cuối cùng là chạy lại toàn bộ dự án bằng lệnh:

```bash
docker compose down --volumes
docker compose up --build
```

# Tìm hiểu về Docker Swarm

### Giới thiệu

**Docker Swarm** là công cụ native clustering cho Docker. Cho phép ta có thể gom một số Docker host lại với nhau thành dạng cụm (cluster) và ta có xem nó như một máy chủ Docker ảo (virtual Docker host) duy nhất. Và một Swarm là một cluster của một hoặc nhiều Docker Engine đang chạy. Và Swarm mode cung cấp cho ta các tính năng để quản lý và điều phối cluster.

### Kiến trúc Swarm

![image.png](/Images/tuan_5_docker/image%209.png)

### **Làm việc với Docker Swarm**

Trong phần này ta sẽ tiến hành thực hành với Docker Swarm thông qua demo nhỏ. Đầu tiên ta cần vài máy ảo, có thể sử dụng lệnh sau để tạo:

```bash
 docker-machine create <machine-name>
```

Trong đó:

- <machine-name>: tên máy ảo bạn muốn đặt.

Tạo machine(máy ảo) cho swarm manager:

```bash
docker-machine create manager
```

Tiếp đến là các machine cho swarm worker lần lượt là : worker1, worker2, worker3.

Trong ví dụ này tôi chỉ tạo một máy ảo worker và 1 manager, chạy lệnh này nếu bạn muốn máy chủ này là mannager:

```bash
docker swarm init 
```

![image.png](/Images/tuan_5_docker/image%2010.png)

Giờ ta làm việc trên những máy ảo được tạo để làm worker, chạy lệnh như sau:

```bash
docker swarm join --token <token> <host>:<port>
```

Trong đó:

- host: Địa chỉ ip của máy manager.
- port: Cổng port của máy manager.

![image.png](/Images/tuan_5_docker/image%2011.png)

Nếu thành công sẽ hiện dòng chữ “This node joined a swarm as a work” như trên hình

Đẻ xem danh sách các wordker trên máy manager ta chạy lệnh sau:

```bash
docker node ls
```

![image.png](/Images/tuan_5_docker/image%2012.png)

### **Thực hành chạy dịch vụ Docker Swarm**

Các dịch vụ chạy trên Swarm thì nó sẽ tạo ra các container từ các Image, ở đây có xây dựng sẵn các ứng dụng đạng lưu ở Hub Docker, đó là các ứng dụng chạy bằng Node, DotNet, PHP với tên Image tương ứng trên Hub Docker là `ichte/swarmtest:node`, `ichte/swarmtest:dotnet`, `ichte/swarmtest:php` ([swarmtest](https://hub.docker.com/repository/docker/ichte/swarmtest)). Xây dựng sẵn để các Node trong Swarm có thể pull về khí nó cần.

Chạy dịch vụ đầu tiên:

```
docker service create --replicas 5 -p 8085:8085 --name testservice ichte/swarmtest:node
```

![image.png](/Images/tuan_5_docker/image%2013.png)

Lệnh trên tạo ra một dịch vụ đặt tên là testservcie `--name testservice`, nó tạo 5 container trên swarm cho dịch vụ này `--replicas 5`, các container tạo từ Image `ichte/swarmtest:node`, có ánh xạ cổng qua tham số `-p`

Một số lệnh quản lý, cập nhật, scale dịch vụ (lệnh thi hành trên node manager)

```
# Liệt kê các service trên swarm
docker service ls

# Liệt kê các container cho dịch vụ có tên testservice
docker service ps testservice

# Kiểm tra log cho dịch vụ testservice
docker service logs testservice

# Scale - thay đổi số container cho dịch vụ testservice đang chạy thành n (1, 2, 3 ...) container
docker service scale testservice=n

# Cập nhật thiết lập cho dịch vụ testservice đang chạy
# - Thay đổi Image
docker service update --image=ichte/swarmtest:php testservice
# - Thay đổi tài nguyên CPU, MEM
docker service update --limit-cpu="0.5"  --limit-memory=150MB testservice
# - Các cập nhật khác update service

# Xóa dịch vụ testservice
docker service rm servicename
```

# Cài đặt WordPress thông qua Docker Compose

**Bước 1: Cập nhật các gói trong hệ thống** 

```bash
sudo apt update && apt upgrade -y
```

**Bước 2: Tạo thư mục chứa WordPress** 

Ở đâu mình sẽ tạo thư mục wp-project để phục vụ cho việc cài đặt Wordpress

```bash
cd /home
mkdir wp-project
```

**Bước 3: Tạo file .env đặt các biến môi trường** 

Thay vì thiết lập toàn bộ trong file Docker Compose (file quy định hành động của container), chúng ta sẽ lưu và giới hạn truy cập cho các thông tin nhạy cảm vào file `.env`. Bằng cách này, các bạn có thể ngăn thông tin bị sao chép sang các repository khác và tránh nguy cơ bị lộ dữ liệu.
Ở đây mình mình sẽ tạo và khai cáo các biến liên quan đến CSDL như tài user, password, tên database

Tạo file **.env:**

```bash
touch .env
```

Gán những giá trị nói trên cho các biến trong file:

```bash
MYSQL_ROOT_PASSWORD=pwroot
MYSQL_USER=user
MYSQL_PASSWORD=userpw
MYSQL_DATABASE=dbname
```

**Bước 4: Tạo file định nghĩa các service docker-compose.yml**

Tạo một số container bao gồm: Cơ sở dữ liệu MariaDB, ứng dụng WordPress, web server Nginx, PHP và PHPMyAdmin.

Tạo file bằng lệnh sau:

```bash
touch docker-compose.yml
```

Mở file và dán nội dung này vào:

```bash
nano docker-compose.yml
```

```bash
services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: unless-stopped
    env_file: .env
    volumes:
      - mariadb:/var/lib/mysql
    command: '--default-authentication-plugin=mysql_native_password'
    networks:
      - app-network

  wordpress:
    depends_on:
      - mariadb
    image: wordpress:6.8-fpm-alpine
    container_name: wordpress
    restart: unless-stopped
    env_file: .env
    environment:
      - WORDPRESS_DB_HOST=mariadb:3306
      - WORDPRESS_DB_USER=${MYSQL_USER}
      - WORDPRESS_DB_PASSWORD=${MYSQL_PASSWORD}
      - WORDPRESS_DB_NAME=${MYSQL_DATABASE}
    volumes:
      - wordpress:/var/www/html
    networks:
      - app-network

  nginx:
    depends_on:
      - wordpress
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - wordpress:/var/www/html
      - ./nginx:/etc/nginx/conf.d
      - certbot-etc:/etc/letsencrypt
    networks:
      - app-network

  php:
    image: php:8.2-fpm-alpine
    container_name: php_8.2
    restart: unless-stopped
    volumes:
      - ./wordpress:/var/www/html:delegated
    networks:
      - app-network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      - PMA_HOST=mariadb
      - PMA_USER=$MYSQL_USER
      - PMA_PASSWORD=$MYSQL_PASSWORD
    depends_on:
      - mariadb
    networks:
      - app-network

volumes:
  certbot-etc:
  wordpress:
  mariadb:

networks:
  app-network:
    driver: bridge
```

**Bước 5: Cấu hình file nginx.conf**

Như đã khai báo trong file docker-compose.yml thì `./nginx` sẽ ánh xạ tới `/etc/nginx/conf.d`

. Ta cần tạo thư mục **nginx** cùng cấp với file **.yml** này và tạo file cấu hình **nginx.conf** trong thư mục **nginx** vừa tạo

Tạo thư mục, file cấu hình và bắt đầu cấu hình:

```bash
mkdir nginx
sudo nano nginx/nginx.conf
```

Có thể tùy chỉnh nội dụng trong file nginx.conf như sau:

```bash
server {
        listen 80;
        listen [::]:80;

        server_name your_domain www.your_domain;

        index index.php index.html index.htm;

        root /var/www/html;

        location ~ /.well-known/acme-challenge {
                allow all;
                root /var/www/html;
        }

        location / {
                try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass wordpress:9000;
                fastcgi_index index.php;
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_param PATH_INFO $fastcgi_path_info;
        }

        location ~ /\.ht {
                deny all;
        }

        location = /favicon.ico {
                log_not_found off; access_log off;
        }
        location = /robots.txt {
                log_not_found off; access_log off; allow all;
        }
        location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
                expires max;
                log_not_found off;
        }
}
```

Đóng và lưu file để cập nhật cấu hình.

**Bước 6: Tải xuống và khởi chạy các container đã khai báo**

Bạn chạy lệnh này để tiến hành tải các image cũng như tạo và khởi chạy các container từ các image đó:

```bash
docker-compose up -d
```

**Bước 7: Kiểm tra kết quả**

Trong ví dụ này thì mình có cài đặt Wordpress và PHPMyAdmin nên sẽ tiến hành kiểm trả lần lượt

Do mình đã cấu hình Nginx Proxy nên nếu muốn vào giao diện Wordpress thì bạn chỉ cần truy cập **http://your_ip_or_domain** trên bất kì trình duyệt nào thì sẽ được giao diện như sau:

![image.png](/Images/tuan_5_docker/image%2014.png)

Đối với PHPMyAdmin mình có cấu hình port 8080 trong file .yml nên chỉ cần truy cập **http://your_id_or_domain:8080** là sẽ vào được giao diện PHPMyAdmin

![image.png](/Images/tuan_5_docker/image%2015.png)

Tới đây ta đã hoàn thành tìm hiểu cài đặt WordPress qua Docker Compose rồi

---

THE END