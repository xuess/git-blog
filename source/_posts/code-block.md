---
title: 常用代码片段
date: 2016-06-09 14:47:21
tags:
---


# 常用代码片段


### a标签创建连接点击跳转

```javascript
var el = document.createElement("a");
document.body.appendChild(el);
el.href = 'http://www.baidu.com'; //url 是你得到的连接
//el.target = '_new'; //指定在新窗口打开
el.click();
document.body.removeChild(el);
return ;
```


### transform垂直左右居中

```
width: 80%;
padding: 10px;
border: 1px solid #E94637;
position: fixed;
text-align: center;
z-index: 13;
left: 50%;
top: 50%;
transform: translate(-50%, -50%);
-webkit-transform: translate(-50%, -50%);
```

### js异步上传 from表单

```javascript
//异步上传
new FormData($('#uploadForm')[0])
```



### 事件节流

```javascript
/**
 * 事件节流 或者 防止滚动卡顿 17毫秒左右每帧
 * @param {Object} fn
 * @param {Object} interval
 */
function throttle(fn, interval) {
	var doing = false;
	return function() {
		if(doing) {
			return;
		}
		doing = true;
		fn.apply(this, arguments);
		setTimeout(function() {
			doing = false;
		}, interval);
	};
};

```

### vue初始化模板

```javascript
var vm = new Vue({
	el: "#main",
	data: {

	},
	//计算属性
	computed: {

	},
	//方法
	methods: {
		
	},
	//监听
	watch: {

	},
	//	生命周期钩子的一些使用方法：

	//	beforecreate : 可以在这加个loading事件，在加载实例时触发
	//	created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用
	//	mounted : 挂载元素，获取到DOM节点
	//	updated : 如果对数据统一处理，在这里写上相应函数
	//	beforeDestroy : 可以做一个确认停止事件的确认框
	//	nextTick : 更新数据后立即操作dom

	//vue实例创建之前
	beforecreate: function() {

	},
	//vue实例创建之后
	created: function() {

	}
});
```