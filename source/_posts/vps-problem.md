---
title: vps问题整理
date: 2017-12-19 14:52:08
tags:
---

## 为什么VPS能ping通，但是通过域名打不开？

首先，确定您的VPS上的iptables是否屏蔽了80端口。如果iptables屏蔽了80端口（HTTP服务默认端口）的话，是无法访问您在VPS上创建的网站的。在Centos上，请使用如下命令关闭iptables试试：

```
service iptables stop

```

还有另一种可能是你使用的PC的DNS被污染了，请将您的PC上的DNS服务器更换为OpenDNS或者使用Google DNS(8.8.8.8,8.8.4.4)。为了能更好的使您的VPS被国内用户访问，最好把在Godaddy或者其他国外域名提供商那里购买域名的DNS的nameserve更换为dnspod，这样你的域名更容易被国内用户访问了，因为这些域名提供商的nameserver经常在国内无法正常访问。

对内存在256MB以下的VPS,也极有可能发生可以ping通，但是网站无法访问的情况。大多数是因为httpd负载太高而内存不足导致Apache挂掉，请升级VPS的内存或优化系统负载情况。