---
title: nodejs express笔记
date: 2017-05-21 14:56:39
tags: nodejs
---



## Express 应用生成器

通过应用生成器工具 express 可以快速创建一个应用的骨架。

通过如下命令安装：

```
npm install express-generator -g
```

-h 选项可以列出所有可用的命令行选项：

```
$ express -h

  Usage: express [options] [dir]

  Options:

    -h, --help           output usage information
        --version        output the version number
    -e, --ejs            add ejs engine support
        --pug            add pug engine support
        --hbs            add handlebars engine support
    -H, --hogan          add hogan.js engine support
    -v, --view <engine>  add view <engine> support (dust|ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
    -c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
        --git            add .gitignore
    -f, --force          force on non-empty directory

```

下面的示例就是在当前工作目录下创建一个命名为 myapp 的应用。

```
$ express myapp

  warning: the default view engine will not be jade in future releases
  warning: use `--view=jade' or `--help' for additional options


   create : myapp
   create : myapp/package.json
   create : myapp/app.js
   create : myapp/public
   create : myapp/routes
   create : myapp/routes/index.js
   create : myapp/routes/users.js
   create : myapp/views
   create : myapp/views/index.jade
   create : myapp/views/layout.jade
   create : myapp/views/error.jade
   create : myapp/bin
   create : myapp/bin/www
   create : myapp/public/javascripts
   create : myapp/public/images
   create : myapp/public/stylesheets
   create : myapp/public/stylesheets/style.css

   install dependencies:
     $ cd myapp && npm install

   run the app:
     $ DEBUG=myapp:* npm start

```

安装依赖包与运行项目

```
install dependencies:
$ cd myapp && npm install

run the app:
$ DEBUG=myapp:* npm start
```

> 注意 `mac` 和 `win` 启动命令不同，上面是 `mac`的，`win` 请执行 ：`set DEBUG=myapp & npm start`
> 

然后在浏览器中打开 http://localhost:3000/ 网址就可以看到这个应用了

通过 Express 应用生成器创建的应用一般都有如下目录结构：

```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.jade
    ├── index.jade
    └── layout.jade
```


## Express 路由

路由是指如何定义应用的端点（URIs）以及如何响应客户端的请求。

路由是由一个 URI、HTTP 请求（GET、POST等）和若干个句柄组成，它的结构如下： app.METHOD(path, [callback...], callback)， app 是 express 对象的一个实例， METHOD 是一个 HTTP 请求方法， path 是服务器上的路径， callback 是当路由匹配时要执行的函数。

下面是一个基本的路由示例：

```
var express = require('express');
var app = express();

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function(req, res) {
  res.send('hello world');
});
```

### 路由方法

路由方法源于 `HTTP` 请求方法，和 `express` 实例相关联。

下面这个例子展示了为应用跟路径定义的 `GET` 和 `POST` 请求：

```

// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage');
});

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage');
});
```


Express 定义了如下和 HTTP 请求对应的路由方法： get, post, put, head, delete, options, trace, copy, lock, mkcol, move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge, m-search, notify, subscribe, unsubscribe, patch, search, 和 connect。

>有些路由方法名不是合规的 JavaScript 变量名，此时使用括号记法，比如： app['m-search']('/', function ...
>
>


`app.all()` 是一个特殊的路由方法，没有任何 `HTTP` 方法与其对应，它的作用是对于一个路径上的所有请求加载中间件。

在下面的例子中，来自 “`/secret`” 的请求，不管使用 `GET`、`POST`、`PUT`、`DELETE` 或其他任何 `http` 模块支持的 `HTTP` 请求，句柄都会得到执行。

```
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...');
  next(); // pass control to the next handler
});
```

### 路由路径

路由路径和请求方法一起定义了请求的端点，它可以是字符串、字符串模式或者正则表达式。

Express 使用 `path-to-regexp` 匹配路由路径，请参考文档查阅所有定义路由路径的方法。 Express `Route Tester` 是测试基本 Express 路径的好工具，但不支持模式匹配。

>查询字符串不是路由路径的一部分。

使用字符串的路由路径示例：

```
// 匹配根路径的请求
app.get('/', function (req, res) {
  res.send('root');
});

// 匹配 /about 路径的请求
app.get('/about', function (req, res) {
  res.send('about');
});

// 匹配 /random.text 路径的请求
app.get('/random.text', function (req, res) {
  res.send('random.text');
});
```


使用字符串模式的路由路径示例：

```
// 匹配 acd 和 abcd
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

// 匹配 abcd、abbcd、abbbcd等
app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});

// 匹配 abcd、abxcd、abRABDOMcd、ab123cd等
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// 匹配 /abe 和 /abcde
app.get('/ab(cd)?e', function(req, res) {
 res.send('ab(cd)?e');
});
```

>字符 ?、+、* 和 () 是正则表达式的子集，- 和 . 在基于字符串的路径中按照字面值解释。


使用正则表达式的路由路径示例：

```
// 匹配任何路径中含有 a 的路径：
app.get(/a/, function(req, res) {
  res.send('/a/');
});

// 匹配 butterfly、dragonfly，不匹配 butterflyman、dragonfly man等
app.get(/.*fly$/, function(req, res) {
  res.send('/.*fly$/');
});
```

### 路由句柄

可以为请求处理提供多个回调函数，其行为类似 `中间件`。唯一的区别是这些回调函数有可能调用 `next('route')` 方法而略过其他路由回调函数。可以利用该机制为路由定义前提条件，如果在现有路径上继续执行没有意义，则可将控制权交给剩下的路径。

路由句柄有多种形式，可以是一个函数、一个函数数组，或者是两者混合，如下所示.

使用一个回调函数处理路由：

```
app.get('/example/a', function (req, res) {
  res.send('Hello from A!');
});
```

使用多个回调函数处理路由（记得指定 `next` 对象）：

```
app.get('/example/b', function (req, res, next) {
  console.log('response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});
```


使用回调函数数组处理路由：

```
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

var cb2 = function (req, res) {
  res.send('Hello from C!');
}

app.get('/example/c', [cb0, cb1, cb2]);
```

混合使用函数和函数数组处理路由：

```
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from D!');
});
```


### 响应方法
下表中响应对象（res）的方法向客户端返回响应，终结请求响应的循环。如果在路由句柄中一个方法也不调用，来自客户端的请求会一直挂起。

方法 | 描述 
----|------
res.download()	|提示下载文件。
res.end()	|终结响应处理流程。
res.json()	|发送一个 JSON 格式的响应。
res.jsonp()	|发送一个支持 JSONP 的 JSON 格式的响应。
res.redirect()	|重定向请求。
res.render()	|渲染视图模板。
res.send()	|发送各种类型的响应。
res.sendFile()	|以八位字节流的形式发送文件。 sendfile('login.html');
res.sendStatus()	|设置响应状态代码，并将其以字符串形式作为响应体的一部分发送。


### app.route()

可使用 `app.route()` 创建路由路径的链式路由句柄。由于路径在一个地方指定，这样做有助于创建模块化的路由，而且减少了代码冗余和拼写错误。请参考 `Router()` 文档 了解更多有关路由的信息。

下面这个示例程序使用 `app.route()` 定义了链式路由句柄。


```
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```
  
  
### express.Router

可使用 `express.Router` 类创建模块化、可挂载的路由句柄。`Router` 实例是一个完整的中间件和路由系统，因此常称其为一个 “`mini-app`”。

下面的实例程序创建了一个路由模块，并加载了一个中间件，定义了一些路由，并且将它们挂载至应用的路径上。

在 `app` 目录下创建名为 `birds.js` 的文件，内容如下：


```
var express = require('express');
var router = express.Router();

// 该路由使用的中间件
router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// 定义网站主页的路由
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// 定义 about 页面的路由
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;
```


然后在应用中加载路由模块：

```
var birds = require('./birds');
...
app.use('/birds', birds);
```


应用即可处理发自 `/birds` 和 `/birds/about` 的请求，并且调用为该路由指定的 `timeLog` 中间件。



## 利用 Express 托管静态文件

通过 `Express` 内置的 `express.static` 可以方便地托管静态文件，例如`图片`、`CSS`、`JavaScript` 文件等。

将静态资源文件所在的目录作为参数传递给 `express.static` 中间件就可以提供静态资源文件的访问了。例如，假设在 `public` 目录放置了`图片`、`CSS` 和 `JavaScript` 文件，你就可以：

```
app.use(express.static('public'));
```

现在，`public` 目录下面的文件就可以访问了。

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```


>所有文件的路径都是相对于存放目录的，因此，存放静态文件的目录名不会出现在 `URL` 中。


如果你的静态资源存放在多个目录下面，你可以多次调用 `express.static` 中间件：

```
app.use(express.static('public'));
app.use(express.static('files'));
```

访问静态资源文件时，`express.static` 中间件会根据目录添加的顺序查找所需的文件。

如果你希望所有通过 `express.static` 访问的文件都存放在一个“`虚拟（virtual）`”目录（即目录根本不存在）下面，可以通过为静态资源目录指定一个挂载路径的方式来实现，如下所示：

```
app.use('/static', express.static('public'));
```

现在，你就爱可以通过带有 “`/static`” 前缀的地址来访问 `public` 目录下面的文件了。

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```



### 中间件与 next

`express` 中的中间件（middleware）就是用来处理请求的，当一个中间件处理完，可以通过调用 `next()` 传递给下一个中间件，如果没有调用 `next()`，则请求不会往下传递，如内置的 `res.render` 其实就是渲染完 `html` 直接返回给客户端，没有调用 `next()`，从而没有传递给下一个中间件。看个小例子，修改 `index.js` 如下：

**index.js**

```
var express = require('express');
var app = express();

app.use(function(req, res, next) {
  console.log('1');
  next();
});

app.use(function(req, res, next) {
  console.log('2');
  res.status(200).end();
});

app.listen(3000);
```

此时访问 localhost:3000，终端会输出：

```
1
2
```

通过 `app.use` 加载中间件，在中间件中通过 `next` 将请求传递到下一个中间件，`next` 可接受一个参数接收错误信息，如果使用了 `next(error)`，则会返回错误而不会传递到下一个中间件，修改 `index.js` 如下：

**index.js**

```
var express = require('express');
var app = express();

app.use(function(req, res, next) {
  console.log('1');
  next(new Error('haha'));
});

app.use(function(req, res, next) {
  console.log('2');
  res.status(200).end();
});

app.listen(3000);
```


报错信息：
```
Error: haha
    at /Users/shanshanxue/Documents/workspace/aym_admin_nav/index.js:6:8
    at Layer.handle [as handle_request] (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/layer.js:95:5)
    at trim_prefix (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:317:13)
    at /Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:284:7
    at next (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:275:10)
    at expressInit (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/middleware/init.js:40:5)
    at Layer.handle [as handle_request] (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/layer.js:95:5)
    at trim_prefix (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:317:13)
    at /Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:284:7
    at next (/Users/shanshanxue/Documents/workspace/aym_admin_nav/node_modules/express/lib/router/index.js:275:10)
```




> 小提示：`app.use` 有非常灵活的使用方式，详情见 [官方文档](http://expressjs.com/en/4x/api.html#app.use)。
> 

`express` 有成百上千的第三方中间件，在开发过程中我们首先应该去 `npm` 上寻找是否有类似实现的中间件，尽量避免造轮子，节省开发时间。下面给出几个常用的搜索 npm 模块的网站：


1. http://npmjs.com(npm 官网)
2. http://node-modules.com
3. https://npms.io
4. https://nodejsmodules.org

>小提示：`express@4` 之前的版本基于 `connect` 这个模块实现的中间件的架构，`express@4` 及以上的版本则移除了对 `connect` 的依赖自己实现了，理论上基于 `connect` 的中间件（通常以 `connect-` 开头，如 `connect-mongo`）仍可结合 `express` 使用。
>
>
>注意：中间件的加载顺序很重要！比如：通常把日志中间件放到比较靠前的位置，后面将会介绍的 `connect-flash` 中间件是基于 `session` 的，所以需要在 `express-session` 后加载。
>

### 错误处理

上面的例子中，应用程序为我们自动返回了错误栈信息（`express` 内置了一个默认的错误处理器），假如我们想手动控制返回的错误内容，则需要加载一个自定义错误处理的中间件，修改 `index.js` 如下：

**index.js**

```
var express = require('express');
var app = express();

app.use(function(req, res, next) {
  console.log('1');
  next(new Error('haha'));
});

app.use(function(req, res, next) {
  console.log('2');
  res.status(200).end();
});

//错误处理
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

app.listen(3000);
```

此时访问 `localhost:3000`，浏览器会显示 `Something broke!`。



