---
title: api多版本方案
date: 2016-03-12 11:52:09
tags: api
---

1.利用url

```
https://www.taofen8.com/api/v2/getXXX
```


2.利用自定义请求头 api-version

```
https://www.taofen8.com/api/getXXX
api-version: 2
```


3.请求头内容协商

```
https://www.taofen8.com/api/getXXX
Accept: application/vnd.taofen8+json; version=2.0 
```

4.通过参数形式

```
https://www.taofen8.com/api/getXXX?v=2
```

或者

```
var param = {
"version":"2.0",
"method":"getXXX",
}

https://www.taofen8.com/api/{param}
```