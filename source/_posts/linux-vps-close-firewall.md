---
title: linux vps关闭防火墙
date: 2017-11-28 15:24:27
tags: vps
---

## vps web 如果想让本地访问，需要关闭防火墙

关闭防火墙：

```
sudo systemctl stop firewalld.service && sudo systemctl disable firewalld.service
```

centos从7开始默认用的是firewalld，这个是基于iptables的，虽然有iptables的核心，但是iptables的服务是没安装的。所以你只要停止firewalld服务即可：
sudo systemctl stop firewalld.service && sudo systemctl disable firewalld.service

如果你要改用iptables的话，需要安装iptables服务：
sudo yum install iptables-services
sudo systemctl enable iptables && sudo systemctl enable ip6tables
sudo systemctl start iptables && sudo systemctl start ip6tables

或者 

```
service iptables stop
```