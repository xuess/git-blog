---
title: nodejs+express开发笔记
date: 2016-11-08 15:15:55
tags: nodejs
---

# nodejs 开发笔记

## supervisor

在开发过程中，每次修改代码保存后，我们都需要手动重启程序，才能查看改动的效果。使用 `supervisor` 可以解决这个繁琐的问题，全局安装 `supervisor`：

```
npm install -g supervisor
```

运行 `supervisor --harmony index` 启动程序，如下所示：

```
$ supervisor --harmony index

Running node-supervisor with
  program '--harmony index'
  --watch '.'
  --extensions 'node,js'
  --exec 'node'

Starting child process with 'node --harmony index'
Watching directory '/Users/shanshanxue/Documents/workspace/aym_admin_nav' for changes.
Press rs for restarting the process.

```
supervisor 会监听当前目录下 node 和 js 后缀的文件，当这些文件发生改动时，supervisor 会自动重启程序。


## 路由

```
var express = require('express');
var app = express();

app.get('/', function(req, res) {
  res.send('hello, express');
});

app.get('/users/:name', function(req, res) {
  res.send('hello, ' + req.params.name);
});

app.listen(3000);
```

以上代码的意思是：当访问根路径时，依然返回 `hello`, `express`，当访问如 `localhost:3000/users/nswbmw` 路径时，返回 `hello`, `nswbmw`。路径中 `:name` 起了占位符的作用，这个占位符的名字是 `name`，可以通过 `req.params.name` 取到实际的值。

>小提示：express 使用了 `path-to-regexp` 模块实现的路由匹配。

不难看出：req 包含了请求来的相关信息，res 则用来返回该请求的响应，更多请查阅 express 官方文档。下面介绍几个常用的 req 的属性：


* `req.query`: 解析后的 `url` 中的 `querystring`，如 `?name=haha`，`req.query` 的值为 `{name: 'haha'}`
* `req.params`: 解析 `url` 中的占位符，如 `/:name`，访问 `/haha`，`req.params` 的值为 `{name: 'haha'}`
* `req.body`: 解析后请求体，需使用相关的模块，如 `body-parser`，请求体为 `{"name": "haha"}`，则 `req.body` 为 `{name: 'haha'}`


## 模板引擎ejs

模板引擎有很多，`ejs` 是其中一种，因为它使用起来十分简单，而且与 `express` 集成良好，所以我们使用 `ejs`。安装 `ejs`：

```
npm i ejs --save
```

**ejs文件**

```
<!DOCTYPE html>
<html>
  <head>
    <style type="text/css">
      body {padding: 50px;font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;}
    </style>
  </head>
  <body>
    <h1><%= name.toUpperCase() %></h1>
    <p>hello, <%= name %></p>
  </body>
</html>
```

**routes**

```
var express = require('express');
var router = express.Router();

router.get('/:name', function(req, res) {
  res.render('users', {
    name: req.params.name
  });
});

module.exports = router;
```

通过调用 `res.render` 函数渲染 `ejs` 模板，`res.render` 第一个参数是模板的名字，这里是 `users` 则会匹配 `views/users.ejs`，第二个参数是传给模板的数据，这里传入 `name`，则在 `ejs` 模板中可使用 `name`。`res.render` 的作用就是将模板和数据结合生成 `html`，同时设置响应头中的 `Content-Type: text/html`，告诉浏览器我返回的是 `html`，不是纯文本，要按 `html` 展示



上面代码可以看到，我们在模板 `<%= name.toUpperCase() %>` 中使用了 `JavaScript` 的语法 `.toUpperCase()` 将名字转化为大写，那这个 `<%= xxx %>` 是什么东西呢？`ejs` 有 `3` 种常用标签：

1. `<% code %>`：运行 `JavaScript` 代码，不输出
2. `<%= code %>`：显示转义后的 `HTML` 内容
3. `<%- code %>`：显示原始 `HTML` 内容

>注意：`<%= code %>` 和 `<%- code %>` 都可以是 `JavaScript` 表达式生成的字符串，当变量 `code` 为普通字符串时，两者没有区别。当 `code` 比如为 `<h1>hello</h1>` 这种字符串时，`<%= code %>` 会原样输出 `<h1>hello</h1>`，而 `<%- code %>` 则会显示 `H1` 大的 `hello` 字符串。

下面的例子解释了 `<% code %>` 的用法：

Data

```
supplies: ['mop', 'broom', 'duster']
```

Template

```
<ul>
<% for(var i=0; i<supplies.length; i++) {%>
   <li><%= supplies[i] %></li>
<% } %>
</ul>
```

Result

```
<ul>
  <li>mop</li>
  <li>broom</li>
  <li>duster</li>
</ul>
```

#### includes

我们使用模板引擎通常不是一个页面对应一个模板，这样就失去了模板的优势，而是把模板拆成可复用的模板片段组合使用，如在 views 下新建 header.ejs 和 footer.ejs，并修改 users.ejs：


**views/header.ejs**

```
<!DOCTYPE html>
<html>
  <head>
    <style type="text/css">
      body {padding: 50px;font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;}
    </style>
  </head>
  <body>
  ```
  
  
**views/footer.ejs**

```
  </body>
</html>
```

**views/users.ejs**

```
<%- include('header') %>
  <h1><%= name.toUpperCase() %></h1>
  <p>hello, <%= name %></p>
<%- include('footer') %>
```

我们将原来的 `users.ejs` 拆成出了 `header.ejs` 和 `footer.ejs`，并在 `users.ejs` 通过 `ejs` 内置的 `include` 方法引入，从而实现了跟以前一个模板文件相同的功能。

>小提示：拆分模板组件通常有两个好处：
>
>1. 模板可复用，减少重复代码
>
>2. 主模板结构清晰



>注意：要用 `<%- include('header') %>` 而不是 `<%= include('header') %>`


## 开发准备

### 目录结构

对应文件及文件夹的用处：

1. `models`: 存放操作数据库的文件
2. `public`: 存放静态文件，如样式、图片等
3. `routes`: 存放路由文件
4. `views`: 存放模板文件
5. `index.js`: 程序主文件
6. `package.json`: 存储项目名、描述、作者、依赖等等信息

>小提示：不知读者发现了没有，我们遵循了 `MVC`（模型(model)－视图(view)－控制器(controller/route)） 的开发模式。


### 安装依赖模块

运行以下命令安装所需的模块：

```
npm i config-lite connect-flash connect-mongo ejs express express-formidable express-session marked moment mongolass objectid-to-timestamp sha1 winston express-winston --save
```

对应模块的用处：


1. `express`: web 框架
2. `express-session`: session 中间件
3. `connect-mongo`: 将 session 存储于 mongodb，结合 express-session 使用
4. `connect-flash`: 页面通知提示的中间件，基于 session 实现
5. `ejs`: 模板
6. `express-formidable`: 接收表单及文件的上传中间件
7. `config-lite`: 读取配置文件
8. `marked`: markdown 解析
9. `moment`: 时间格式化
10. `mongolass`: mongodb 驱动
11. `objectid-to-timestamp`: 根据 ObjectId 生成时间戳
12. `sha1`: sha1 加密，用于密码加密
13. `winston`: 日志
14. `express-winston`: 基于 winston 的用于 express 的日志中间件



## config-lite

不管是小项目还是大项目，将配置与代码分离是一个非常好的做法。我们通常将配置写到一个配置文件里，如 `config.js` 或 `config.json` ，并放到项目的根目录下。但通常我们都会有许多环境，如本地开发环境、测试环境和线上环境等，不同的环境的配置不同，我们不可能每次部署时都要去修改引用 `config.test.js` 或者 `config.production.js`。`config-lite` 模块正是你需要的。

`config-lite` 是一个轻量的读取配置文件的模块。`config-lite` 会根据环境变量（`NODE_ENV`）的不同从当前执行进程目录下的 `config` 目录加载不同的配置文件。如果不设置 `NODE_ENV`，则读取默认的 `default` 配置文件，如果设置了 `NODE_ENV`，则会合并指定的配置文件和 `default` 配置文件作为配置，`config-lite` 支持 `.js`、`.json`、`.node`、`.yml`、`.yaml` 后缀的文件。

如果程序以 `NODE_ENV=test node app` 启动，则 `config-lite` 会依次降级查找 `config/test.js`、`config/test.json`、`config/test.node`、`config/test.yml`、`config/test.yaml` 并合并 `default` 配置; 如果程序以 `NODE_ENV=production node app` 启动，则 `config-lite` 会依次降级查找 `config/production.js`、`config/production.json`、`config/production.node`、`config/production.yml`、`config/production.yaml` 并合并 `default` 配置。

在 `myblog` 下新建 `config` 目录，在该目录下新建 `default.js`，添加如下代码：

**config/default.js**

```
module.exports = {
  port: 3000,
  session: {
    secret: 'myblog',
    key: 'myblog',
    maxAge: 2592000000
  },
  mongodb: 'mongodb://localhost:27017/myblog'
};
```

配置释义：

1. `port`: 程序启动要监听的端口号
2. `session`: `express-session` 的配置信息，后面介绍
3. `mongodb`: `mongodb` 的地址，`myblog` 为 `db` 名





## 功能与路由设计

功能列表：
路由设计如下：


```
注册 
	* 注册页 ：GET /signup
	* 注册方法： POST /signup

登陆：
	* 登陆页：GET /signin
	* 登录：POST /signin

登出：GET /signout

	
类目:
查看类目 ： GET /category
发表类目 ：
   * 发表类目页面：GET /category/create
   * 发表类目： POST /category
修改类目： 
	修改类目页面： GET /category/:categoryId/edit
	修改类目：POST /category/:categoryId/edit
删除类目：GET /category/:categoryId/remove


NAV :
查看nav
	个人主页： GET /nav?user=xxx
发表nav ：
   * 发表nav页面：GET /nav/create
   * 发表nav： POST /nav
修改nav： 
	修改nav页面： GET /nav/:navId/edit
	修改nav：POST /nav/:navId/edit
删除nav：GET /nav/:navId/remove


用户与nav分类关联信息：
user To Nav Category :
添加分类页面： GET /userToCategory
更新nav分类管理信息 ：POST /userToCategory/updata?userId=xxx&categorys=1,2,3,4,4,5,6



```

用户表：

名称 | name
---|------
id|id
用户名|name
密码|password


nav表：

名称 | name
---|------
id | id
名称 | title
内容 | content
图片url | imgUrl
类别 | categoryId
作者ID | userId
pv | pv


nav分类：

名称 | name
---|------
id |id
名称 | name


用户个人分类表（根据此表展示当前用户选择的分类下的所有nav）：

名称 | name
---|------
id | id
用户id| userId
分类ID  （1，2，3，4，5）| categoryId



接口结果格式：


```
{
success : true，
msg : '',
code : '200',
obj : {}
}
```





