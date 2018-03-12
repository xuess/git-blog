---
title: vue入门教程
date: 2016-04-02 15:25:45
tags: vue
---


## 写在前面

看完此教程可以达到：能看懂并能修改简单的`vue项目`。

看的过程中，请把所有例子都放到`html`文件中跑一遍。


## Vue.js 是什么

Vue.js（读音 /vjuː/，类似于 view） 是一套构建用户界面的**渐进式框架**。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注**视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。

>什么叫渐进式
>
>没有多做职责之外的事,它只是个轻量视图而已，只做了自己该做的事，没有做不该做的事，仅此而已。
>
>你可以在原有的项目上面，把一两个组件改用它实现，当jQuery用
>
>也可以整个用它全家桶开发，当Angular用
>
>还可以在页面上当模板引擎用
>



### 特点
1. 轻量级的框架
2. 双向数据绑定
3. 指令
4. 组件化


### 优点
1. 简单：官方文档很清晰，简单易学，国人开发，生态好，社区、文档齐全
2. 快速：异步批处理方式更新DOM （Virtual DOM）
3. 组合：解耦的、可复用的组件
4. 紧凑：28.96kb min+gzip 且无依赖
5. 强大：性能好，表达式、指令、组件化···



### 缺点

1. 不支持IE8 


## vue的使用


### 安装

> vue的 [安装方式](https://cn.vuejs.org/v2/guide/installation.html) 有很多，这里所有的示例都是通过 `<script>` 标签引入, Vue就会被注册为一个全局变量。
> 
> `<script src="https://vuejs.org/js/vue.min.js"></script>`
> 


### 实例

**hello vue！**

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>hello vue!</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="app">
			{{ message }}
		</div>

		<script type="text/javascript">
			var app = new Vue({
				el: '#app',
				data: {
					message: 'Hello Vue!'
				}
			})
		</script>

	</body>

</html>

```
>查看[运行结果](http://runjs.cn/code/uhuk48vf) *此运行页面，仅供查看结果使用，操作时，把代码复制到新建的html文件中运行，下同*
>
>
>
>我们已经生成了我们的第一个 Vue 应用！看起来这跟单单渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和 DOM 已经被绑定在一起，所有的元素都是**响应式的**。我们如何知道？打开你的浏览器的控制台（就在这个页面打开,或者右键点击`审查元素`），在控制台输入 `app.message='xxxxx'`，你将看到上例相应地更新。


**通过指令绑定数据**

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>指令</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="app">
			<p>
				鼠标悬停在图片上可以看到提示内容
			</p>
			
			<img v-bind:title="message" src="https://img.alicdn.com/imgextra/i2/2296013456/TB2ia_RvKJ8puFjy1XbXXagqVXa_!!2296013456.png"/>
		</div>

		<script type="text/javascript">
			var app = new Vue({
				el: '#app',
				data: {
					message: '这是个宝箱！'
				}
			})
		</script>

	</body>

</html>

```

> 查看[运行结果](http://runjs.cn/code/us07dv4m)  
> 
> 你看到的 `v-bind` 属性被称为指令。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊属性。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。简言之，这里该指令的作用是：“将这个元素节点的 title 属性和 Vue 实例的 message 属性保持一致”。
> 
>
再次打开浏览器的 JavaScript 控制台输入 `app.message = '新消息'`，就会再一次看到这个绑定了 title 属性的 HTML 已经进行了更新。



**条件与循环**

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>条件与循环</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="app">
			<p v-if="seen">现在你看到我了</p>
			
			<ul>
				<li v-for="todo in todos">
			      {{ todo.text }}
			    </li>
			</ul>
			
		</div>

		<script type="text/javascript">
			var app = new Vue({
				el: '#app',
				data: {
					seen: true,
					todos: [
				      { text: '学习 JavaScript' },
				      { text: '学习 Vue' },
				      { text: '整个牛项目' }
				    ]
				}
			})
		</script>

	</body>

</html>

```

> [查看结果](http://runjs.cn/code/nslezwtw)
> 
> 继续在控制台设置 `app.seen = false`，你会发现 `“现在你看到我了”` 消失了。
> 
> 在控制台里，输入 `app.todos.push({ text: '新项目' })`，你会发现列表中添加了一个新项。



**双向绑定**

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>表单双向绑定</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="app">
			<p>{{message}}</p>
			<input type="text" name="test" id="test" value="" v-model="message" />
		</div>

		<script type="text/javascript">
			var app = new Vue({
				el: '#app',
				data: {
					message :'hello vue!'
				}
			})
		</script>

	</body>

</html>
```

>  查看[运行结果](http://runjs.cn/code/wq8ioygn)
> 
> Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。


**使用JavaScript表达式**

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>表达式</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="app">
			<div v-if="Math.random() > 0.5">
				随机数大于 0.5
			</div>
			<div v-else>
				随机数不大于 0.5
			</div>
		</div>
		<script>
			new Vue({
				el: '#app'
			})
		</script>

	</body>

</html>

```


> [查看结果](http://runjs.cn/code/9ps7l6dp)









### 模板语法


#### 文本

数据绑定最常见的形式就是使用 `Mustache` 语法（双大括号）的文本插值：

```
<span>Message: {{ msg }}</span>

```


`Mustache` 标签将会被替代为对应数据对象上 `msg` 属性的值。无论何时，绑定的数据对象上 `msg` 属性发生了改变，插值处的内容都会更新。

通过使用 `v-once` 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定：

```
<span v-once>This will never change: {{ msg }}</span>
```


双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 `v-html` 指令：

```
<div v-html="rawHtml"></div>
```

这个 div 的内容将会被替换成为属性值 rawHtml，直接作为 HTML —— 数据绑定会被忽略。注意，你不能使用 `v-html` 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担任 UI 重用与复合的基本单元。

>你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 **XSS** 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容插值。

#### 属性

`Mustache` 不能在 HTML 属性中直接使用，应使用 `v-bind指令`：

```
<div v-bind:id="dynamicId"></div>
```

这对布尔值的属性也有效 —— 如果条件被求值为 `false` 的话该属性会被移除：

```
<button v-bind:disabled="isButtonDisabled">Button</button>
```


#### 使用 JavaScript 表达式


迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。

```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。

```
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

>模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 `Math` 和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。



#### 指令

指令（Directives）是带有 `v-` 前缀的特殊属性。指令属性的值预期是单一 `JavaScript` 表达式（除了 `v-for`，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。让我们回顾一下在介绍里的例子：

```
<p v-if="seen">现在你看到我了</p>
```


这里， `v-if` 指令将根据表达式 `seen` 的值的真假来移除/插入 `<p>` 元素。


#### 参数

一些指令能接受一个“参数”，在指令后以冒号指明。例如， `v-bind` 指令被用来响应地更新 `HTML` 属性：

```
<a v-bind:href="url"></a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` 属性与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```
<a v-on:click="doSomething">
```

在这里参数是监听的事件名。我们也会更详细地讨论事件处理。


#### 修饰符

修饰符（Modifiers）是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```
<form v-on:submit.prevent="onSubmit"></form>
```
之后当我们更深入地了解 `v-on` 与 `v-model`时，会看到更多修饰符的使用。


#### 缩写

`v-` 前缀在模板中是作为一个标示 Vue 特殊属性的明显标识。当你使用 Vue.js 为现有的标记添加动态行为时，它会很有用，但对于一些经常使用的指令来说有点繁琐。同时，当搭建 Vue.js 管理所有模板的 `SPA` 时，`v-` 前缀也变得没那么重要了。因此，Vue.js 为两个最为常用的指令提供了特别的缩写：

**v-bind 缩写**

```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

**v-on 缩写**

```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

>它们看起来可能与普通的 HTML 略有不同，但 : 与 @ 对于属性名来说都是合法字符，在所有支持 Vue.js 的浏览器都能被正确地解析。而且，它们不会出现在最终渲染的标记。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。










## 计算属性

模板内的表达式是非常便利的，但是它们实际上只用于简单的运算。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

在这种情况下，模板不再简单和清晰。在意识到这是反向显示 `message` 之前，你不得不再次确认第二遍。当你想要在模板中多次反向显示 `message` 的时候，问题会变得更糟糕。

这就是对于任何复杂逻辑，你都应当使用计算属性的原因。

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>计算属性</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="example">
			<p>Original message: "{{ message }}"</p>
			<input type="text" name="test" id="test" value="" v-model="message"/>
			<p>Computed reversed message: "{{ reversedMessage }}"</p>
		</div>

		<script>
			var vm = new Vue({
				el: '#example',
				data: {
					message: 'Hello'
				},
				computed: {
					// a computed getter
					reversedMessage: function() {
						// `this` points to the vm instance
						return this.message.split('').reverse().join('')
					}
				}
			})
		</script>

	</body>

</html>
```

> [查看结果](http://runjs.cn/code/dp4a3t59)
> 
> 


这里我们声明了一个计算属性 `reversedMessage` 。我们提供的函数将用作属性 `vm.reversedMessage` 的 `getter` 。

```
console.log(vm.reversedMessage) // -> 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // -> 'eybdooG'

```

你可以打开浏览器的控制台，自行修改例子中的 `vm` 。 `vm.reversedMessage` 的值始终取决于 `vm.message` 的值。

你可以像绑定普通属性一样在模板中绑定计算属性。 Vue 知道 `vm.reversedMessage` 依赖于 `vm.message` ，因此当 vm.message 发生改变时，所有依赖于 `vm.reversedMessage` 的绑定也会更新。而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 `getter` 是没有副作用，这使得它易于测试和推理。



## Class 与 Style 绑定

数据绑定一个常见需求是操作元素的 `class` 列表和它的内联样式。因为它们都是属性 ，我们可以用`v-bind` 处理它们：只需要计算出表达式最终的字符串。不过，字符串拼接麻烦又易错。因此，在 `v-bind` 用于 `class` 和 `style` 时， Vue.js 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组。



### 对象语法

我们可以传给 `v-bind:class` 一个对象，以动态地切换 `class` 。

```
<div v-bind:class="{ active: isActive }"></div>
```

上面的语法表示 `class active` 的更新将取决于数据属性 `isActive` 是否为真值 。
我们也可以在对象中传入更多属性用来动态切换多个 `class` 。此外， `v-bind:class` 指令**可以与普通的 class 属性共存**。如下模板:

```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

如下 data:

```
data: {
  isActive: true,
  hasError: false
}
```

渲染为:

```
<div class="static active"></div>
```

当 `isActive` 或者 `hasError` 变化时，`class` 列表将相应地更新。例如，如果 `hasError` 的值为 `true` ， class列表将变为 `"static active text-danger"`。
你也可以直接绑定数据里的一个对象：

```
<div v-bind:class="classObject"></div>

data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

渲染的结果和上面一样。我们也可以在这里绑定返回对象的计算属性。这是一个常用且强大的模式：

```
<div v-bind:class="classObject"></div>

data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### 数组语法

我们可以把一个数组传给 `v-bind:class` ，以应用一个 `class` 列表：

```
<div v-bind:class="[activeClass, errorClass]">
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

渲染为:

```
<div class="active text-danger"></div>
```

如果你也想根据条件切换列表中的 `class` ，可以用三元表达式：

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```

此例始终添加 `errorClass` ，但是只有在 `isActive` 是 `true` 时添加 `activeClass` 。
不过，当有多个条件 `class` 时这样写有些繁琐。可以在数组语法中使用对象语法：

```
<div v-bind:class="[{ active: isActive }, errorClass]">
```



## 条件渲染

### v-if

语法格式：

```
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```


#### 在`<template>` 中配合 v-if 条件渲染一整组

因为 `v-if` 是一个指令，需要将它添加到一个元素上。但是如果我们想切换多个元素呢？此时我们可以把一个 `<template>` 元素当做包装元素，并在上面使用 `v-if`。最终的渲染结果不会包含 `<template>` 元素。

```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### v-else

你可以使用 `v-else` 指令来表示 `v-if` 的 `else` 块：

```
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

> `v-else` 元素必须紧跟在 `v-if` 或者 `v-else-if` 元素的后面——否则它将不会被识别。


### v-else-if

>2.1.0 新增

`v-else-if`，顾名思义，充当 `v-if` 的 `else-if` 块。可以链式地使用多次：

```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

类似于 `v-else`，`v-else-if` 必须紧跟在 `v-if` 或者 `v-else-if` 元素之后。



### v-show

另一个用于根据条件展示元素的选项是 `v-show` 指令。用法大致一样：

```
<h1 v-show="ok">Hello!</h1>
```

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 是简单地切换元素的 `CSS` 属性 `display` 。

> 注意， `v-show` 不支持 `<template>` 语法，也不支持 `v-else`。

### v-if vs v-show

`v-if` 是“真正的”条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下， `v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 `CSS` 进行切换。

一般来说， `v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件不太可能改变，则使用 `v-if` 较好。


## 列表渲染


### v-for

我们用 `v-for` 指令根据一组数组的选项列表进行渲染。 `v-for` 指令需要以 `item in items` 形式的特殊语法， `items` 是源数据数组并且 `item` 是数组元素迭代的别名。

基本用法

```
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      {message: 'Foo' },
      {message: 'Bar' }
    ]
  }
})
```

结果：

```
Foo
Bar
```

在 `v-for` 块中，我们拥有对父作用域属性的完全访问权限。 `v-for` 还支持一个可选的第二个参数为当前项的索引。

```
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

结果：

```
Parent - 0 - Foo
Parent - 1 - Bar
```

你也可以用 `of` 替代 `in` 作为分隔符，因为它是最接近 `JavaScript` 迭代器的语法：

```
<div v-for="item of items"></div>
```


### Template v-for

如同 `v-if` 模板，你也可以用带有 `v-for` 的 `<template>` 标签来渲染多个元素块。例如：

```
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider"></li>
  </template>
</ul>
```


### 对象迭代 v-for

你也可以用 `v-for` 通过一个对象的属性来迭代。

```
<ul id="repeat-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```

```
new Vue({
  el: '#repeat-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

结果：

```
John
Doe
30
```


你也可以提供第二个的参数为键名：

```
<div v-for="(value, key) in object">
  {{ key }} : {{ value }}
</div>
```

第三个参数为索引：

```
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }} : {{ value }}
</div>
```

在遍历对象时，是按 `Object.keys()` 的结果遍历，但是不能保证它的结果在不同的 `JavaScript` 引擎下是一致的。


### 整数迭代 v-for

`v-for` 也可以取整数。在这种情况下，它将重复多次模板。


```
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

结果：

```
1 2 3 4 5 6 7 8 9 10
```



## 事件处理器

### 监听事件

可以用 `v-on` 指令监听 `DOM` 事件来触发一些 `JavaScript` 代码。

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>事件处理</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="example-1">
			<button v-on:click="counter += 1">增加 1</button>
			<p>这个按钮被点击了 {{ counter }} 次。</p>
		</div>
		<script type="text/javascript">
			var example1 = new Vue({
				el: '#example-1',
				data: {
					counter: 0
				}
			})
		</script>

	</body>

</html>
```

> [查看结果](http://runjs.cn/code/dplb7ulk)



### 方法事件处理器

许多事件处理的逻辑都很复杂，所以直接把 `JavaScript` 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 可以接收一个定义的方法来调用。

示例：

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>事件处理</title>
		<script src="https://vuejs.org/js/vue.min.js"></script>
	</head>

	<body>

		<div id="example-2">
			<!-- `greet` 是在下面定义的方法名 -->
			<button v-on:click="greet">Greet</button>
		</div>
		<script type="text/javascript">
			var example2 = new Vue({
				el: '#example-2',
				data: {
					name: 'Vue.js'
				},
				// 在 `methods` 对象中定义方法
				methods: {
					greet: function(event) {
						// `this` 在方法里指当前 Vue 实例
						alert('Hello ' + this.name + '!')
						// `event` 是原生 DOM 事件
						if(event) {
							alert(event.target.tagName)
						}
					}
				}
			})
			// 也可以用 JavaScript 直接调用方法
			example2.greet() // -> 'Hello Vue.js!'
		</script>

	</body>

</html>
```

> [查看结果](http://runjs.cn/code/urqxhdwy)
 


### 内联处理器方法

除了直接绑定到一个方法，也可以用内联 `JavaScript` 语句：

```
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```


```
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

结果：

```
Say hi
Say what
```


有时也需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 `$event` 把它传入方法：

```
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
```

### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在 `methods` 中轻松实现这点，但更好的方式是：`methods` 只有纯粹的数据逻辑，而不是去处理 `DOM` 事件细节。

为了解决这个问题， `Vue.js` 为 `v-on` 提供了 事件修饰符。通过由点`(.)`表示的指令后缀来调用修饰符。

```
.stop
.prevent
.capture
.self
.once

<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（比如不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```


>使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `@click.prevent.self` 会阻止所有的点击，而 `@click.self.prevent` 只会阻止元素上的点击。




## 进阶玩法

此篇为入门教程，想成为高级玩家，[请点这里](https://cn.vuejs.org/)

>附上一个vue写的小项目，当做实例：[点我直达](http://ocean.taofen8.com/vip/actPage)



> 转载请注明出处
>
> 作者：xuess<wuniu2010@126.com>
> 
> 时间：2017年06月16日
> 
> 最后修改时间：2017年06月16日