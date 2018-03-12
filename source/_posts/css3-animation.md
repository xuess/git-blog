---
title: css3animation动画
date: 2016-05-11 14:55:37
tags: css3
---


##CSS3 transition

demo

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>css3 transition</title>
	</head>
	<body>
		<style type="text/css">
			span {
				position: absolute;
				top: 30px;
				left: 50px;
				width: 200px;
				height: 200px;
				background: gold;
				color: #000000;
				font-size: 12px;
				transition: width 10s,background-color 10s, height 10s, left 10s, top 10s, font-size 10s, line-height 10s;
				-webkit-transition: width 10s,background-color 10s, height 10s, left 10s, top 10s, font-size 10s, line-height 10s;
			}
			span:hover {
				top: 300px;
				left: 500px;
				width: 100px;
				height: 100px;
				font-size: 35px;
				background: green;
			}
		</style>
		<span>aaa</span>
	</body>

</html>

```

说明：

transition 属性是一个简写属性，用于设置四个过渡属性：

* transition-property
* transition-duration
* transition-timing-function
* transition-delay

值| 描述
---|----
默认值：| all 0 ease 0 | 
JavaScript 语法：| `object.style.transition="width 2s"`

语法

```
transition: property duration timing-function delay;
```



值| 描述
---|----
transition-property |	规定设置过渡效果的 CSS 属性的名称。 如：`transition-property:width;`
transition-duration	| 规定完成过渡效果需要多少秒或毫秒。如：`transition-duration: 5s;`
transition-timing-function	 | 规定速度效果的速度曲线。如：`transition-timing-function: linear;`
transition-delay	| 定义过渡效果何时开始。如： `transition-delay: 2s;` 在过渡效果开始前等待 2 秒：




### transition-timing-function 速度曲线属性说明


####  定义和用法
transition-timing-function 属性规定过渡效果的速度曲线。

该属性允许过渡效果随着时间来改变其速度。

值| 描述
---|----
默认值：| ease | 
JavaScript 语法：| `object.style.transitionTimingFunction="linear"`



语法

```
transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-
bezier(n,n,n,n);
```


值	| 描述
---| ----
linear	|规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。
ease	|规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。
ease-in	|规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。
ease-out	|规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。
ease-in-out	|规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。
cubic-bezier(n,n,n,n)	|在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。

>提示：请在实例中测试不同的值，这样可以更好地理解它们的工作原理。


demo

```
<!DOCTYPE html>
<html>
<head>
<style> 
div
{
width:100px;
height:50px;
background:red;
color:white;
font-weight:bold;
transition:width 2s;
-moz-transition:width 2s; /* Firefox 4 */
-webkit-transition:width 2s; /* Safari and Chrome */
-o-transition:width 2s; /* Opera */
}

#div1 {transition-timing-function: linear;}
#div2 {transition-timing-function: ease;}
#div3 {transition-timing-function: ease-in;}
#div4 {transition-timing-function: ease-out;}
#div5 {transition-timing-function: ease-in-out;}

/* Firefox 4: */
#div1 {-moz-transition-timing-function: linear;}
#div2 {-moz-transition-timing-function: ease;}
#div3 {-moz-transition-timing-function: ease-in;}
#div4 {-moz-transition-timing-function: ease-out;}
#div5 {-moz-transition-timing-function: ease-in-out;}

/* Safari and Chrome: */
#div1 {-webkit-transition-timing-function: linear;}
#div2 {-webkit-transition-timing-function: ease;}
#div3 {-webkit-transition-timing-function: ease-in;}
#div4 {-webkit-transition-timing-function: ease-out;}
#div5 {-webkit-transition-timing-function: ease-in-out;}

/* Opera: */
#div1 {-o-transition-timing-function: linear;}
#div2 {-o-transition-timing-function: ease;}
#div3 {-o-transition-timing-function: ease-in;}
#div4 {-o-transition-timing-function: ease-out;}
#div5 {-o-transition-timing-function: ease-in-out;}

div:hover
{
width:300px;
}
</style>
</head>
<body>

<div id="div1" style="top:100px">linear</div>
<div id="div2" style="top:150px">ease</div>
<div id="div3" style="top:200px">ease-in</div>
<div id="div4" style="top:250px">ease-out</div>
<div id="div5" style="top:300px">ease-in-out</div>

<p>请把鼠标指针移动到红色的 div 元素上，就可以看到过渡效果。</p>

<p><b>注释：</b>本例在 Internet Explorer 中无效。</p>

</body>
</html>

```

demo2

```
<!DOCTYPE html>
<html>
<head>
<style> 
div
{
width:100px;
height:50px;
background:red;
color:white;
font-weight:bold;
transition:width 2s;
-moz-transition:width 2s; /* Firefox 4 */
-webkit-transition:width 2s; /* Safari and Chrome */
-o-transition:width 2s; /* Opera */
}

#div1 {transition-timing-function: cubic-bezier(0,0,0.25,1);}
#div2 {transition-timing-function: cubic-bezier(0.25,0.1,0.25,1);}
#div3 {transition-timing-function: cubic-bezier(0.42,0,1,1);}
#div4 {transition-timing-function: cubic-bezier(0,0,0.58,1);}
#div5 {transition-timing-function: cubic-bezier(0.42,0,0.58,1);}

/* Firefox 4: */
#div1 {-moz-transition-timing-function: cubic-bezier(0,0,0.25,1);}
#div2 {-moz-transition-timing-function: cubic-bezier(0.25,0.1,0.25,1);}
#div3 {-moz-transition-timing-function: cubic-bezier(0.42,0,1,1);}
#div4 {-moz-transition-timing-function: cubic-bezier(0,0,0.58,1);}
#div5 {-moz-transition-timing-function: cubic-bezier(0.42,0,0.58,1);}

/* Safari and Chrome: */
#div1 {-webkit-transition-timing-function: cubic-bezier(0,0,0.25,1);}
#div2 {-webkit-transition-timing-function: cubic-bezier(0.25,0.1,0.25,1);}
#div3 {-webkit-transition-timing-function: cubic-bezier(0.42,0,1,1);}
#div4 {-webkit-transition-timing-function: cubic-bezier(0,0,0.58,1);}
#div5 {-webkit-transition-timing-function: cubic-bezier(0.42,0,0.58,1);}

/* Opera: */
#div1 {-o-transition-timing-function: cubic-bezier(0,0,0.25,1);}
#div2 {-o-transition-timing-function: cubic-bezier(0.25,0.1,0.25,1);}
#div3 {-o-transition-timing-function: cubic-bezier(0.42,0,1,1);}
#div4 {-o-transition-timing-function: cubic-bezier(0,0,0.58,1);}
#div5 {-o-transition-timing-function: cubic-bezier(0.42,0,0.58,1);}

div:hover
{
width:300px;
}
</style>
</head>
<body>

<div id="div1" style="top:100px">linear</div>
<div id="div2" style="top:150px">ease</div>
<div id="div3" style="top:200px">ease-in</div>
<div id="div4" style="top:250px">ease-out</div>
<div id="div5" style="top:300px">ease-in-out</div>

<p>请把鼠标指针移动到红色的 div 元素上，就可以看到过渡效果。</p>

<p><b>注释：</b>本例在 Internet Explorer 中无效。</p>

</body>
</html>

```


### 多个属性写法

```
 a {
    -moz-transition: background 0.5s ease-in,color 0.3s ease-out;
    -webkit-transition: background 0.5s ease-in,color 0.3s ease-out;
    -o-transition: background 0.5s ease-in,color 0.3s ease-out;
    transition: background 0.5s ease-in,color 0.3s ease-out;
  }


原文: http://www.w3cplus.com/content/css3-transition
```

### 所有属性写法

```
  a {
    -moz-transition: all 0.5s ease-in;
    -webkit-transition: all 0.5s ease-in;
    -o-transition: all 0.5s ease-in;
    transition: all 0.5s ease-in;
  }
原文: http://www.w3cplus.com/content/css3-transition © w3cplus.com
```




> 要加前缀
>






# animation @keyframes

### CSS3 `@keyframes` 规则

如需在 `CSS3` 中创建动画，您需要学习 `@keyframes` 规则。

`@keyframes` 规则用于创建动画。在 `@keyframes` 中规定某项 `CSS` 样式，就能创建由当前样式逐渐改为新样式的动画效果。


>目前浏览器都不支持 `@keyframes` 规则。
>
>Firefox 支持替代的 `@-moz-keyframes` 规则。
>
>Opera 支持替代的 `@-o-keyframes` 规则。
>
>Safari 和 Chrome 支持替代的 `@-webkit-keyframes` 规则。


### 定义和用法
通过 `@keyframes` 规则，您能够创建动画。

创建动画的原理是，将一套 `CSS` 样式逐渐变化为另一套样式。

在动画过程中，您能够多次改变这套 `CSS` 样式。

以百分比来规定改变发生的时间，或者通过关键词 `from` 和 `to`，等价于 `0%` 和 `100%`。

`0%` 是动画的开始时间，`100%` 动画的结束时间。

为了获得最佳的浏览器支持，您应该始终定义 `0%` 和 `100%` 选择器。

>注释：请使用动画属性来控制动画的外观，同时将动画与选择器绑定。


### 语法

```
@keyframes animationname {keyframes-selector {css-styles;}}
```

值 | 描述
---|----
animationname | 必需。定义动画的名称。
keyframes-selector | 必需。动画时长的百分比。 <br> 合法的值：<br>  0-100% <br> from（与 0% 相同） <br> to（与 100% 相同）
css-styles | 必需。一个或多个合法的 CSS 样式属性。




当您在 `@keyframes` 中创建动画时，请把它捆绑到某个选择器，否则不会产生动画效果。

通过规定至少以下两项 `CSS3` 动画属性，即可将动画绑定到选择器：

* 规定动画的名称

* 规定动画的时长


### animation 

>Internet Explorer 10、Firefox 以及 Opera 支持 animation 属性。
>
>Safari 和 Chrome 支持替代的 -webkit-animation 属性。
>
>注释：Internet Explorer 9 以及更早的版本不支持 animation 属性。
>


实例

```
div{
	animation:mymove 5s infinite;
	-webkit-animation:mymove 5s infinite; /* Safari 和 Chrome */
}
```


定义和用法

animation 属性是一个简写属性，用于设置六个动画属性：

* animation-name
* animation-duration
* animation-timing-function
* animation-delay
* animation-iteration-count
* animation-direction

> 注释：请始终规定 `animation-duration` 属性，否则时长为 `0`，就不会播放动画了。


值| 内容
----|-----
默认值： |	none 0 ease 0 1 normal
继承性： |	no
版本：|	CSS3
JavaScript |语法：	object.style.animation="mymove 5s infinite"



语法：

```
animation: name duration timing-function delay iteration-count direction;
```

值| 内容
----|-----
animation-name	| 规定需要绑定到选择器的 keyframe 名称。。
animation-duration	| 规定完成动画所花费的时间，以秒或毫秒计。
animation-timing-function| 	规定动画的速度曲线。
animation-delay	| 规定在动画开始之前的延迟。
animation-iteration-count	| 规定动画应该播放的次数。
animation-direction	| 规定是否应该轮流反向播放动画。




demo

```
<!--
	author：xuess
	email：wuniu2010@126.com
	date：2017-08-11
-->
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>小球运动</title>
	</head>

	<body>

		<style type="text/css">
			#ball {
				background: #F0A8BD;
				height: 100px;
				width: 100px;
				position: absolute;
				top: 10px;
				left: 20px;
				border-radius: 50%;
				animation: bounce 2s infinite;
			}
			
			#ball1 {
				background: #00008B;
				height: 100px;
				width: 100px;
				position: absolute;
				top: 10px;
				left: 200px;
				border-radius: 50%;
				animation: bounce1 2s infinite;
			}
			
			#ball2 {
				background: #008000;
				height: 100px;
				width: 100px;
				position: absolute;
				top: 10px;
				left: 400px;
				border-radius: 50%;
				animation: bounce2 2s infinite;
			}
			
			@-webkit-keyframes bounce2 {
				0 % {
					transform: translate3d(0, 20px, 0);
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					transform: translate3d(0, 300px, 0);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					transform: translate3d(0, 20px, 0);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
			
			@keyframes bounce2 {
				0 % {
					transform: translate3d(0, 20px, 0);
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					transform: translate3d(0, 300px, 0);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					transform: translate3d(0, 20px, 0);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
			
			@-webkit-keyframes bounce1 {
				0 % {
					transform: translate(0, 20px);
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					transform: translate(0, 300px);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					transform: translate(0, 20px);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
			
			@keyframes bounce1 {
				0 % {
					transform: translate(0, 20px);
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					transform: translate(0, 300px);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					transform: translate(0, 20px);
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
			
			@-webkit-keyframes bounce {
				0 % {
					top: 20px;
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					top: 300px;
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					top: 20px;
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
			
			@keyframes bounce {
				0 % {
					top: 20px;
					animation-timing-function: cubic-bezier(1, 0, 0.96, 0.91);
				}
				50% {
					top: 300px;
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
				100% {
					top: 20px;
					animation-timing-function: cubic-bezier(0, 0.27, 0.32, 1);
				}
			}
		</style>

		<div id="ball"></div>
		<div id="ball1"></div>
		<div id="ball2"></div>
	</body>

</html>
```



> 转载请注明出处
>
> 作者：xuess<wuniu2010@126.com>
> 
> 时间：2017年08月11日
> 
> 最后修改时间：2017年08月11日