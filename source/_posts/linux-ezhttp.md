---
title: linux 一键安装环境ezhttp
date: 2017-11-19 15:10:37
tags: linux,vps
---



```bash
wget --no-check-certificate centos.bz/ezhttp.zip?time=$(date +%s) -O ezhttp.zip
unzip ezhttp.zip
cd ezhttp-master
chmod +x start.sh

```


交互安装: (自己选择)

```bash
./start.sh
```


## 交互安装

### 1.选择安装lnmp
1） 输入1回车进入`Installation of stack`.
2） 输入1回车选择`LNMP(Nginx MySQL PHP)`安装.


### 2.nginx安装设置
1）首先是nginx版本选择。
这里有5个选项：

```bash
1) nginx-1.8.0
2) tengine-2.1.0
3) openresty-1.9.7.3
4) custom_version
5) do_not_install
```
输入一个1-5的数字或直接回车，直接回车默认选择5。
第1-3个选项是选择指定nginx，包含nginx官方版本,淘宝的tengine和整合nginx luajit的openresty。
第4个选项是指定版本号，输入的规则为nginx-1.4.1 tengine-1.4.6 ngx_openresty-1.2.8.3。
第5个选择是
这里我们选择nginx-1.8.0。
2）然后是输入安装路径
直接回车默认是/usr/local/nginx。我们可以更改其安装路径，如输入/opt/nginx。 这里我们直接回车使用默认值。
3）接着将会显示安装nginx使用的编译参数
且提示是否更改编译参数，直接回车默认是不更改。输入y是更改参数，n为不更改参数。这里我们直接回车，即不更改。
4）最后是提示是否安装nginx模块
默认为不安装，即n。输入y则安装，n为不安装。我们这里输入y，安装模块。接着将显示可安装的nginx模块，比如

```
1) lua-nginx-module-0.10.0
2) nginx-http-concat-1.2.2
3) nginx-upload-module-2.2
4) ngx_http_substitutions_filter_module-0.6.4
5) do_not_install
```

输入对应的数字选择安装的模块或输入5不安装。这里我们输入2安装`nginx-http-concat-1.2.2`模块。




### 3.mysql安装配置

1) 选择安装的mysql版本
1-4选项为mysql5.1,mysql5.5,mysql5.6,mysql5.7版本，5为libmysqlclient18，6为自定义版本，格式为mysql-5.1.71 mysql-5.5.32 mysql-5.6.12 mysql-5.7.9。这里我们输入3安装mysql5.6。
2）输入mysql安装路径
直接回车默认路径为/usr/local/mysql，可以输入其它安装路径。这里我们直接回车选择默认的/usr/local/mysql。
3) 输入mysql数据目录
直接回车为默认的{上面设置的mysql安装路径}/data。可以输入其它的，如/data/mysql。我们直接回车选择默认的/usr/local/mysql/data。
4) 输入mysql端口
直接回车默认使用3306端口。可以输入任意一个有效的端口，如3307。我们直接回车选择默认的3306端口。
5) 设置mysql root用户密码
直接回车默认设置密码为root。可以输入任意字符串的密码，这里我们输入mysqlpwd。
6）设置mysql编译参数
直接回车默认不更改。可以输入y进行更改或n不更改。


### 4.php安装配置
1) 选择安装的php版本
支持php5.2,php5.3,php5.4,php5.5,php5.6,php7.1的版本。输入对应的数字安装对应的版本。也可以选择custom_version自定义版本。这里输入5安装php5.6版本。
2) 设置安装路径
直接回车默认选择/usr/local/php路径，可以输入其它的路径，如/opt/php。这里直接驾车。
3) 更改编译参数
直接回车选择不更改。
4）安装php模块
将会列表可安装的php模块，安装多个模块输入以空格分隔的数字，如1 2 3。这里输入2 4安装memcache和redis模块。


### 5.其它软件安装
将会列出可安装的软件，安装多个软件输入以空格分隔的多个数字，如1 2 3。这里输入1 4安装memcached和redis。
然后接着要求输入各自的安装路径，这里我们直接回车使用默认值。

### 6.检查设置
最后将列出以上的所有设置。直接回车和输入y开始安装或输入n返回重新设置。



## 非交互安装
非交互安装即不需要手动选择或输入各种配置进行安装，可需要一个命令就行。

可以执行`./start.sh` `-h`查看帮助。以上的lnmp配置参数可以使用如下命令进行非交互安装。

```bash
./start.sh --stack=lnmp --package=nginx,php5.6,mysql5.6,memcached,redis --nginx-module=nginx-http-concat --mysql-root-pwd=mysqlpwd --redis-maxmem=2g
```

### `ez`命令介绍
`ezhttp提供了一个ez命令来对环境进行操作。用法如下：


虚拟主机管理

```bash
ez vhost add：创建虚拟主机
ez vhost list:	列出所有虚拟主机
ez vhost del：	删除虚拟主机
```

mysql管理

```bash
ez mysql reset：重置mysql root用户密码
ez mysql add：创建mysql用户
ez mysql mod：更新mysql用户
ez mysql del：删除mysql用户
```

ftp管理

```bash
ez ftp add：添加ftp用户
ez ftp list：列出所有ftp用户
ez ftp del：删除ftp用户
ez ftp mod：更改ftp用户
```


进程管理

```bash
nginx：/etc/init.d/nginx (start|stop|restart)
apache：/etc/init.d/httpd (start|stop|restart)
php-fpm：/etc/init.d/php-fpm (start|stop|restart)
mysql：/etc/init.d/mysqld (start|stop|restart)
```





```hash
#############################################################################

You are welcome to use this script to deploy your linux,hope you like.
The script is written by Zhu Maohai.
If you have any question.
please visit http://www.centos.bz/ezhttp/ and submit your issue.thank you.

############################################################################

1) Installation of stack.
2) Some Useful Tools.
3) Upgrade Software
4) Exit.

please select: 1
you select Installation of stack.
1) LNMP(Nginx MySQL PHP)
2) LAMP(Apache MySQL PHP)
3) LNAMP(Nginx Apache MySQL PHP)
4) back to main menu

please input the package you like to install: 3
#################### nginx setting ####################


1) nginx-1.10.3
2) tengine-2.1.0
3) openresty-1.11.2.2
4) custom_version
5) do_not_install

which nginx you'd select(default do_not_install): 1
your selection: nginx-1.10.3
nginx-1.10.3 install location(default:/usr/local/nginx,leave blank for default):
nginx-1.10.3 install location: /usr/local/nginx
the nginx-1.10.3 configure parameter is:
--prefix=/usr/local/nginx --with-http_ssl_module --with-openssl=/root/ezhttp-master/soft/openssl-1.0.2h  --with-http_sub_module --with-http_stub_status_module --with-pcre-jit --with-pcre --with-pcre=/root/ezhttp-master/soft/pcre-8.33 --with-zlib=/root/ezhttp-master/soft/zlib-1.2.8 --with-http_secure_link_module


Would you like to change it?[N/y](default n):
you select no,configure parameter will not be changed.

Do you need to install nginx module?[N/y](default n): y
#################### nginx_modules install ####################

1) lua-nginx-module-0.10.7
2) nginx-http-concat-1.2.2
3) nginx-upload-module-2.2
4) ngx_http_substitutions_filter_module-0.6.4
5) ngx_stream_core_module
6) nginx_upstream_check_module-master
7) nginx-stream-upsync-module-master
8) do_not_install

please input one or more number between 1 and 8(default do_not_install)(ie.1 2 3): 2 3
your selection nginx-http-concat-1.2.2 nginx-upload-module-2.2
#################### apache setting ####################


1) httpd-2.2.31
2) httpd-2.4.25
3) custom_version
4) do_not_install

which apache you'd select(default do_not_install): 1
your selection: httpd-2.2.31
httpd-2.2.31 install location(default:/usr/local/apache,leave blank for default):
httpd-2.2.31 install location: /usr/local/apache
the httpd-2.2.31 configure parameter is:
--prefix=/usr/local/apache --with-included-apr --enable-so --enable-deflate=shared --enable-expires=shared  --enable-ssl=shared --enable-headers=shared --enable-rewrite=shared --enable-static-support


Would you like to change it?[N/y](default n):
you select no,configure parameter will not be changed.
#################### mysql setting ####################


1) mysql-5.1.73
2) mysql-5.5.54
3) mysql-5.6.35
4) mysql-5.7.17 (need about 2GB RAM when building,try mysql-5.6 if failed)
5) libmysqlclient18
6) custom_version
7) do_not_install

which mysql you'd select(default do_not_install): 3
your selection: mysql-5.6.35
mysql-5.6.35 install location(default:/usr/local/mysql,leave blank for default):
mysql-5.6.35 install location: /usr/local/mysql
mysql data location(default:/usr/local/mysql/data,leave blank for default):
mysql-5.6.35 data location: /usr/local/mysql/data
mysql port number(default:3306,leave blank for default):
mysql port number: 3306
mysql server root password (default:root,leave blank for default): c5bMaVD818
mysql-5.6.35 root password: c5bMaVD818
the mysql-5.6.35 configure parameter is:
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DSYSCONFDIR=/usr/local/mysql/etc -DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=complex -DENABLED_LOCAL_INFILE=1


Would you like to change it?[N/y](default n):
you select no,configure parameter will not be changed.
#################### php setting ####################


1) php-5.2.17
2) php-5.3.29
3) php-5.4.43
4) php-5.5.27
5) php-5.6.15
6) php-7.1.0
7) custom_version
8) do_not_install

which php you'd select(default do_not_install): 5
your selection: php-5.6.15
php-5.6.15 install location(default:/usr/local/php,leave blank for default):
php-5.6.15 install location: /usr/local/php
the php-5.6.15 configure parameter is:
--prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-apxs2=/usr/local/apache/bin/apxs --enable-bcmath=shared --with-pdo_sqlite --with-gettext=shared --with-iconv --enable-ftp=shared --with-sqlite --with-sqlite3 --enable-mbstring=shared --enable-sockets=shared --enable-zip --enable-soap=shared --with-openssl --with-zlib --with-curl=shared --with-gd=shared --with-jpeg-dir --with-png-dir --with-freetype-dir --with-mcrypt=shared,/opt/ezhttp/libmcrypt-2.5.8 --with-mhash=shared,/opt/ezhttp/mhash-0.9.9.9 --enable-opcache --with-mysql=mysqlnd --with-mysqli=shared,mysqlnd --with-pdo-mysql=shared,mysqlnd --without-pear --with-libdir=lib64 --disable-fileinfo


Would you like to change it?[N/y](default n):
you select no,configure parameter will not be changed.
#################### PHP modules install ####################
php-5.6.15 version available modules:

#################### php_modules install ####################

1) php-imagick-3.1.2
2) php-memcache-3.0.8
3) php-memcached-2.2.0 (Support Aliyun OCS)
4) php-redis-2.2.7
5) php-mongo-legacy-1.6.11
6) xdebug-2.2.2
7) mssql
8) fileinfo
9) php-gmp
10) php-swoole-1.7.20
11) do_not_install

please input one or more number between 1 and 11(default do_not_install)(ie.1 2 3):
your selection do_not_install
#################### other_soft install ####################

1) memcached-1.4.24
2) pure-ftpd-1.0.41
3) phpMyAdmin-4.4.12-all-languages
4) redis-3.0.3
5) mongodb-linux-x86_64-2.4.9
6) phpRedisAdmin-1.1.0
7) memadmin-1.0.12
8) rockmongo-1.1.6-fix-auth
9) jdk1.7.0_79
10) jdk1.8.0_66
11) apache-tomcat-7.0.68
12) apache-tomcat-8.0.39
13) do_not_install

please input one or more number between 1 and 13(default do_not_install)(ie.1 2 3): 4 5
your selection redis-3.0.3 mongodb-linux-x86_64-2.4.9
input redis-3.0.3 location(default:/usr/local/redis):
redis location: /usr/local/redis
please input the max memory allowed for redis(ie.128M,512m,2G,4g):
input errors,please input ie.128M,512m,2G,4g
please input the max memory allowed for redis(ie.128M,512m,2G,4g): 128M
128M
input mongodb-linux-x86_64-2.4.9 location(default:/usr/local/mongodb):
mongodb location: /usr/local/mongodb
input mongodb-linux-x86_64-2.4.9 data location(default:/usr/local/mongodb/data/):
mongodb data location: /usr/local/mongodb/data/
#################### your choice overview ####################

Package: lnamp

*****Nginx Setting*****
Nginx: nginx-1.10.3
Nginx Location: /usr/local/nginx
Nginx Configure Parameter: --prefix=/usr/local/nginx --with-http_ssl_module --with-openssl=/root/ezhttp-master/soft/openssl-1.0.2h  --with-http_sub_module --with-http_stub_status_module --with-pcre-jit --with-pcre --with-pcre=/root/ezhttp-master/soft/pcre-8.33 --with-zlib=/root/ezhttp-master/soft/zlib-1.2.8 --with-http_secure_link_module --add-module=/root/ezhttp-master/soft/nginx-http-concat-1.2.2 --add-module=/root/ezhttp-master/soft/nginx-upload-module-2.2
Nginx Modules:  nginx-http-concat-1.2.2 nginx-upload-module-2.2

*****Apache Setting*****
Apache: httpd-2.2.31
Apache Location: /usr/local/apache
Apache Configure Parameter: --prefix=/usr/local/apache --with-included-apr --enable-so --enable-deflate=shared --enable-expires=shared  --enable-ssl=shared --enable-headers=shared --enable-rewrite=shared --enable-static-support

*****MySQL Setting*****
MySQL Server: mysql-5.6.35
MySQL Location: /usr/local/mysql
MySQL Data Location: /usr/local/mysql/data
MySQL Port Number: 3306
MySQL Root Password: c5bMaVD818
MySQL Configure Parameter: -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DSYSCONFDIR=/usr/local/mysql/etc -DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=complex -DENABLED_LOCAL_INFILE=1

*****PHP Setting*****
PHP: php-5.6.15
PHP Location: /usr/local/php
PHP Configure Parameter: --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-apxs2=/usr/local/apache/bin/apxs --enable-bcmath=shared --with-pdo_sqlite --with-gettext=shared --with-iconv --enable-ftp=shared --with-sqlite --with-sqlite3 --enable-mbstring=shared --enable-sockets=shared --enable-zip --enable-soap=shared --with-openssl --with-zlib --with-curl=shared --with-gd=shared --with-jpeg-dir --with-png-dir --with-freetype-dir --with-mcrypt=shared,/opt/ezhttp/libmcrypt-2.5.8 --with-mhash=shared,/opt/ezhttp/mhash-0.9.9.9 --enable-opcache --with-mysql=mysqlnd --with-mysqli=shared,mysqlnd --with-pdo-mysql=shared,mysqlnd --without-pear --with-libdir=lib64 --disable-fileinfo

*****Other Software Setting*****
Other Software:  redis-3.0.3 mongodb-linux-x86_64-2.4.9
redis_location: /usr/local/redis
mongodb_location: /usr/local/mongodb

##############################################################

Are you ready to configure your Linux?[Y/n](default y):


```



apache 文件目录

```
/usr/local/apache/htdocs

```


初始化
安装 unzip zip 命令

```
yum install -y unzip zip
```

