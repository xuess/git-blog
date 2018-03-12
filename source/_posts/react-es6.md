---
title: react es6
date: 2017-03-22 15:19:12
tags: react
---


```
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));

// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});

箭头函数 ：
var aaa = (a,b) => {console.log(a)}

var aaa = function (a,b){
    console.log(a)
}


var aaa = (a,b) =>(
    a+b
)

var  aaa = function(a,b){
    return a+b
}


var aaa = (a,b) =>(
    {a:a,b:b}
)

var  aaa = function(a,b){
    return {a:a,b:b}
}


```


计算属性 

```
**es6**
this.setState({
  [name]: value
});

**es5**
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```