---
title: vue使用速查
date: 2016-04-26 15:26:48
tags: vue
---



### 组件直接数据传递

### 使用 Prop 传递数据


```javascript

子组件要显式地用 props 选项声明它预期的数据：

Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 也可以在模板中使用
  // 同样也可以在 vm 实例中通过 this.message 来使用
  template: '<span>{{ message }}</span>'
})


然后我们可以这样向它传入一个普通字符串：

<child message="hello!"></child>


```

```javascript
//HTML 特性是不区分大小写的。所以，当使用的不是字符串模板时，camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名)：


Vue.component('child', {
  // 在 JavaScript 中使用 camelCase
  props: ['myMessage'],
  template: '<span>{{ myMessage }}</span>'
})

<!-- 在 HTML 中使用 kebab-case -->
<child my-message="hello!"></child>


如果你使用字符串模板，则没有这些限制。
```


## 动态 Prop

```html

与绑定到任何普通的 HTML 特性相类似，我们可以用 v-bind 来动态地将 prop 绑定到父组件的数据。每当父组件的数据变化时，

该变化也会传导给子组件：

<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>


你也可以使用 v-bind 的缩写语法：

<child :my-message="parentMsg"></child>


```


```css

如果你想把一个对象的所有属性作为 prop 进行传递，可以使用不带任何参数的 v-bind (即用 v-bind 而不是 v-bind:prop-name)。

例如，已知一个 todo 对象：

todo: {
  text: 'Learn Vue',
  isComplete: false
}

然后：

<todo-item v-bind="todo"></todo-item>

将等价于：

<todo-item
  v-bind:text="todo.text"
  v-bind:is-complete="todo.isComplete"
>
</todo-item>

```


### 字面量语法 vs 动态语法

初学者常犯的一个错误是使用字面量语法传递数值：

```html
<!-- 传递了一个字符串 "1" -->
<comp some-prop="1"></comp>
因为它是一个字面量 prop，它的值是字符串 "1" 而不是一个数值。如果想传递一个真正的 JavaScript 数值，则需要使用 v-bind，从而让它的值被当作 JavaScript 表达式计算：

<!-- 传递真正的数值 -->
<comp v-bind:some-prop="1"></comp>

```

### 单向数据流

Prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop。如果你这么做了，Vue 会在控制台给出警告。

在两种情况下，我们很容易忍不住想去修改 prop 中数据：

1.Prop 作为初始值传入后，子组件想把它当作局部数据来用；

2.Prop 作为原始数据传入，由子组件处理成其它数据输出。


对这两种情况，正确的应对方式是：



```javascript
//定义一个局部变量，并用 prop 的值初始化它：
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}


//定义一个计算属性，处理 prop 的值并返回：
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}

```

> 注意在 JavaScript 中对象和数组是`引用类型`，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会`影响父组件`的状态。



### Prop 验证

我们可以为组件的 prop 指定验证规则。如果传入的数据不符合要求，Vue 会发出警告。这对于开发给他人使用的组件非常有用。

防止数据类型错误，引起难以排查的错误


要指定验证规则，需要用`对象`的形式来定义`prop`，而不能用字符串数组：

```javascript

Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 指允许任何类型)
    propA: Number,
    // 可能是多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数值且有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})



```

type 可以是下面原生构造器：

```bash 
String
Number
Boolean
Function
Object
Array
Symbol
```

type 也可以是一个自定义构造器函数，使用 instanceof 检测。

> 当 prop 验证失败，Vue 会抛出警告 (如果使用的是开发版本)。注意 prop 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用。



### 非 Prop 特性
所谓非 prop 特性，就是指它可以直接传入组件，而不需要定义相应的 `prop`。

尽管为组件定义明确的 `prop` 是推荐的传参方式，组件的作者却并不总能预见到组件被使用的场景。所以，组件可以接收任意传入的特性，这些特性都会被添加到组件的根元素上。

例如，假设我们使用了第三方组件 `bs-date-input`，它包含一个 `Bootstrap` 插件，该插件需要在 `input` 上添加 `data-3d-date-picker` 这个特性。这时可以把特性直接添加到组件上 (不需要事先定义 prop)：

```html
<bs-date-input data-3d-date-picker="true"></bs-date-input>

```
添加属性 `data-3d-date-picker="true"` 之后，它会被自动添加到 `bs-date-input` 的根元素上。

#### 替换/合并现有的特性

假设这是 bs-date-input 的模板：

```html
<input type="date" class="form-control">
```

为了给该日期选择器插件增加一个特殊的主题，我们可能需要增加一个特殊的 `class`，比如：

```html
<bs-date-input
  data-3d-date-picker="true"
  class="date-picker-theme-dark"
></bs-date-input>
```

在这个例子当中，我们定义了两个不同的 `class` 值：

* `form-control`，来自组件自身的模板
* `date-picker-theme-dark`，来自父组件
* 
对于多数特性来说，传递给组件的值会覆盖组件本身设定的值。即例如传递 `type="large"` 将会覆盖 `type="date"` 且有可能破坏该组件！所幸我们对待 `class` 和 `style` 特性会更聪明一些，这两个特性的值都会做合并 (`merge`) 操作，让最终生成的值为：`form-control date-picker-theme-dark`。


### 自定义事件

我们知道，父组件使用 prop 传递数据给子组件。但子组件怎么跟父组件通信呢？这个时候 Vue 的自定义事件系统就派得上用场了。


####  使用 v-on 绑定自定义事件

每个 Vue 实例都实现了事件接口，即：

使用 `$on(eventName)` 监听事件
使用 `$emit(eventName)` 触发事件

> Vue 的事件系统与浏览器的`EventTarget API` 有所不同。尽管它们的运行起来类似，但是 `$on` 和 `$emit` 并不是`addEventListener` 和 `dispatchEvent` 的别名。

另外，父组件可以在使用子组件的地方直接用 `v-on` 来监听子组件触发的事件。

> 不能用 $on 侦听子组件释放的事件，而必须在模板里直接用 v-on 绑定，参见下面的例子。

下面是一个例子：

```html
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>
```

```javascript
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})

```

在本例中，子组件已经和它外部完全解耦了。它所做的只是报告自己的内部事件，因为父组件可能会关心这些事件。请注意这一点很重要。


#### 给组件绑定原生事件

有时候，你可能想在某个组件的根元素上监听一个原生事件。可以使用 `v-on` 的修饰符 `.native`。例如：

```html
<my-component v-on:click.native="doTheThing"></my-component>
```


