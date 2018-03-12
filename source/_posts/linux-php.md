---
title: linux php环境安装与配置以及eoLinker安装
date: 2017-07-05 15:12:56
tags:
---



## 安装 XAMPP

```
下载包
wget https://www.apachefriends.org/xampp-files/7.2.0/xampp-linux-x64-7.2.0-0-installer.run


写权限
chmod 755 xampp-linux-*-installer.run

安装
sudo ./xampp-linux-*-installer.run

```

#### start XAMPP


```
sudo /opt/lampp/lampp start
```

如果如下报错 请安装 

```
XAMPP: Starting Apache...already running.
XAMPP: Starting MySQL.../opt/lampp/share/xampp/xampplib:行22: netstat: 未找到命令
ok.
XAMPP: Starting ProFTPD.../opt/lampp/share/xampp/xampplib:行22: netstat: 未找到命令
ok.
```

请安装 

```
yum -y install net-tools
```


### You should now see something like this on your screen:

```
Starting XAMPP 1.8.2...

LAMPP: Starting Apache...

LAMPP: Starting MySQL...

LAMPP started.

Ready. Apache and MySQL are running.

```


#### stop XAMPP


To stop XAMPP simply call this command:

```
sudo /opt/lampp/lampp stop
```


You should now see something like this on your screen:

```

Stopping XAMPP 1.8.2...

LAMPP: Stopping Apache...

LAMPP: Stopping MySQL...

LAMPP stopped.

```



### 初始化 密码：

```
/opt/lampp/lampp security
```

下面是过程：

```
# /opt/lampp/lampp security
XAMPP:  Quick security check...
XAMPP:  MySQL is accessable via network.
XAMPP: Normaly that's not recommended. Do you want me to turn it off? [yes]
XAMPP:  Turned off.
XAMPP: Stopping MySQL...ok.
XAMPP: Starting MySQL...ok.
XAMPP:  The MySQL/phpMyAdmin user pma has no password set!!!
XAMPP: Do you want to set a password? [yes]
XAMPP: Password:
XAMPP: Password (again):
XAMPP:  Setting new MySQL pma password.
XAMPP:  Setting phpMyAdmin's pma password to the new one.
XAMPP:  MySQL has no root passwort set!!!
XAMPP: Do you want to set a password? [yes]
XAMPP:  Write the password somewhere down to make sure you won't forget it!!!
XAMPP: Password:
XAMPP: Password (again):
XAMPP:  Setting new MySQL root password.
XAMPP:  Change phpMyAdmin's authentication method.
XAMPP:  The FTP password for user 'daemon' is still set to 'xampp'.
XAMPP: Do you want to change the password? [yes]
XAMPP: Password:
XAMPP: Password (again):
XAMPP: Reload ProFTPD...ok.
XAMPP:  Done.

```

#### 修改数据库密码

```
/opt/lampp/bin/mysqladmin --user=root password "ECMA(root2018)."
```

允许数据库 远程登录

```
//链接数据库 //新安装的数据库是没有密码的，这里直接按下回车就行 如果有密码则输入密码
mysql -u root -p
//设置密码 设置过了 略过
set password for 'root'@'localhost' = password('xuess'); 

//为其他主机远程连接数据库开放访问权限，重新登入数据库：
//选择mysql数据库进行操作
use mysql; 
 
//查看user,password,host这三个字段的权限分配情况
select user,password,host from user;  

+------+-------------------------------------------+-----------+
| user | password                                  | host      |
+------+-------------------------------------------+-----------+
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | localhost |
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | 127.0.0.1 |
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | ::1       |
|      |                                           | localhost |
| pma  | *7723D1540A6840E272E047B564098468B7DB9BD8 | localhost |
+------+-------------------------------------------+-----------+


//通过以上输出可以看出数据库默认只允许用户root在本地服务器（localhost）上登录，不允许其他主机远程连接。

//修改root 链接权限 *xuess* 是设置好的密码
//这条语句将允许用户root使用密码(xuess)在任何主机上连接该数据库，并赋予该用户所有权限。
MariaDB [mysql]> grant all privileges on *.* to root@"%" identified by "xuess";  
//出现下面说明 生效
> Query OK, 0 rows affected (0.01 sec)

//执行 把权限缓存到 内存中 
flush privileges

//再查询一下
select user,password,host from user;
+------+-------------------------------------------+-----------+
| user | password                                  | host      |
+------+-------------------------------------------+-----------+
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | localhost |
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | 127.0.0.1 |
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | ::1       |
|      |                                           | localhost |
| pma  | *7723D1540A6840E272E047B564098468B7DB9BD8 | localhost |
| root | *7723D1540A6840E272E047B564098468B7DB9BD8 | %         |
+------+-------------------------------------------+-----------+


//上面中的“%”就意味着任何主机都被允许连接数据库

如果远程还链接不上，请关闭防火墙

sudo systemctl stop firewalld.service && sudo systemctl disable firewalld.service


```


#### 目录说明

```
/opt/lampp/bin/	XAMPP 命令库。例如 /opt/lampp/bin/mysql 可执行 MySQL 监视器。
/opt/lampp/htdocs/	Apache 文档根目录。
/opt/lampp/etc/httpd.conf	Apache 配置文件。
/opt/lampp/etc/my.cnf	MySQL 配置文件。
/opt/lampp/etc/php.ini	PHP 配置文件。
/opt/lampp/etc/proftpd.conf	ProFTPD 配置文件。（从 0.9.5 版开始）
/opt/lampp/phpmyadmin/config.inc.php phpMyAdmin 配制文件。

```


## eoLinker 安装


```
打开 文档目录
cd /opt/lampp/htdocs/

创建目录
mkdir eolinker

进入
cd eolinker

从git下载eoLinker 安装包 
wget https://github.com/eolinker/eoLinker-API-Management-System-OS-3.X/raw/master/release%5B%E6%AD%A3%E5%BC%8F%E5%AE%89%E8%A3%85%E5%8C%85%5D/eolinker_os_3.2.1.zip


解压：如果没有 unzip 命令 请安装 yum install -y unzip zip
unzip eolinker_os_3.2.1.zip

赋权限
cd ../
chmod 777 -R  eolinker/
或者
chmod 777 server/RTP/config/


创建数据库
cd /opt/lampp/bin/	

./mysql -u root -p
然后 输入数据库密码


然后进入 mysql，在 mysql 环境下 创建数据库 eolinker_os 命令
create database eolinker_os;


然后访问页面  配置和安装

```



由于服务器设置了xampp不允许远程访问，所以远程不能访问需要修改conf文件

```
vi /opt/lampp/etc/extra/httpd-xampp.conf
```

将`Require local`改成`Require all granted`

```
/opt/lampp/lampp restart 
重启xampp
```
到此xampp安装完成