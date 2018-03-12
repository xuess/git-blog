---
title: 设置清除html5页面缓存
date: 2016-09-04 14:51:22
tags:
---

### `html5`端设置 `meta` 标签：

```
<meta http-equiv="Pragma" content="no-cache" />
<meta http-equiv="Cache-Control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
```

>  设置上面的只是紧紧可以保证`html`文件每次从服务器中获取，不从缓存文件中拿，而对于外链`CSS` `JS` `图片` 等文件仍旧是从缓存中获取的；
>


### 设置 `css` `JS`文件不从缓存中读取:

通过添加版本号的和随机数的方法，保证每次加载`JS` `CSS`连接都是最新的，通常的做法是添加一个版本号，在每次更新了`JS` `CSS`时给**版本号+1**，保证没有更新时采用缓存文件。

> 不推荐加随机数的方法，每次都是最新的，没办法利用缓存提高性能。

```
<script type="text/javascript" src="/js/index.js?v=1.0.0"></script>
```


> 转载请注明出处
>
> 作者：xuess<wuniu2010@126.com>
> 
> 时间：2017年08月08日
> 
> 最后修改时间：2017年08月08日