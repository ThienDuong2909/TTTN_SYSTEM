# Tìm hiểu về Laravel

## Mục lục
- [Tìm hiểu về Laravel](#tìm-hiểu-về-laravel)
  - [Giới thiệu về Laravel](#giới-thiệu-về-laravel)
    - [Khái niệm](#khái-niệm)
    - [Tính năng nổi bật của Laravel](#tính-năng-nổi-bật-của-laravel)
  - [Xây dựng Website Laravel cơ bản (CRUD)](#xây-dựng-website-laravel-cơ-bản-crud)
    - [Chuẩn bị trước khi bắt đầu xây dựng website bằng Lavarel](#chuẩn-bị-trước-khi-bắt-đầu-xây-dựng-website-bằng-lavarel)
    - [Tiến hành xây dựng website](#tiến-hành-xây-dựng-website)
    - [Thao tác và kiểm tra](#thao-tác-và-kiểm-tra)
  - [Deploy ứng dụng lên Hosting](#deploy-ứng-dụng-lên-hosting)

# Giới thiệu về Laravel

## Khái niệm

**Laravel** là một framework mã nguồn mở được viết bằng ngôn ngữ PHP. Nó được tạo ra bởi Taylor Otwell vào năm 2011 và hiện tại đang được phát triển và duy trì bởi một cộng đồng lớn các nhà phát triển trên toàn thế giới.

## **Tính năng nổi bật của Laravel**

**Cú pháp đơn giản và dễ hiểu**

Một trong những điểm thu hút người dùng của Laravel chính là cú pháp đơn giản và dễ hiểu. Với việc sử dụng các câu lệnh ngắn gọn và rõ ràng, việc phát triển ứng dụng trở nên dễ dàng hơn bao giờ hết. Ngoài ra, Laravel còn hỗ trợ sẵn các thư viện và công cụ giúp cho việc xử lý các tác vụ phức tạp trở nên đơn giản hơn.

**Hỗ trợ MVC (Model-View-Controller)**

Laravel sử dụng mô hình MVC để tổ chức và quản lý mã nguồn. Điều này giúp cho việc phát triển ứng dụng trở nên có cấu trúc hơn và dễ bảo trì hơn. Ngoài ra, với việc tách biệt các thành phần của ứng dụng, Laravel cũng giúp cho việc phát triển độc lập giữa các thành viên trong nhóm.

**Tính năng Artisan**

Artisan là một công cụ dòng lệnh được tích hợp sẵn trong Laravel. Nó giúp cho việc tạo ra các câu lệnh và tác vụ tự động trở nên đơn giản hơn. Với Artisan, người dùng có thể tạo ra các file controller, model hay migration chỉ với một vài dòng lệnh đơn giản.

**Hệ thống định tuyến mạnh mẽ**

Laravel cung cấp cho người dùng một hệ thống định tuyến mạnh mẽ và linh hoạt. Người dùng có thể dễ dàng định nghĩa các route và xử lý các yêu cầu từ người dùng một cách dễ dàng. Ngoài ra, Laravel còn hỗ trợ các tính năng như middleware, middleware nhóm và middleware route để giúp cho việc xử lý các yêu cầu trở nên linh hoạt hơn.

**Hỗ trợ tích hợp với các thư viện bên ngoài**

Laravel có tính mở rộng cao và hỗ trợ tích hợp với nhiều thư viện bên ngoài như Bootstrap, jQuery hay Vue.js. Điều này giúp cho việc phát triển ứng dụng trở nên đa dạng và linh hoạt hơn. Ngoài ra, Laravel còn có tính tương thích cao với các hệ quản trị cơ sở dữ liệu phổ biến như MySQL, PostgreSQL hay MongoDB.

# Xây dựng Website Laravel cơ bản (CRUD)

## **Chuẩn bị trước khi bắt đầu xây dựng website bằng Lavarel**

Điều kiện đầu tiên để xây dựng một website bằng Lavarel là cần cài đặt các công cụ cần thiết như PHP, Composer, MySQL\MariaDB

Sau khi cài đặt xong những công cụ nói trên thì tiến hành khởi tạo dự án:

```jsx
composer create-project laravel/laravel ten_du_an
cd ten_du_an
```

Tiếp theo là cấu hình file **.env** để kết nối CSDL:

```jsx
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=yourDB
DB_USERNAME=username
DB_PASSWORD=password
```

Tạo Database để chứa dữ liệu website (nếu chưa có). Cần vào giao diện làm việc của MySQL\MariaDB hoặc một vài phần mềm khác để tạo DB bằng thao tác hoặc muốn tạo bằng lệnh thì chạy như sau:

```jsx
CREATE DATABASE yourDB;
```

## **Tiến hành xây dựng website**

Ở ví dụ này với mục tiêu là xây dựng một website đơn giản bằng Lavarel sử dụng CRUD để quản lý dữ liệu và sau đó deploy website đó lên hosting.

Mình chọn xây dựng một website đơn giản với các chức năng chính là thêm, sửa xóa và hiển thị những bài Post.

Đầu tiên là tạo model, migration và controller. Có thể tạo thủ công hoặc chạy lệnh sau:

```jsx
cd ten_du_an
php artisan make:model Post -mcr
```

Sau khi chạy lệnh trên thì Laravel sẽ tạo:

- `app/Models/Post.php`
- `database/migrations/xxxx_create_posts_table.php`
- `app/Http/Controllers/PostController.php`

Tiếp đến sẽ là khỏi tạo đối tương, Mở file `database/migrations/xxxx_create_posts_table.php`:

```jsx
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->text('content');
        $table->timestamps();
    });
}
```

Tổng thể file sẽ trông như thế này:

![image.png](/Images/tuan_5_laravel/image.png)

Sau khi khởi tạo migrate xong thì chạy lệnh sau để án xạ đối tượng tương ứng xuống Database, nếu như chưa có thì sẽ tự động tạo và ánh xạ luôn:

```jsx
php artisan migrate
```

Ta vào file mode/Post.php để thêm dòng `protected $fillable = ['title', 'content'];`

![image.png](/Images/tuan_5_laravel/image%201.png)

Tiếp tới ta sẽ vào file `routes\web.php` để cấu hình route:

![image.png](/Images/tuan_5_laravel/image%202.png)

Ở đây mình cấu hình route mặc định khi người dùng truy cập tới với path `/` thì sẽ gọi hàm `index` trong PostController và các path còn lại với bắt đầu bằng `/posts/**` thì do PostController xử lý 

Khi tạo file Controller bằng lệnh tạo đối tượng nhanh `php artisan make:model Post -mcr` thì mặc định trong file Controller sẽ tự khai báo các hàm rỗng như index, show, update, store, destroy rồi.

Trước tiên mình sẽ viết logic cho hàm index. ở hàm này khi gọi thì sẽ lấy ra danh sách toàn bộ bài Post có trong Database và trả giao diện `view/posts/index` kèm theo dữ liệu danh sách vừa lấy.

![image.png](/Images/tuan_5_laravel/image%203.png)

Tiếp theo là chức năng thêm bài Post mới, với hàm `create` trả về giao diện `view/posts/create` còn hàm `store` để lưu thông tin bài Post người dùng đã nhập vào. Sau khi tạo mới thì chuyển về lại giao diện `view/posts/index`

![image.png](/Images/tuan_5_laravel/image%204.png)

Đối với chức năng sửa bài Post cũng tương tự , hàm edit trả về giao diện và dữ liệu hiện có của bài Post, hàm update để lưu thông tin bài Post người dùng chỉnh sửa.

![image.png](/Images/tuan_5_laravel/image%205.png)

Cuối cùng là hàm destroy thực hiện chức năng xóa bài Post.

![image.png](/Images/tuan_5_laravel/image%206.png)

Bây giờ tới bước tạo giao diện, như đã khai báo trong các hàm trên, ta cần tạo những file tương ứng:

![image.png](/Images/tuan_5_laravel/image%207.png)

Đây là file app.blade.php để làm layout chung:

![image.png](/Images/tuan_5_laravel/image%208.png)

Đây là file create.blade.php:

![image.png](/Images/tuan_5_laravel/image%209.png)

Đây là file edit.blade.php:

![image.png](/Images/tuan_5_laravel/image%2010.png)

Đây là file index.blade.php:

![image.png](/Images/tuan_5_laravel/image%2011.png)

## Thao tác và kiểm tra

Sau khi xây dựng xong website bằng Lavarel như bước trên, để khởi chạy website chạy lệnh sau trong terminal của dự án:

```jsx
php artisan serve
```

![image.png](/Images/tuan_5_laravel/image%2012.png)

Mở trình duyệt bất kỳ, truy cập vào `http://127.0.0.1:8000` hoặc `http://localhost:8000` sẽ vào đc website đã làm.
Đây là giao diện mặc định khi truy cập, ta có thể thấy được tất cả bài Post hiện có

![image.png](/Images/tuan_5_laravel/image%2013.png)

Đây là giao diện thêm và sửa bài Post:

![image.png](/Images/tuan_5_laravel/image%2014.png)

![image.png](/Images/tuan_5_laravel/image%2015.png)

Đây là thông báo xác nhận khi nhấn xóa Post:

![image.png](/Images/tuan_5_laravel/image%2016.png)

# Deploy ứng dụng lên Hosting
### Đối với môi trường test (máy ảo)

Để deploy 1 website lên hosting thì đầu tiên ta chuẩn bị cần cài đặt sẵn PHP, MySQL/MariaDB và sẵn cài đặt luôn 1 webserver bất kì ở trên hosting, ở đây mình sẽ chọn Apache Webserver

Để cài đặt những công cụ cần thiết có thể chạy lệnh sau:

```jsx
sudo apt update
sudo apt upgrade
sudo apt install apache2 php php-cli php-mbstring php-xml php-bcmath php-curl php-mysql unzip curl git composer mysql-server
```

Tiếp theo cần tạo tài khoản đăng nhập và Database để lưu dữ liệu trên MySQL

 Có thể đăng tiến vào giao diện dòng lệnh MySQL thì chạy lệnh:

```jsx
sudo mysql
```

Nếu sử dụng phương thức đăng nhập bằng tài khoản thì là :

```jsx
sudo mysql -u username -p
```

Sau khi vào được giao diện dòng lệnh MySQL, tạo database lavarel hoặc tên bất kỳ để lưu trữ dữ liệu bằng lệnh:

```jsx
CREATE DATABASE lavarel;
```

Kiểm tra xem tạo thành công chưa:

```jsx
SHOW DATABASE;
```

![image.png](/Images/tuan_5_laravel/image%2017.png)

Cài đặt Composer (một Dependency Management, dùng để **quản lý các thư viện mà dự án PHP của bạn phụ thuộc vào**. Cụ thể, Composer sẽ giúp theo dõi, cài đặt và cập nhật các gói mã nguồn, thư viện cần thiết cho project.), bằng lệnh sau:

```jsx
wget -O composer-setup.php https://getcomposer.org/installer
```

```jsx
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

Kiểm tra lại bằng lệnh:

```jsx
composer -V
```

![image.png](/Images/tuan_5_laravel/image%2018.png)

Do source code mình để trên github nên mình sẽ clone về và chọn thư mục hợp lý để chứa source code, ở ví dụ này tôi để ở `/Downloads`:

```jsx

cd ~
cd /Downloads
git clone https://github.com/ThienDuong2909/Lavarel_CRUD
```

Tạo vào nhập thông tin vào file **.env:**

```jsx
sudo nano /Downloads/Lavarel_CRUD/.env
```

Thêm thông tin Database vào file **.env**:

![image.png](/Images/tuan_5_laravel/image%2019.png)

Tự động tạo các đối tượng ánh xạc từ source code PHP qua MySQL bằng lệnh:

```jsx
cd /Downloads/Lavarel_CRUD
php artisan migrate
```

Cấp quyền ghi cho các thư mục cần thiết:

```jsx
chmod o+x /home/username
chmod o+x /home/username/Downloads
```

```jsx
cd /Downloads/Lavarel_CRUD
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

 Kiểm tra `.htaccess` trong thư mục `public` ,nếu không có, tạo file mới với nội dung sau:

```jsx
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews -Indexes
    </IfModule>

    RewriteEngine On

    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]

    # Send Requests To Front Controller...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
</IfModule>
```

Tiếp đến là tạo file .conf trong /etc/apache2/sites-available/ bằng lệnh:

```jsx
sudo nano /etc/apache2/sites-available/your_domain
```

Cấu hình như sau:

```jsx
<VirtualHost *:80>
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com
    ServerAdmin your@gmail.com
    DocumentRoot /home/user/project/public
    <Directory /home/user/project/public>
       AllowOverride All
       Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Bạn có thể thêm cấu hình SSL và chuyển hướng:

```jsx
<VirtualHost *:80>
    ServerName lavarel.crud.com
    Redirect / https://lavarel.crud.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com
    ServerAdmin your@gmail.com
    DocumentRoot /home/user/project/public
    <Directory /home/user/project/public>
       AllowOverride All
       Require all granted
    </Directory>
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Bạn tạo file cấu hình là `yourdomain.com.conf`. Vậy bạn cần chắc chắn rằng bạn đã:

```jsx
sudo a2ensite yourdomain.com.conf
sudo a2dissite 000-default.conf
```

Khởi động lại và xem trạng thái của Apache Webserver:

```jsx
sudo systemctl restart apache2
```

```jsx
sudo systemctl status apache2
```

Sau khi cấu hình tất cả các bước trên xong thì trang web của bạn đã có thể hoạt động được rồi, để kiểm tra bạn hãy vào trình duyệt bất kỳ truy cập `https://your_domain_or_IP` 

![image.png](/Images/tuan_5_laravel/image%2020.png)

Đến đây là ta đã hoàn thành các thao tác Deploy dự án lên Hosting rồi
### Đối với môi trường thật (tương tác qua cPanel)

**Bước 1: Thêm domain (subdomain)**

Nhập từ khóa “**Domains**” trên thanh tìm kiếm → chọn **Domains**

![image.png](/Images/tuan_5_laravel/image%2021.png)

Nhấn **Create A New Domain**

![image.png](/Images/tuan_5_laravel/image%2022.png)

Nhập Domain cần tạo → chọn Thư mục lưu thông tin website **Document Root** → nhấn **Submit**

![image.png](/Images/tuan_5_laravel/image%2023.png)

Sau khi tạo xong, quay lại trang quản lý Domains → nhấn vào dòng Document Root vừa tạo

![image.png](/Images/tuan_5_laravel/image%2024.png)

Ta sẽ vào được File Manager của hosting này trên giao diện cPanel

**Bước 2: Tạo Database và Database Users**

Nhập trên thanh tìm kiếm **“Manage My Database” → chọn vào cái đầu tiên**

![image.png](/Images/tuan_5_laravel/image%2025.png)

Chúng ta đã vào được trang quản lý Database. Đầu tiên cần tạo Database để chứa dữ liệu của website
Ở mục **Create New Database** → nhập tên **Database** → nhấn **Create Database**

![image.png](/Images/tuan_5_laravel/image%2026.png)

Kéo xuống dưới mục **Add New User →** nhập thông tin tài khoản để đăng nhập vào MySQL → nhấn **Create User**

![image.png](/Images/tuan_5_laravel/image%2027.png)

Cuối cùng là thêm User vừa tạo vào Database vừa tạo

Kéo xuống mục **Add User To Database →** chọn **User** và **Database** cần thêm **User →** nhấn **Add**

![image.png](/Images/tuan_5_laravel/image%2028.png)

**Bước 3: Upload dữu liệu web lên hosting**

Nhập vào thanh tìm kiếm **“File Manager”** → chọn cái đầu tiên

![image.png](/Images/tuan_5_laravel/image%2029.png)

Ta sẽ vào được trang quản lý thư mục và file của hosting, ở đây có 2 thư mục cần chú ý đó là **`public`** và **`yourdomain` .** Thì public_html là thư mục chứa dữ liệu trang web ứng với domain chính (main domain), còn thư mục có tên `yourdomain` là thư mục chứa dữ liệu trang web ứng ới subdomain bạn vừa thêm . 

Tại ví dụ này, mình thực hành trên subdomain `thienduong.info` 

![image.png](/Images/tuan_5_laravel/image%2030.png)

**Chú ý:** khi nén file lại thì cần bỏ .git nếu bạn lưu source code trên github

![image.png](/Images/tuan_5_laravel/image%2031.png)

Mở thư mục để chứa dữ liệu website ra (thienduong.info) → nhấn **Upload →** tải lên ****file **.zip** chứa dữ liệu website vừa mới nén

![image.png](/Images/tuan_5_laravel/image%2032.png)

Kéo file vào ô hoặc nhấn Select File để chọn file .zip tải lên. Thời gian tải lên tương ứng với kích thước của file 

![image.png](/Images/tuan_5_laravel/image%2033.png)

![image.png](/Images/tuan_5_laravel/image%2034.png)

Trở ra thư mục yourdomain chọn file **.zip** vừa upload lên → nhấn **Extract** → chọn thư mục yourdomain làm nơi giải nén → nhấn **Extract Files**

![image.png](/Images/tuan_5_laravel/image%2035.png)

Giải nén xong ta chuột phải → chọn **Delete** → tích vô ô xóa vĩnh viễn → Xác nhận xóa

![image.png](/Images/tuan_5_laravel/image%2036.png)

![image.png](/Images/tuan_5_laravel/image%2037.png)

Cấu hình file .**env** → nhấp chuột phải file .**env** → chọn **Edit** → chỉnh sửa thông tin liên quan đến **Database** vừa tạo ở bước trên, thay đổi **APP_URL** bằng `http://yourdomain` hoặc `https://yourdomain` nếu có SSL rồi, **APP_DEBUG** thành **false →** xong thì nhấn **Save Changes** bên góc phải

![image.png](/Images/tuan_5_laravel/image%2038.png)

![image.png](/Images/tuan_5_laravel/image%2039.png)

![image.png](/Images/tuan_5_laravel/image%2040.png)

Kiểm tra trong thư mục public đã có file .htaccess chưa nếu chưa thì tạo mới và dán nội dung như sau:

```
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews -Indexes
    </IfModule>

    RewriteEngine On

    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]

    # Send Requests To Front Controller...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
</IfModule>

```

**Bước 4: Kiểm tra**

Truy cập vào trình duyệt và tìm kiếm `https://yourdomain` hoặc `http://yourdomain` nếu cấu hình đúng sẽ hiện ra giao diện website

Trong ví dụ của tôi thi, kết quả khi truy cập vào https://thienduong.info  sẽ là:

![image.png](/Images/tuan_5_laravel/image%2041.png)

---
THE END