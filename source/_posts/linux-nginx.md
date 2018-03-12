---
title: nginx 搭建静态网站
date: 2017-09-18 15:13:49
tags:
---



## 搭建Http静态服务器环境


搭建静态网站，首先需要部署环境。下面的步骤，将告诉大家如何在服务器上通过 Nginx 部署 HTTP 静态服务。

### 安装 Nginx

在 CentOS 上，可直接使用 `yum` 来安装 Nginx

```
yum install nginx -y
```

安装完成后，使用 `nginx` 命令启动 Nginx：

```
nginx
```

此时，访问 测试地址 可以看到 Nginx 的测试页面


> <bubble for="help">
> 如果无法访问，请重试用 `nginx -s reload` 命令重启 Nginx
> </bubble>

> <checker type="output-contains" command="ls /usr/sbin/" hint="Nginx 未安装">
>     <keyword regex="nginx" />
> </checker>

> <checker type="output-contains" command="netstat -nltp" hint="Nginx 未启动">
>     <keyword regex="nginx" />
>     <keyword regex="80" />
> </checker>

### 配置静态服务器访问路径

外网用户访问服务器的 Web 服务由 Nginx 提供，Nginx 需要配置静态资源的路径信息才能通过 url 正确访问到服务器上的静态资源。

打开 Nginx 的默认配置文件 [/etc/nginx/nginx.conf] ，修改 Nginx 配置，将默认的 `root /usr/share/nginx/html;` 修改为: `root /data/www;`，如下：

```nginx
/// <example verb="edit" file="/etc/nginx/nginx.conf" />
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /data/www;

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

}
```

配置文件将 [/data/www/static][data-www-static-path] 作为所有静态资源请求的根路径，如访问: `http://xxx.xxx.xxx.xxx/static/index.js`，将会去 [/data/www/static/][data-www-static-path] 目录下去查找 `index.js`。现在我们需要重启 Nginx 让新的配置生效，如：

> <locate for="data-www-static-path" path="/data/www/static" hint="这里是所有静态资源请求的根路径"/>

```
nginx -s reload
```

重启后，现在我们应该已经可以使用我们的静态服务器了，现在让我们新建一个静态文件，查看服务是否运行正常。

首先让我们在 [/data][data-path] 目录 下创建 `www` 目录，如：

> <locate for="data" path="/data/www" hint="该目录作为服务器的根目录使用"/>

```
mkdir -p /data/www
```

> <locate for="1" path="/etc/nginx/nginx.conf" hint="编辑默认服务器配置，修改网站的根路径" />

> <checker type="output-contains" command="ls /data/ -la" hint="在 `/data`目录 下创建 `/www`目录">
>     <keyword regex="www" />
> </checker>

### 创建第一个静态文件

在 [/data/www] 目录下创建我们的第一个静态文件 [index.html]

```html
/// <example verb="edit" file="/data/www/index.html" />
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
	<title>第一个静态文件</title>
</head>
<body>
Hello world！
</body>
</html>
```

现在访问 [http://xxx.xxx.xxx.xxx/index.html] 应该可以看到页面输出 [Hello world!][indicate-hello-world] 

到此，一个基于 Nginx 的静态服务器就搭建完成了，现在所有放在 [/data/www] 目录下的的静态资源都可以直接通过域名访问。

> <locate for="data-www-path" path="/data/www" hint="/data/www 是服务器的根目录"/>

> <locate for="1" path="/data/www" hint="添加 index.html 文件" />

> <bubble for="indicate-hello-world">
> 如果无显示，请刷新浏览器页面
> </bubble>

> <checker type="output-contains" command="curl -I --silent http://xxx.xxx.xxx.xxx" hint="配置 Nginx 静态服务根路径">
>     <keyword regex="HTTP/1.1 200 OK" />
> </checker>
