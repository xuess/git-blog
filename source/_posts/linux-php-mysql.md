---
title: php+mysql等常用工具环境搭建
date: 2017-10-30 15:17:11
tags: linux
---



### 1使用yum命令，安装所需的程序库

```
yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers
                
```

### 2安装 nginx

```
//安装命令
 yum install -y nginx 
 
//验证启动
service nginx start

//查看状态
service nginx status
 
```


### 3安装PHP及若干扩展

```
yum install -y php php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt libmcrypt-devel php-fpm

```

### 4启动php-fpm

```
/etc/rc.d/init.d/php-fpm start 
```


### 5设置开机启动项

```
chkconfig php-fpm on
```

### 安装mysql

```

wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server -y


启动 MySQL 服务：
service mysqld restart


设置 MySQL 账户 [root 密码]：
/usr/bin/mysqladmin -u root password 'Password4eoLinker'

```


#### 安装node使用npm工具

```

curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -

yum -y install nodejs

```


