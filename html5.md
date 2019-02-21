# HTML5

## WEB语义化标签

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<header>头部标签</header>
		<nav>导航标签</nav>
		<div id="main">
			<article><!--文章标签-->
				<h1>文章标题</h1>
				<time datetime="2019-3-5 15:3:8">2019-3-5 15:3:8</time>
				<p>
					<mark>记号标签，起强调作用</mark>
					<ins>下划线标签</ins>
					<del>删除线标签</del>
				</p>
				<!--文章章节-->
				<section>
					<h3>章节1</h3>
					<p>章节内容1</p>
				</section>
				<section>
					<h3>章节2</h3>
					<p>章节内容2</p>
				</section>
				<!--文章章节结束-->
			</article>
			<aside><!--侧边栏标签-->
				<code>代码标签</code>
				<progress min='0' max='100' value="33">进度条标签，兼容性还不行</progress>
			</aside>
		</div>
		<footer><!--尾部标签-->
			<address>地址标签</address>
		</footer>
	</body>
</html>

```

## 表单新特性

> 邮箱：`<input type="email">`

> 网址：`<input type="url">`

> 数值：`<input type="number" min="0" max="20" step="2">`

> 数值范围滑块：`<input type="range" min="0" max="20">`；用onchange事件获取值

> 日期：`<input type="date">`； IE浏览器不支持，兼容性不好，一般用第三方的插件

> 颜色：`<input type="color">`；用onchange事件获取值（颜色值为十六进制，例：#F00000）

> 下拉选项分组：
>
> ```
> <select>
> 	<optgroup label='四川'>
> 		<option value='成都'>成都</option>
> 		<option value='南充'>南充</option>
> 	</optgroup>
> 	<optgroup label='广东'>
> 		<option value='广州'>广州</option>
> 		<option value='中山'>中山</option>
> 	</optgroup>
> </select>
> ```
>
> 单行文本候选列表：
>
> ```
> <input type="text" list="l" />		
> <datalist id="l">
>     <option value="beijing">北京</option>
>     <option value="nanjin">南京</option>
>     <option value="dongjing">东京</option>
>     <option value="shanghai">上海</option>
>     <option value="shangrao">上饶</option>
>     <option value="jinan">济南</option>
>     <option value="longnan">陇南</option>
> </datalist>
> 
> ```
>
> 自动聚焦：`<input type="text" autofocus>`
>
> 表单外的表单项也提交：
>
> ```
> <form id="f"></form>
> <input type="text" autofocus form="f">
> ```
>
> 多文件上传：`<input type="file" multiple>`
>
> 默认值：`<input type="text" placeholder="请输入关键词">`
>
> 提交时正则验证：`<input type="text" pattern="正则表达式，例：\d+">`
>
> 不能为空：`<input type="text" required>`

## 多媒体标签

#### 音乐

```
<audio src="music.mp3" controls autoplay loop></audio>
```

> controls：显示控制台

>autoplay：自动播放

>loop：反复播放；也可指定播放次数，不加次属性默认一次

#### 视频

video标签支持mp4、ogg、webm格式的视频文件；尽量用mp4格式

```
<video src="big_buck_bunny_720p_stereo.ogg" width="600" controls loop poster='big_buck_bunny_720p_stereo.jpg'></video>
```

> controls：显示控制台

> autoplay：自动播放

> loop：反复播放；也可指定播放次数，不加次属性默认一次

> poster：视频封面

#### 自定义播放器

音视频API：http://www.w3school.com.cn/html5/html5_ref_audio_video_dom.asp

```
<body>
    <input type="button" value="播放" id="play" />
    <input type="button" value="暂停" id="pause" />
    <input type="button" value="后退5秒" id="back5s" />
    <input type="button" value="快进5秒" id="go5s" />
    <input type="button" value="静音" id="muted" />
    <input type="button" value="提高音量" id="volume1" />
    <input type="button" value="减少音量" id="volume2" />
    <br />
    播放进度： 
    <div id="pro">
    <div id="ls"></div>
    </div>
    <br /><br /> 
    <video src="big_buck_bunny_720p_stereo.ogg" width="600" poster="big_buck_bunny_720p_stereo.jpg" id="v" ></video>
	<script type="text/javascript">
		var v = document.getElementById("v");
		var pro = document.getElementById("pro");
		var ls = document.getElementById("ls");	
//		播放
		document.getElementById("play").onclick = function(){
			v.play();
		}
//		暂停
		document.getElementById("pause").onclick = function(){
			v.pause();
		}
//		后退5秒
		document.getElementById("back5s").onclick = function(){
			var ctime = v.currentTime;
			v.currentTime = ctime-5;
		}
//		快进5s
		document.getElementById("go5s").onclick = function(){
//			获得当前的播放位置
			var ctime = v.currentTime;
			v.currentTime = ctime+5;
		}
//		静音
		document.getElementById("muted").onclick = function(){
//			if(v.muted){//true表示正处于静音状态
//				v.muted = false;
//			}else{
//				v.muted = true;
//			}
			v.muted = !v.muted;
		}
//		提高音量
		document.getElementById("volume1").onclick = function(){
//			获得当前音量
			var curvolume = v.volume;
			var newvolume = curvolume+0.1;
//			赋值新的音量
			v.volume = newvolume>1?1:newvolume;
		}
//		减少音量
		document.getElementById("volume2").onclick = function(){
			var curvolume = v.volume;
			var newvolume = curvolume-0.1;
			v.volume = newvolume<0?0:newvolume;
		}
//		进度条
		v.onplay = function(){
//			alert('开始播放啦！');
			setInterval(function(){
//				获得当前的播放位置
				var ctime = v.currentTime;
//				获得视频总长度
				var alltime = v.duration;
//				计算进度条长度
				var lswidth = ctime/alltime*600;
				ls.style.width = lswidth+'px';
			},500)
		}
//		点击播放位置
		pro.onclick = function(e){
			var ev = window.event || e;
//			获得鼠标的位置
			var mx = ev.layerX || offsetX;
//			计算播放位置
			var ctime = mx/600*v.duration;
//			设置播放位置
			v.currentTime = ctime;
			ls.style.width = mx+'px';
		}
	</script>
</body>
```

## 本地存储

```
//sessionStorage存储的数据在浏览器关闭后就清理掉
//sessionStorage写入数据的页面和获得数据的页面要在同一个浏览器标签中打开才可以
sessionStorage.setItem('name','名字');//设置数据
sessionStorage.getItem('name');//获取数据
sessionStorage.removeItem('name');//删掉数据	
sessionStorage.length;//获得数据数量
sessionStorage.clear();//清理所有数据

//localStorage 是永久存储
//localStorage 多标签可以共享数据
localStorage.setItem('name','名字');//设置数据
localStorage.getItem('name');//获取数据
localStorage.removeItem('name');//删掉数据	
localStorage.length;//获得数据数量
localStorage.clear();//清理所有数据

```

## canvas

getContext  • 返回一个用于在画布上绘图的环境

```
<script type="text/javascript">
	c = document.getElementById("canvas");
	obj = c.getContext('2d');
</script>
```

### 方法或属性

矩形
• context.fillRect(x,y,width,height)           绘制“被填充”的矩形
• context.strokeRect(x,y,width,height)     绘制矩形（无填充）
• context.clearRect(x,y,width,height)       在给定的矩形内清除指定的像素
颜色、样式
• context.fillStyle=‘#f00f00’         设置或返回填充绘画的颜色、渐变或模式
• context.strokeStyle=‘green’     设置或返回笔触的颜色、渐变或模式
• context.lineWidth=10          设置或返回当前的线条宽度
• context.lineJoin=“边界类型”      bevel:斜角,round:圆角,miter:尖角
canvas方法或属性
路径
• beginPath() 开始一条路径，或重置当前路径
• closePath() 创建从当前点回到起始点的路径(闭合路径）
• moveTo(x,y) 把路径移动到画布中的指定点，不创建线条
• lineTo(x,y) 添加一个新点，创建从该点到最后指定点的线条
• fill() 填充当前绘图（填充路径）
• stroke() 绘制已定义的路径（连线路径）

### 画布控制

• context.scale(scalewidth,scaleheight)      缩放处理 1=100%
• context.translate(x,y)           图形位置处理
• context.rotate(angle) 旋转画布,单位：弧度，默认以画布为圆心旋转


### 文本控制

设置字体属性
• context.font="40px Arial”
设置对齐方式
• context.textAlign=“left | right | center”
在画布上绘制“被填充的”文本
• context.fillText(text,x,y,maxWidth);
在画布上绘制文本（无填充）
• context.strokeText(text,x,y,maxWidth)
设置文字基线
• context.textBaseline=“top | middle |  bottom”;
获取文本宽度
• context.measureText(text);

### 图像控制

向画布上绘制图像、画布或视频
语法 1: 在画布上定位图像：
context.drawImage(img,画布x坐标,画面y坐标);
语法 2: 在画布上定位图像，并规定图像的宽度和高度
context.drawImage(img,画布x坐标,画面y坐标,图片width,图片height);
语法 3: 剪切图像，并在画布上定位被剪切的部分
context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height);
参数：
img 规定要使用的图像、画布或视频。
sx 可选。开始剪切的 x 坐标位置。
sy 可选。开始剪切的 y 坐标位置。
swidth可选。被剪切图像的宽度。
sheight可选。被剪切图像的高度。
x 可选。在画布上放置图像的 x 坐标位置。
y 可选。在画布上放置图像的 y 坐标位置。
width 可选。要使用的图像的宽度。（伸展或缩小图像）
height可选。要使用的图像的高度。（伸展或缩小图像）
注意:参数数量不同，x、y的函数不同

### 画圆弧

• context.arc(x,y,r,sAngle,eAngle,counterclockwise) 创建弧/曲线（用于创建
圆形或部分圆）
参数说明：
x 圆的中心的 x 坐标。
y 圆的中心的 y 坐标。
r 圆的半径。
sAngle 起始角，以弧度计。（弧的圆形的三点钟位置是 0 
度）。
eAngle 结束角，以弧度计。
counterclockwise 可选。False = 顺时针，true = 逆时针。
弧度计算公式： 角度*Math.PI/180

### 绘制线

```
<canvas id="canvas" width="300" height="300"></canvas>
<script type="text/javascript">
    c = document.getElementById("canvas");
    obj = c.getContext('2d');
    obj.lineWidth = 10;
    //线颜色
    obj.strokeStyle = "red";
    //开始绘制路径
    obj.beginPath();
    //光标移动到0,0
    obj.moveTo(0, 0);
    //绘制到300,300
    obj.lineTo(300, 300);
    //绘制定义好的路径
    obj.stroke();
</script>
```

### 绘制矩形

```
<canvas id="canvas" width="300" height="300"></canvas>
<script type="text/javascript">
    c = document.getElementById("canvas");
    //获得绘图对象
    obj = c.getContext('2d');
    //线宽
    obj.lineWidth=2;
    //颜色
    obj.strokeStyle='green';
    //绘制开始
    obj.beginPath();
    //绘制矩形
    //参数：x,y,width,height
    obj.strokeRect(50,50,100,100);
    //填充颜色 
    obj.fillStyle="red";
    //实心矩形
    obj.fillRect(220,220,100,100);
</script>
```



### 