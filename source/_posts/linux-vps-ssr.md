---
title: vps SSR安装教程
date: 2018-01-05 15:23:01
tags: vps
---



## SSR一键安装

```
wget –no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh

chmod +x shadowsocksR.sh

./shadowsocksR.sh 2>&1 | tee shadowsocksR.log


密码：password 端口：9527


Welcome to visit:https://shadowsocks.be/9.html
Enjoy it!
```

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh

chmod +x shadowsocksR.sh

./shadowsocksR.sh 2>&1 | tee shadowsocksR.log

Congratulations, ShadowsocksR server install completed!
Your Server IP        :  45.76.54.***
Your Server Port      :  9527
Your Password         :  password
Your Protocol         :  origin
Your obfs             :  plain
Your Encryption Method:  aes-256-cfb

Welcome to visit:https://shadowsocks.be/9.html
Enjoy it!
```

##  一键加速VPS服务器

此加速教程为谷歌BBR加速 ，vultr的服务器可以装谷歌bbr。

按照第二步的步骤，重新连接服务器ip，登录成功后，在命令栏里粘贴以下代码：

【谷歌BBR加速教程】

```
yum -y install wget

wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh

```

```
卸载 SSR：./shadowsocksR.sh uninstall

SSR 一些常用的命令


启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
日志路径

配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocksr.log
代码安装目录：/usr/local/shadowsocks
如果后面想修改 SSR 的一些配置可以直接修改 shadowsocks.json ，然后重启 SSR 即可。

```



## 开启加速：net_speeder

```
wget --no-check-certificate https://gist.github.com/LazyZhu/dc3f2f84c336a08fd6a5/raw/d8aa4bcf955409e28a262ccf52921a65fe49da99/net_speeder_lazyinstall.sh

sh net_speeder_lazyinstall.sh

或者：

wget https://github.com/snooda/net-speeder/archive/master.zip
unzip master.zip
cd net-speeder-master
chmod +x build.sh
./build.sh

把文件移动位置
mkdir /usr/local/net_speeder/
cp net_speeder /usr/local/net_speeder/net_speeder


开启加速
nohup /usr/local/net_speeder/net_speeder eth0 "ip" >/dev/null 2>&1 &



基本命令

查看net-speeder是否运行：ps aux|grep net_speeder|grep -v grep
停止net-speeder：killall net_speeder


```