---
title: CENTOS7 安装 NET-SPEEDER 提升 VPS 网络性能
date: 2017-12-09 15:22:00
tags: vps
---


## 1、安装依赖库 

先安装epel源 

```bash
rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm

（这里 epel-release-7-8.noarch.rpm 版本可能更新，可以去到 http://dl.fedoraproject.org/pub/epel/7/x86_64/e 搜索查看当前有的版本）

```

## 2、然后即可使用yum安装： 

```bash
yum install libnet libpcap libnet-devel libpcap-devel gcc
```


## 3、然后获取net-speeder

```bash
wget https://github.com/snooda/net-speeder/archive/master.zip
unzip master.zip
cd net-speeder-master （这里注意查看下目录名称，可能会变）
chmod +x build.sh
./build.sh

```


> 编译完成，当前目录下会多出 net_speeder 文件




## 4、运行 net-speeder

```bash
参数：./net_speeder 网卡名 加速规则（bpf规则） 
最简单用法：./net_speeder venet0 "ip"  加速所有ip协议数据

./net_speeder eth0 "ip" &  网卡一般为 eth0，可以使用 ifconfig 查看。（然而 CentOS7 没有内置 ifconfig 命令，使用 yum install net-tools.x86_64 安装即可）
```


## 5、然后复制到/usr/local/目录并设置开机自启动：

```bash
mkdir /usr/local/net_speeder/    创建 net_speeder 目录

cp net_speeder

/usr/local/net_speeder/net_speeder    复制 net_speeder
echo 'nohup /usr/local/net_speeder/net_speeder eth0 "ip" >/dev/null 2>&1 &' >> /etc/rc.local    创建开机启动
```

### 6、其它

可以使用 ps -e 来查看进程中是否有 net_speeder 来确认是否运行。

控制台偶尔会自动跳出 libnet_write_raw_ipv4(): -1 bytes written (Message too long) 错误提示，目前得到的信息是正常情况。

基本命令

```
查看net-speeder是否运行：ps aux|grep net_speeder|grep -v grep
停止net-speeder：killall net_speeder
```
