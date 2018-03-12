---
title: facebook immutable.js 意义何在
date: 2017-09-18 14:57:34
tags: js
---




Javascript中对象都是参考类型，也就是a={a:1}; b=a; b.a=10;你发现a.a也变成10了。可变的好处是节省内存或是利用可变性做一些事情，但是，在复杂的开发中它的副作用远比好处大的多。于是才有了浅copy和深copy，就是为了解决这个问题。


举个常见例子：

```

var  defaultConfig = { /* 默认值 */};
var config = $.extend({}, defaultConfig,initConfig); 
// jQuery用法。initConfig是自定义值
var config = $.extend(true, {}, defaultConfig, initConfig); 
//如果对象是多层的，就用到deep-copy

```

了`ES6`出现原生的`assign`方法，但它相当于是`浅copy`。如果有了不可变的数据结构就省心了，`ES5.1`中对象有了`freeze`方法，也是`浅copy`，


```
a=Object.freeze({a:1}); 
b=a; b.a=10;

a.a还是1。在实际开发中浅copy通常不够。
```


如果用immutableJS:

```

var  defaultConfig = Immutable.fromJS({ /* 默认值 */}); 
var config = defaultConfig.merge(initConfig); 
// defaultConfig不会改变，返回新值给config
var config = defaultConfig.mergeDeep(initConfig);//深层merge


```

上述用deep-copy也可以做到，差别在于性能。每次`deep-copy`都要把整个对象递归的复制一份。而`immutable`的实现有些像`链表`，添加一个新结点把旧结点的父子关系转移到新结点上，性能提升很多，想深挖原理请看这里：

[Persistent data structure](https://en.wikipedia.org/wiki/Persistent_data_structure)。

ImmutableJS给的远不止这些，它提供了7种不可变的数据结构：`List`, `Stack`, `Map`, `OrderedMap`, `Set`, `OrderedSet`, `Record` （详见文档[Immutable.js](http://facebook.github.io/immutable-js/docs/#/)，文档很geek，打开console试吧）。

immutableJS ＋ 原生Javascript等于真正的函数式编程。遍历对象不再用`for-in`，可以这样:

```
Immutable.fromJS({a:1, b:2, c:3}).map(function(value, key) { /* do some thing */});
实现一个map-reduce:
var o = Immutable.fromJS({a:{a:1}, b:{a:2}, c:{a:3}});
o.map(function(e){ return e.get('a'); }).reduce(function(e1, e2){ return e1 + e2; }, 0);
修改藏在深处的值，可以这样：
var o = Immutable.fromJS({a:[{a1:1}, {b:[{t:1}]}, {c1:2}], b:2, c:3});
o = o.setIn(['a', 1, 'b', 0, 't'], 100);  
// t赋值o = o.updateIn(['a', 1, 'b', 0, 't'], function(e){ return e * 100; }); // t * 100

比较两个对象是否完全相等:
o1.equals(o2)
```


远不止这些，`immutableJS`提供了强大的api自己去看吧。由于是不可变的，可以放心的对对象进行任意操作。在`React`开发中，频繁操作state对象或是store，配合immutableJS快、安全、方便。



>form链接：https://www.zhihu.com/question/28016223/answer/50154351
