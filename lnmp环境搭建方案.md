# linux的常用命令

```bash
# <-- 文件操作常用命令 -->
$ rm  -rf   test/ # 删除目录 test，-f 不管该目录下是否有子目录或文件，都直接删除,无需逐一确认。
$ rm  -r  * # 删除当前目录下的所有文件及目录
$ tar -xvf file.tar #解压 tar包
$ tar -xzvf file.tar.gz #解压tar.gz
$ mv aaa bbb # 将 文件 aaa 更名为 bbb
$ mv /usr/student/*  .  # 将/usr/student下的所有文件和目录移到当前目录下
$ mkdir name #新建文件夹

# <-- yum常用命令 -->
$ yum repolist #列出设定yum源信息
$ yum repolist all #查看您拥有的仓库
$ yum-config-manager --disable “仓库名” #禁用仓库
$ yum-config-manager --enable “仓库名” #启用仓库
$ yum clean all #清除缓存
$ yum makecache #构建缓存
$ yum install package1 package2... #安装
$ yum reinstall package #重新安装
$ yum remove package #卸载
$ yum update package #更新
$ yum downgrage package #降级
$ yum check-update #检查可用的更新
$ yum deplist package1  #查看依赖性
$ rpm -ql name #查找安装包的安装路径

# <-- vi常用命令 -->
# 全部删除：按esc键后，先按gg（到达顶部），然后dG
# 全部复制：按esc键后，先按gg，然后ggyG
# 全选高亮显示：按esc键后，先按gg，然后ggvG或者ggVG
# 单行复制：按esc键后, 然后yy
# 单行删除：按esc键后, 然后dd
# 粘贴：按esc键后, 然后p
```

# aliyun yum源的设置

```bash
# <-- centos 6 -->
# 备份当前的yum源
$ mv /etc/yum.repos.d /etc/yum.repos.d.backup 
# 下载阿里云的yum源配置,从http://mirrors.aliyun.com/repo/ 中找到和你的linux版本相同的
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
# 重建缓存
$ yum clean all && yum makecache
```



# Nginx1.15的编译步骤

```bash
# <-- centos6 / mysql57-->
# 安装编译工具及库文件
$ yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
# 安装 PCRE,让 Nginx 支持 Rewrite 功能
$ cd /usr/local/src/
$ wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
$ tar zxvf pcre-8.35.tar.gz
$ cd pcre-8.35
$ ./configure
$ make && make install
# 查看 pcre 版本
$ pcre-config --version 
# 编译安装 Nginx
$ cd /usr/local/src/
$ wget http://nginx.org/download/nginx-1.15.9.tar.gz
$ tar zxvf nginx-1.15.9.tar.gz
$ cd nginx-1.15.9
$ ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
$ make && make install
# 查看 nginx 版本
$ /usr/local/webserver/nginx/sbin/nginx -v 
# 创建 Nginx 运行使用的用户 www
$ /usr/sbin/groupadd www 
$ /usr/sbin/useradd -g www www 
# 配置nginx.conf
$ vi /usr/local/webserver/nginx/conf/nginx.conf
# -------------------需要写入nginx.conf配置----------------------
# -------------------------------------------------------------
# 检查配置文件nginx.conf
$ /usr/local/webserver/nginx/sbin/nginx -t 
# 启动 Nginx
$ /usr/local/webserver/nginx/sbin/nginx 
```

# MySQL5.7的安装步骤

```bash
# <-- centos6 / mysql57-->
# 访问https://dev.mysql.com/downloads/repo/yum/存储库 ，下载安装yum源
$ wget https://dev.mysql.com/get/mysql80-community-release-el6-2.noarch.rpm
$ rpm -Uvh mysql80-community-release-el6-2.noarch.rpm
# 选择发布系列,默认只开启mysql80，以下启用mysql57，禁用mysql80
$ yum-config-manager --disable mysql80-community 
$ yum-config-manager --enable mysql57-community 
# 安装MySQL
$ yum install mysql-community-server
# 启动MySQL
$ service mysqld start
# 初始密码显示
$ grep 'temporary password' /var/log/mysqld.log
# 修改root密码，至少一个大写字母，一个小写字母，一个数字和一个特殊字符，至少8个字符。
$ mysql -uroot -p
mysql> $ ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

```

# PHP7.3的安装步骤

```bash
# <-- centos6 / PHP 7.3.3 -->
$ yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
$ yum install http://rpms.remirepo.net/enterprise/remi-release-6.rpm
$ yum install yum-utils
$ yum-config-manager --enable remi-php73
$ yum update
$ yum search php73 | more
$ yum install php73 php73-php-fpm php73-php-gd php73-php-json php73-php-mbstring php73-php-mysqlnd php73-php-xml php73-php-xmlrpc php73-php-opcache
# <-- 配置PHP与nginx的用户名一致 - vi /etc/opt/remi/php73/php-fpm.d/www.conf 
user = www
group = www
listen.owner = www
listen.group = www 
# 启动PHP fpm服务
$ service php73-php-fpm start 

```

```php
# 测试脚本
<?php
  // test script for CentOS/RHEL 7+PHP 7.2+Nginx 
  phpinfo();
?>
```

# nginx/mysql/php的常用命令及配置目录

```bash
# <-- Nginx命令 -->
$ /usr/local/webserver/nginx/sbin/nginx                    # 启动 Nginx
$ /usr/local/webserver/nginx/sbin/nginx -s stop            # 停止 Nginx
$ /usr/local/webserver/nginx/sbin/nginx -s reopen          # 重启 Nginx
$ /usr/local/webserver/nginx/sbin/nginx -s reload          # 重新载入配置文件
# <-- mysql命令 -->
$ service mysqld start # 启动MySQL
$ mysql -uroot -p # 根据用户名密码，登录数据库
$ create database <数据库名>; # 创建数据库
$ drop database <数据库名>; # 删除数据库
$ show databases; # 显示所有数据库
# <-- PHP fpm命令 -->
$ service php73-php-fpm start    # 启动PHP fpm服务
$ service php73-php-fpm stop     # 停止PHP fpm服务
$ service php73-php-fpm restart  # 重启PHP fpm服务
$ service php73-php-fpm          # 获取PHP fpm服务的状态
$ chkconfig php73-php-fpm on     # 启用开机自启
$ chkconfig php73-php-fpm off    # 禁用开机自启
$ php73 -v                       # 版本查看
$ php73 --modules                # 查看安装的模块

# <-- Nginx路径 -->
$ /usr/local/webserver/nginx/html # 站点目录
$ /usr/local/webserver/nginx/conf/nginx.conf # 配置nginx.conf
 # <-- PHP路径 -->
$ /etc/opt/remi/php73/php-fpm.d/www.conf #配置www.conf
```
# 内存对象缓存系统的编译步骤

```bash
# <-- memcached -->
$ yum install libevent-devel
$ wget https://memcached.org/latest
# 需要换名称 ！！！！！
$ tar -zxf memcached-1.x.x.tar.gz
$ cd memcached-1.x.x
$ ./configure --prefix=/usr/local/memcached
$ make && make test && sudo make install
# 后台启动
$ /usr/local/memcached -d -m 10m -p 11211 -u root 
```

# wordpress出现ftp问题的解决方案

第一种：

给我们的文件赋予权限：

```bash
chown -R www usr/local/webserver/nginx/html
chmod -R 775 usr/local/webserver/nginx/html
chmod -R 777 usr/local/webserver/nginx/html/wp-content/
```

第二种：

在网站根目录下，找到wp-config.php文件并添加以下代码：（加到最后就行）

```php
define("FS_METHOD", "direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);
```

# 已安装软件的路径查找

```bash
# 1、首先安装一个redis
$ yum install redis
# 2、查找redis的安装包
$ rpm -qa|grep redis
redis-3.2.10-2.el7.x86_64
# 3、查找安装包的安装路径
$ rpm -ql redis-3.2.10-2.el7.x86_64
```