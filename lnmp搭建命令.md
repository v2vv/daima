# Nginx1.15的编译步骤

```bash
# <-- centos 6 / nginx1.15 -->
# 安装编译工具及库文件
 yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
# 安装 PCRE,让 Nginx 支持 Rewrite 功能
 cd /usr/local/src/
 wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
 tar zxvf pcre-8.35.tar.gz
 cd pcre-8.35
 ./configure
 make && make install
# 查看 pcre 版本
 pcre-config --version 
# 安装 Nginx
 cd /usr/local/src/
 wget http://nginx.org/download/nginx-1.15.9.tar.gz
 tar zxvf nginx-1.15.9.tar.gz
 cd nginx-1.15.9
 ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
 make && make install
# 查看 nginx 版本
 /usr/local/webserver/nginx/sbin/nginx -v 
# 创建 Nginx 运行使用的用户 www
 /usr/sbin/groupadd www 
 /usr/sbin/useradd -g www www 
# 配置nginx.conf ，将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容
 vi /usr/local/webserver/nginx/conf/nginx.conf
 
# ------------------------ 输入nginx配置文件 ----------------------
 
 
# ---------------------------------------------------------------

# 检查配置文件nginx.conf
 /usr/local/webserver/nginx/sbin/nginx -t 
# 启动 Nginx
 /usr/local/webserver/nginx/sbin/nginx 
```

# MySQL的安装步骤

```bash
# <-- centos 6 / mysql57-->
# 访问https://dev.mysql.com/downloads/repo/yum/存储库 ，下载安装yum源
 wget https://dev.mysql.com/get/mysql80-community-release-el6-2.noarch.rpm
 rpm -Uvh mysql80-community-release-el6-2.noarch.rpm
# 选择发布系列,默认只开启mysql80，以下启用mysql57，禁用mysql80
 yum-config-manager --disable mysql80-community 
 yum-config-manager --enable mysql57-community 
# 安装MySQL
 yum install mysql-community-server
 y
# 启动MySQL
 service mysqld start
# 初始密码显示
 grep 'temporary password' /var/log/mysqld.log
# 修改root密码，至少一个大写字母，一个小写字母，一个数字和一个特殊字符，至少8个字符。
 mysql -uroot -p
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

# PHP7.3的安装步骤

```bash
# <-- centos 6 / PHP 7.3.3 -->
 sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
 y
 sudo yum install http://rpms.remirepo.net/enterprise/remi-release-6.rpm
 y
 sudo yum install yum-utils
 y
 sudo yum-config-manager --enable remi-php73
 sudo yum update
 y
 sudo yum install php73 php73-php-fpm php73-php-gd php73-php-json php73-php-mbstring php73-php-mysqlnd php73-php-xml php73-php-xmlrpc php73-php-opcache
 y
 y
# <-- 配置PHP与nginx的用户名一致 --> 
 sudo vi /etc/opt/remi/php73/php-fpm.d/www.conf 
 
----------------------------改变用户名--------------------
user = www
group = www
listen.owner = www
listen.group = www 
---------------------------------------------------------

# 启动PHP fpm服务
 service php73-php-fpm start 

```
# php测试脚本

```php
# 在站点目录写入测试脚本 
$ vi /usr/local/webserver/nginx/html/foo.php 
<?php
  // test script for CentOS/RHEL 7+PHP 7.2+Nginx 
  phpinfo();
?>
```
# nginx配置文件

```bash
user www www;
worker_processes 1; #设置值和CPU核心数一致
error_log /usr/local/webserver/nginx/logs/nginx_error.log crit; #日志位置和日志级别
pid /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;
events
{
  use epoll;
  worker_connections 65535;
}
http
{
  include mime.types;
  default_type application/octet-stream;
  log_format main  'remote_addr - remote_user [time_local] "request" '
               'status body_bytes_sent "http_referer" '
               '"http_user_agent" http_x_forwarded_for';
  
#charset gb2312;
     
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 8m;
     
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 60;
  tcp_nodelay on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  gzip on; 
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
 
 #limit_zone crawler binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录
      location ~ .*\.(php|php5)?
    {
      #fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)
    {
      expires 30d;
  # access_log off;
    }
    location ~ .*\.(js|css)?
    {
      expires 15d;
   # access_log off;
    }
    access_log off;
  }

}
```

