---
title: html5视频方案
date: 2017-08-08 15:06:43
tags: html5
---



## 方案1：播放本地视频源（`.mp4`文件）



> 说明：
> 
> 由于 `h5` 的`video`标签，使用的是浏览器内置的播放器，播放器的样式与平台的不同各式各样。为了统一视频展现形式，建议放一个带播放按钮样式的视频的截图，然后把图片盖在视频区域上，点击图片，通过js调用的形式，视频播放。
> 
> 把实例代码拷贝到`.html`文件中运行，最好在手机上查看效果。pc上播放基本相同，手机上播放不大相同。


这个是带播放按钮的视频截图


![image](http://img.haihu.com/0116110390a5c6ef42f14e6fac5fdf75fef3d2e0.png)

实例代码

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>H5 视频播放 mp4</title>
		<script src="https://g.alicdn.com/mtb/??lib-flexible/0.3.2/flexible_css.js,lib-flexible/0.3.2/flexible.js"></script>
	</head>

	<body>
		<style type="text/css">
			* {
				padding: 0;
				margin: 0;
			}
			html,
			body {
				width: 100%;
			}
			body {
				width: 10rem;
				margin: 0 auto;
			}
			div,
			p,
			img {
				box-sizing: content-box;
			}
			
			.content {
				width: 10rem;
			}
			.mp4 {
				height: 5.78125rem;
				width: 100%;
			}
			
			video {
				position: absolute;
				z-index: 1;
				width: 100%;
				height: 5.78125rem;
			}
			
			.video-play {
				z-index: 2;
				position: absolute;
				width: 100%;
				height: 5.78125rem;
			}
			
			p {
				text-align: center;
			}
		</style>

		<div class="content">
			<p>这里是内容</p>
			<p>这里是内容</p>
			<p>这里是内容</p>
			<div class="mp4">
				<video id="szjVideo" preload="auto" x-webkit-airplay="true" webkit-playsinline="true" controls="controls">
					<source type="video/mp4" src="http://img.haihu.com/01161102914fb7f5262d4f898ad3a6cfc1db35cc.mp4">
				</video>
				<img class="video-play" id="play" src="http://img.haihu.com/0116110390a5c6ef42f14e6fac5fdf75fef3d2e0.png" />
			</div>
			<p>这里是内容</p>
			<p>这里是内容</p>
			<p>这里是内容</p>
		</div>

		<script type="text/javascript">
			//图片播放按钮
			var videoImgPlay = document.getElementById("play");
			//点击图片播放
			videoImgPlay.onclick = function() {
				var videoPlay = true;
				var video = document.getElementById("szjVideo");
				if(videoPlay) {
					videoPlay = false;
					videoImgPlay.style.display = 'none';
					video.play();
				} else {
					videoImgPlay.style.display = 'block';
					videoPlay = true;
					video.pause();
				}
				//监听视频播放完成
				video.addEventListener('ended', function() {
					console.log("end");
					videoPlay = true;
					videoImgPlay.style.display = 'block';
				});
			}
		</script>
	</body>

</html>
```

运行效果：

![image](https://img.alicdn.com/imgextra/i3/2296013456/TB2JbF3aC.EF1JjSZPcXXaxaFXa_!!2296013456.jpg)






## 方案2：播放外部视频资源（优酷为例）

外部链接视频 以`优酷`为例，去优酷播放页面找到下面`iframe`链接，放到html指定区域中


![image](https://img.alicdn.com/imgextra/i3/2296013456/TB29c5xap6.F1JjSZFpXXcZjXXa_!!2296013456.png)


> 由于是外部资源，而且是`iframe`导入的页面，所以`js`、`css`没办法对其做操作，他们的样式、交互、展示内容**没办法控制**。

实例代码：

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>H5外链 优酷视频 播放</title>
		<script src="https://g.alicdn.com/mtb/??lib-flexible/0.3.2/flexible_css.js,lib-flexible/0.3.2/flexible.js"></script>
	</head>

	<body>
		<style type="text/css">
			* {
				padding: 0;
				margin: 0;
			}
			html,
			body {
				width: 100%;
			}
			
			body {
				width: 10rem;
				margin: 0 auto;
			}
			p {
				text-align: center;
			}
		</style>

		<p>
			这里是内容
		</p>
		<p>
			这里是内容
		</p>
		<iframe style="height: 6.25rem;width: 10rem;" src='http://player.youku.com/embed/XMjk1MDIzMTY0OA==' frameborder=0 'allowfullscreen'></iframe>
		<p>
			这里是内容
		</p>
		<p>
			这里是内容
		</p>
		<p>
			这里是内容
		</p>
		<p>
			这里是内容
		</p>
	</body>

</html>

```

运行效果：

![image](https://img.alicdn.com/imgextra/i3/2296013456/TB2elmDaxr9F1JjSZPfXXawbFXa_!!2296013456.jpg)
