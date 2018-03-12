---
title: linux centos7 初始化安装软件运行环境
date: 2017-08-07 14:54:48
tags: linux
---


## centos 7 安装 nginx，配置https 以及注意事项


```bash
yum install -y nginx

```

却发现一直提示`No package nginx available`

尝试安装

```bash
yum install -y epel-release
```


```bash
yum repolist
```

```bash
# 首先进入/yum.repos.d目录
cd /etc/yum.repos.d

# 然后编辑epel.repo文件
vi epel.repo

[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch/debug
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
#baseurl=http://download.fedoraproject.org/pub/epel/7/SRPMS
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1

```

可以看到，`[epel]`和`[epel-source]`里面的enabled都是`0`，解决办法就在这里，只要把`0`改成`1`，保存退出后即可。


现在我们重新运行`yum repolist`，会发现`epel`源已经被加上了：

```
源标识                          源名称                                                                      状态
base/7/x86_64                   CentOS-7 - Base                                                             9,590+1
elrepo-kernel                   ELRepo.org Community Enterprise Linux Kernel Repository - el7                    37
epel/x86_64                     Extra Packages for Enterprise Linux 7 - x86_64                               12,277
epel-source/x86_64              Extra Packages for Enterprise Linux 7 - x86_64 - Source                           0
extras/7/x86_64                 CentOS-7 - Extras                                                               388
nodesource/x86_64               Node.js Packages for Enterprise Linux 7 - x86_64                                 22
updates/7/x86_64                CentOS-7 - Updates                                                          1,922+7
repolist: 24,236

```


再执行`yum install -y nginx`，发现终于能够成功安装了！

启动nginx

```
service nginx statrt #启动
service nginx stop
service nginx restart
service nginx status

```


## 使用HTTPS

首先通过yum下载安装certbot：

```
yum install certbot
```

进入`/etc/nginx`，然后编辑`nginx.conf`，在`server`里面添加下列两个`location`规则：

```
location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root     /usr/share/nginx/html;
}

location = /.well-known/acme-challenge/ {
   return 404;
}

```


可以看到，上面的root我是指向了`/usr/share/nginx/html`，这个目录是可以随便指定的，我这么写完全是为了偷懒。

nginx配置好了以后，就可以使用`certbot`生成证书了：

```
# certbot certonly --webroot -w <root url> -d <hostname>

certbot certonly --webroot -w /usr/share/nginx/html/ -d xueshanshan.com

```


如果看到下列的输出，就证明证书已经生成成功了：

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/xueshanshan.com/fullchain.pem. Your cert
   will expire on 20XX-09-23. To obtain a new or tweaked version of
   this certificate in the future, simply run certbot again. To
   non-interactively renew *all* of your certificates, run "certbot
   renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
   
   
```


证书已经准备好了，我们还需要`nginx`的支持。重新打开`/etc/nginx/nginx.conf`，然后把注释掉的`https server`给[注释]回来：

```

server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  xueshanshan.com;
    root         /home/www;

    ssl_certificate "/etc/letsencrypt/live/xueshanshan.com/fullchain.pem";
    ssl_certificate_key "/etc/letsencrypt/live/xueshanshan.com/privkey.pem";
    ssl_trusted_certificate /etc/letsencrypt/live/xueshanshan.com/chain.pem;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}

```

ps: 上面的root我给配置了`/home/www`目录，意味着以后只要是放在该目录下的静态资源文件夹，我都可以通过`https://xxx.com/文件夹名`直接进行访问，更多关于`nginx`的配置请参考官方文档。


最后重启一下`nginx`，就可以检验我们的页面是否已经打上小绿标了

```
service nginx restart
```


由于`certbot`所使用的`letsencrypt`证书只有`90天`的有效期，所以我们需要对它定期`自动更新`


首先模拟更新：

```
sudo certbot renew --dry-run

# 看到如下输出证明模拟更新成功
-------------------------------------------------------------------------------
Processing /etc/letsencrypt/renewal/your.domain.com.conf
-------------------------------------------------------------------------------
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/xueshanshan.com/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)

```

然后就可以使用`crontab -e`命令来实现自动化了：

```
sudo crontab -e

#添加配置，每周一半夜3点00分执行renew：

00 3 * * 1 /usr/bin/certbot renew  >> /var/log/le-renew.log

```
