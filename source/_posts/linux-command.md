---
title: linux 重用命令整理
date: 2017-04-24 15:11:44
tags: linux
---


## 查看版本

### cat /etc/redhat-release 

```bash
CentOS Linux release 7.4.1708 (Core)
```

### uname -a

```bash
Linux vps_jp_2018_2 4.14.13-1.el7.elrepo.x86_64 #1 SMP Wed Jan 10 15:12:12 EST 2018 x86_64 x86_64 x86_64 GNU/Linux
```



## 重命名例子：

1) 将一个名为abc.txt的文件重命名为1234.txt

```bash
mv abc.txt 1234.txt
```
2) 将目录A重命名为B

```bash
mv A B
```

3) 将a.txt移动到/b下，并重命名为c.txt

```bash
mv a.txt /b/c.txt
```


## 解压
将压缩文件test.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令覆盖原先的文件。

```
unzip -o test.zip -d tmp/
```