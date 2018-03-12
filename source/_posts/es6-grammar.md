---
title: es6语法简介
date: 2017-01-25 14:56:09
tags:
---


## let与var用法区别

```
//var 
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10

------------------------------

//let
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6


------------------------------


//如果不用 let 实现类似功能
function iteratorFactory(i){
    var onclick = function(e){
        console.log(i)
    }
    return onclick;
}
var clickBoxs = document.querySelectorAll('.clickBox')
for (var i = 0; i < clickBoxs.length; i++){
    clickBoxs[i].onclick = iteratorFactory(i)
}


```


## class, extends, super

```

class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

//继承
class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello

```

上面代码首先用`class`定义了一个“类”，可以看到里面有一个`constructor`方法，这就是构造方法，而`this`关键字则代表实例对象。简单地说，`constructor`内定义的方法和属性是实例对象自己的，而`constructor`外定义的方法和属性则是所有实例对象可以共享的。

`Class`之间可以通过`extends`关键字实现继承，这比`ES5`的通过修改原型链实现继承，要清晰和方便很多。

`super`关键字，它指代父类的实例（即父类的`this`对象）。子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。
这是因为子类没有自己的`this`对象，而是继承父类的`this`对象，然后对其进行加工。如果不调用`super`方法，子类就得不到`this`对象。

`ES6`的继承机制，实质是先创造父类的实例对象`this`（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。


## 箭头函数 arrow function

```
function(i){ return i + 1; } //ES5
(i) => i + 1 //ES6
```

如果方程比较复杂，则需要用`{}`把代码包起来：

```
//es5
function(x, y) { 
    x++;
    y--;
    return x + y;
}
//es6
(x, y) => {x++; y--; return x+y}
```

除了看上去更简洁以外，`arrow function`还有一项超级无敌的功能！
长期以来，JavaScript语言的`this`对象一直是一个令人头痛的问题，在对象方法中使用`this`，必须非常小心。例如：

```
//错误代码
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout(function(){
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

 var animal = new Animal()
 animal.says('hi')  //undefined says hi


```

运行上面的代码会报错，这是因为`setTimeout`中的`this`指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：


1.第一种是将`this`传给`self`,再用`self`来指代`this`

```
says(say){
   var self = this;
   setTimeout(function(){
       console.log(self.type + ' says ' + say)
}, 1000)
```

2.第二种方法是用`bind(this)`,即

```
says(say){
    setTimeout(function(){
        console.log(this.type + ' says ' + say)
    }.bind(this), 1000)
}
```

但现在我们有了箭头函数，就不需要这么麻烦了：

```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi

```

当我们使用箭头函数时，函数体内的`this`对象，就是`定义时`所在的对象，而不是使用时所在的对象。
并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本`没有自己的this`，它的this是`继承外面的`，因此内部的this就是外层代码块的this。


## 模板字符串 template string

```
//不用模板字符串 写法
$("#result").append(
  "There are <b>" + basket.count + "</b> " +
  "items in your basket, " +
  "<em>" + basket.onSale +
  "</em> are on sale!"
);


//使用模板字符串写法
$("#result").append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);

```

> 用反引号（\`）来标识起始，用`${}`来引用变量，而且所有的`空格和缩进`都会被保留在输出之中(==这个需要注意==)



## 解构 destructuring

`ES6`允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（`Destructuring`）。

```
let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}

//使用es6解构
let cat = 'ken'
let dog = 'lili'
let zoo = {cat, dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}

//反过来可以这么写：
let dog = {type: 'animal', many: 2}
let { type, many} = dog
console.log(type, many)   //animal 2

```


## 默认值 default


```

function animal(type){
    type = type || 'cat'  
    console.log(type)
}
animal()

//ES6
function animal(type = 'cat'){
    console.log(type)
}
animal()

```


## 展开操作符 rest arguments (...)

[扩展运算符详细介绍](http://blog.csdn.net/qq_30100043/article/details/53391308)

```
function animals(...types){
    console.log(types)
}
animals('cat', 'dog', 'fish') //["cat", "dog", "fish"]
```

## import export

**传统的写法**CommonJS(服务器端)和AMD（浏览器端，如require.js）

AMD写法

```

//content.js
define('content.js', function(){
    return 'A cat';
})

//index.js
require(['./content.js'], function(animal){
    console.log(animal);   //A cat
})

```

CommonJS

```
//index.js
var animal = require('./content.js')

//content.js
module.exports = 'A cat'

```

ES6的写法

```
//index.js
import animal from './content'

//content.js
export default 'A cat'

```

ES6 module的其他高级用法

```
//content.js
export default 'A cat'    
export function say(){
    return 'Hello!'
}    
export const type = 'dog' 




//index.js
import { say, type } from './content'  
let says = say()
console.log(`The ${type} says ${says}`)  //The dog says Hello
```

>这里输入的时候要注意：`大括号`里面的`变量名`，必须与被导入模块（content.js）对外接口的`名称相同`。
>如果还希望输入content.js中输出的`默认值`(default), 可以写在大括号外面。

```
//index.js
import animal, { say, type } from './content'  
let says = say()
console.log(`The ${type} says ${says} to ${animal}`)  
//The dog says Hello to A cat
```

修改变量名

此时我们不喜欢type这个变量名，因为它有可能重名，所以我们需要修改一下它的变量名。在es6中可以用`as`实现一键换名。

```
//index.js
import animal, { say, type as animalType } from './content'  
let says = say()
console.log(`The ${animalType} says ${says} to ${animal}`)  
//The dog says Hello to A cat

```


模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。

```
//index.js

import animal, * as content from './content'  
let says = content.say()
console.log(`The ${content.type} says ${says} to ${animal}`)  
//The dog says Hello to A cat

```

>通常星号`*`结合`as`一起使用比较合适。


# 其他 特性

[阮一峰老师的es6入门](http://es6.ruanyifeng.com/)



----
[参考1](https://segmentfault.com/a/1190000004368132)

[参考2](https://segmentfault.com/a/1190000004365693)