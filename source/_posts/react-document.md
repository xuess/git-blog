---
title: react学习记录
date: 2017-04-02 15:20:14
tags: react
---


### 知识体系



### 组件

上面代码中，变量 `HelloMessage` 就是一个组件类。模板插入`<HelloMessage />` 时，会自动生成 `HelloMessage` 的一个实例（下文的"组件"都指组件类的实例）。所有组件类都必须有自己的 `render` 方法，用于输出组件。

注意，组件类的第一个字母必须大写，否则会报错，比如`HelloMessage`不能写成`helloMessage`。
<br>
另外，组件类只能包含`一个顶层标签`，否则也会报错。


添加组件属性，有一个地方需要注意，就是 `class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的`保留字`。


### this.props.children

`this.props` 对象的属性与组件的属性一一对应，但是有一个例外，就是 `this.props.children` 属性。它表示组件的`所有子节点`


这里需要注意， `this.props.children` 的值有三种可能：如果当前组件`没有子节点`，它就是 `undefined` ;如果有`一个子节点`，数据类型是 `object` ；如果有`多个子节点`，数据类型就是 `array` 。所以，处理 `this.props.children` 的时候要小心。

`React` 提供一个工具方法 `React.Children` 来处理 `this.props.children` 。我们可以用 `React.Children.map` 来遍历子节点，而不用担心 `this.props.children` 的数据类型是 `undefined` 还是 `object`。更多的 `React.Children` 的方法 [参考官网文档](https://reactjs.org/docs/react-api.html#react.children)



### PropTypes

组件的属性可以接受任意值，字符串、对象、函数等等都可以。有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求。

组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求

>注意: `React.PropTypes` 自 React `v15.5` 起已弃用。请使用 [prop-types](https://www.npmjs.com/package/prop-types) 库代替。


```
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};

```

`PropTypes` 包含一整套验证器，可用于确保你接收的数据是有效的。在这个示例中，我们使用了 `PropTypes.string`。当你给属性传递了无效值时，`JavsScript` 控制台将会打印警告。出于性能原因，`propTypes` 只在开发模式下进行检查。


下面是使用不同验证器的例子：

```
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为以下 JS 原生类型
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
  optionalNode: PropTypes.node,

  // 一个 React 元素
  optionalElement: PropTypes.element,

  // 你也可以声明属性为某个类的实例，这里使用 JS 的
  // instanceof 操作符实现。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你也可以限制你的属性值是某个特定值之一
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 限制它为列举类型之一的对象
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个指定元素类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 一个指定类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个指定属性及其类型的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 你也可以在任何 PropTypes 属性后面加上 `isRequired` 
  // 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你也可以指定一个自定义验证器。它应该在验证失败时返回
  // 一个 Error 对象而不是 `console.warn` 或抛出异常。
  // 不过在 `oneOfType` 中它不起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 不过你可以提供一个自定义的 `arrayOf` 或 `objectOf` 
  // 验证器，它应该在验证失败时返回一个 Error 对象。 它被用
  // 于验证数组或对象的每个值。验证器前两个参数的第一个是数组
  // 或对象本身，第二个是它们对应的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};

```



### 获取真实的DOM节点 ref

有时需要从组件获取真实 DOM 的节点，这时就要用到 `ref` 属性


```
//es5
var MyComponent = React.createClass({
  handleClick: function() {
    this.refs.myTextInput.focus();
  },
  render: function() {
    return (
      <div>
        <input type="text" ref="myTextInput" />
        <input type="button" value="Focus the text input" onClick={this.handleClick} />
      </div>
    );
  }
});

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);
```



[参考官方文档](https://doc.react-china.org/docs/refs-and-the-dom.html)

>旧版 API：String 类型的 Refs
如果你之前使用过 React ，你可能了解过之前的API中的 string 类型的 ref 属性，比如 “textInput” ，你可以通过 this.refs.textInput 访问DOM节点。我们不建议使用它，因为 String 类型的 refs 存在问题。它已过时并可能会在未来的版本是移除。如果你目前还在使用 this.refs.textInput 这种方式访问 refs ，我们建议用回调函数的方式代替。




