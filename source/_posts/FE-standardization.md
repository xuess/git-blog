---
title: 前端命名与写法规范
date: 2016-06-04 14:48:19
tags:
---


## 外部命名规范



### html 、js 、css文件名称命名规范

```
my_script.js
my_camel_case_name.css
my_index.html
```


### 路径规范 不写http、https

* js 

```
<script src="//cdn.com/test.min.js"></script>
```

* css

```
.example {
  background: url(//static.example.com/images/bg.jpg);
}
```

> html,js,css文档上部分 增加作者注释，开发时间，功能，最后一次修改时间（多次）



##html命名规范

### 文档格式

* css 文件放在 `head`标签中，js放到 `body`尾部 
* 使用 `utf-8` 文字编码
* html 标签一律使用`小写`
* 属性使用双引号


```
<!--
	作者：xuess
	时间：2017-06-22
	描述：测试页面
-->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<meta http-equiv="Access-Control-Allow-Origin" content="*">
		<!--rem先关首先加载-->
		<script src="https://g.alicdn.com/mtb/??lib-flexible/0.3.2/flexible_css.js,lib-flexible/0.3.2/flexible.js"></script>
		<!--css在head-->
		<link rel="stylesheet" type="text/css" href="xxx.css" />
	</head>

	<body>
		<div>
			<div></div>
		</div>
		<!--js在文档尾部-->
		<script src="xxx.js" type="text/javascript" charset="utf-8"></script>
	</body>

</html>
```

### 推荐使用语义化标签


```
<header>
	<h1>My page title</h1>
</header>

<nav class="top-navigation">
	<ul>
		<li class="nav-item">
			<a href="#home">Home</a>
		</li>
		<li class="nav-item">
			<a href="#news">News</a>
		</li>
		<li class="nav-item">
			<a href="#about">About</a>
		</li>
	</ul>
</nav>
<section class="page-section news">
	<header>
		<h2 class="title">All news articles</h2>
	</header>

	<article class="news-article">
		<header>
			<div class="article-title">Good article</div>
			<small class="intro">Introduction sub-title</small>
		</header>

		<div class="content">
			<p>This is a good example for HTML semantics</p>
		</div>
		<aside class="article-side-notes">
			<p>I think I'm more on the side and should not receive the main credits</p>
		</aside>
		<footer class="article-foot-notes">
			<p>This article was created by David <time datetime="2018-01-01 00:00" class="time">1 month ago</time></p>
		</footer>
	</article>

	<footer class="section-footer">
		<p>Related sections: Events, Public holidays</p>
	</footer>
</section>

<footer class="page-footer">
	Copyright 2017
</footer>
```


### 结构、表现、行为三者分离

```
1. 不要写行内样式
2. 不使用表象元素（`b`, `u`, `center`, `font`, `i`）
3. 不要在html 中写js代码
4. 元素自定义属性 使用`data-`开头
```


## js命名规范

### 变量声明

```
1. 推荐使用es6语法规范，局部作用域变量用let，常量用const
2. 常量使用全大写命名（const FAIL_STATE = 101）
3. 普通变量名称用小写字母开头，驼峰式（let userInfo = {}）
4. 对象命名以大写字母开头，大驼峰式（function User(){//...}）
5. 声明字符串建议使用单引号（let msg = 'This is some HTML <div class="makes-sense"></div>';）

```


#### 变量赋值时的逻辑操作

```
//不推荐
if(!x) {
  if(!y) {
    x = 1;
  } else {
    x = y;
  }
}

//推荐
let x = x || y || 1;

```




##css 命名规范

### css权重

```
第一等：代表内联样式，如: style=””，权值为1000

第二等：代表ID选择器，如：#content，权值为100

第三等：代表类，伪类和属性选择器，如：.content，权值为10

第四等：代表类型选择器和伪元素选择器，如：div p，权值为1

```

### 基本原则

```
1. 选择器应该避免使用ID，一般情况ID不应该用于样式
2. 选择器中避免直接写标签名，没有语义，而且很容被重叠
3. 选择器应该尽可能的精确，推荐使用 大于号`>`
4. 尽量使用简写，如：padding: 10px 35px;
5. 0px、0rem，不用写单位
6. 颜色尽量使用简写，如：用 #fff 代替 #ffffff;
7. 书写代码前, 考虑并提高样式重复使用率，可以定义一些常用的简写样式
8. 选择器要尽可能短，并且尽量限制组成选择器的个数，建议不要超过3层
9. 多写有效注释，小伙伴看了会比较明了
10. 为避免重叠，单独模块，可以使用命名空间
```


### 命名规范

```
1. class名称使用`-`中划线链接（短横线命名），不推荐使用大小写的驼峰式。
2. 需要绑定事件的class名称，应该单独写。不要与样式class公用。事件与样式区分开。推荐`J_xxx`开头
3. 语义明确的情况下，class名称尽量言简意赅
```

### 声明顺序

```
1. 结构性属性：
display
position, left, top, right etc.
overflow, float, clear etc.
margin, padding
2. 表现性属性：
background, border etc.
font, text
```

> 转载请注明出处
>
> 作者：xuess<wuniu2010@126.com>
> 
> 时间：2017年06月16日
> 
> 最后修改时间：2017年06月16日

