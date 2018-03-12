---
title: linux安装node-firekylin博客系统
date: 2017-07-25 14:45:09
tags: linux
---


在服务器上下载安装包

```bash
wget https://firekylin.org/release/latest.tar.gz

```
解压安装包

```bash
tar zvxf latest.tar.gz
```

安装程序依赖

```bash
cd firekylin
npm install
```

复制项目下的 `pm2_default.json` 文件生成新文件 `pm2.json`

```bash
cp pm2_default.json pm2.json
```

修改 pm2.json 文件中的 `cwd` 配置值为项目的当前路径 `/root/firekylin`：

```bash
{
  "apps": [{
    "name": "firekylin",
    "script": "www/production.js",
    "cwd": "/root/firekylin",
    "exec_mode": "fork",
    "max_memory_restart": "1G",
    "autorestart": true,
    "node_args": [],
    "args": [],
    "env": {

    }
  }]
}
```

然后通过以下命令启动项目

```bash
pm2 startOrReload pm2.json
```


访问: http://【ip地址】:8360/
