# 百度地图

## 获取秘钥ak

>登录百度地图开放平台http://lbsyun.baidu.com/

>创建应用，设置好白名单

>应用设置好后即可得到密钥ak

## JavaScript API v3.0

> http://lbsyun.baidu.com/index.php?title=jspopular3.0

### 简单使用

```
<div id="map" style="width:100%;height:100%;overflow: hidden;"></div>
<script type="text/javascript" src="http://api.map.baidu.com/api?v=3.0&ak=XF3VPRfZMqvfd3ztUVL9vliGvOYPrPFD"></script>
<script>
    //百度地图API功能
    var map = new BMap.Map('map');//创建api应用
    //初始化地图,
    /*//第一种方法,设置中心店坐标和地图缩放级别
    //如果使用点坐标来定义中心坐标,那么第二个参数缩放级别必须填写
    map.centerAndZoom(new BMap.Point(121.4,31.2),10);*/
    //第二种方法,可以使用城市名称来定义中心点
    //如果使用城市名称来定义中心坐标,那么第二个参数可以不填写.不填写,则会默认缩放到适应的一个级别
    map.centerAndZoom('南充');
    map.addControl(new BMap.MapTypeControl()); //添加地图类型控件
    //map.setCurrentCity('南充'); //设置地图显示的城市
    map.enableScrollWheelZoom(true); //开始鼠标滚轮缩放,默认是关闭
</script>
```

### 异步使用

```
<div id="map" style="width:100%;height:100%;overflow: hidden;"></div>
<script type="text/javascript" src="http://api.map.baidu.com/api?v=3.0&ak=XF3VPRfZMqvfd3ztUVL9vliGvOYPrPFD"></script>
<script>
    function initialize() {
        //百度地图API功能
        var map = new BMap.Map('map');//创建api应用
        //初始化地图
        //第一种方法,设置中心店坐标和地图缩放级别
        //如果使用点坐标来定义中心坐标,那么第二个参数缩放级别必须填写
        map.centerAndZoom(new BMap.Point(121.4,31.2),10);
        /*//第二种方法,可以使用城市名称来定义中心点
        //如果使用城市名称来定义中心坐标,那么第二个参数可以不填写.不填写,则会默认缩放到适应的一个级别
        map.centerAndZoom('南充');*/
        map.addControl(new BMap.MapTypeControl()); //添加地图类型控件
        //map.setCurrentCity('南充'); //设置地图显示的城市
        map.enableScrollWheelZoom(true); //开始鼠标滚轮缩放,默认是关闭
    }
    function loadScript() {
        var script = document.createElement("script");
        script.src = "http://api.map.baidu.com/api?v=3.0&ak=XF3VPRfZMqvfd3ztUVL9vliGvOYPrPFD&callback=initialize";
        document.body.appendChild(script);
        initialize();
    }
    window.onload = loadScript;
</script>
```

### 其它常用功能

#### 添加控件

> addControl();

#### 删除控件

> removeControl();

#### 添加覆盖物

> addOverlay();

#### 删除覆盖物

> removeOverlay();

#### 地图跳转

> panTo()

```
<script>
    //百度地图API功能
    var map = new BMap.Map('app'); //创建api应用
    map.centerAndZoom(new BMap.Point(116.5,40,0),14); //初始化地图,设置中心店坐标和地图缩放级别
    map.addControl(new BMap.MapTypeControl()); //添加地图类型控件
    //5s之后跳转至上海校区地图
    setTimeout(function () {
    	map.panTo(new BMap.Point(121.4,31.2),16);//跳转
    },5000);
    map.setCurrentCity('上海'); //设置地图显示的城市
    map.enableScrollWheelZoom(true); //开始鼠标滚轮缩放,默认是关闭
</script>
```

#### 设置缩放等级

> setZoom()

```
//5s之后修改缩放级别(将缩放级别放大)
setTimeout(function () {
	map.setZoom(6);
},5000);
```

#### 地图拖拽

> disableDragging()  关闭拖拽功能
>
> enableDragging()   开启拖拽功能

```
//默认是开启拖拽的
map.disableDragging();//关闭拖拽功能
setTimeout(function () {
	//alert('可以拖拽了');
	map.enableDragging();//开启拖拽功能
},10000);
```

#### 计算两点距离

> getDistance()

```
//随便设定2个坐标点
var pointA = new BMap.Point(121.6,31.2);
var pointB = new BMap.Point(121.8,31.5);
var juli = map.getDistance(pointA,pointB);
alert(juli);
```

#### 两点间绘线覆盖物

> http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_reference_3_0.html#a3b11

```
//随便设定2个坐标点
var pointA = new BMap.Point(121.6,31.2);
var pointB = new BMap.Point(121.8,31.5);
var polyLine = new BMap.Polyline([pointA,pointB],{strokeColor:'red',strokeWeight:10,strokeOpacity:0.5});
map.addOverlay(polyLine);
```

#### 比例尺控件

>http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_reference_3_0.html#a2b9

```
var scaleController=new BMap.ScaleControl({anchor:BMAP_ANCHOR_TOP_LEFT});
map.addControl(scaleController);
//map.removeControl(scaleController);
```

#### 工具条控件

>http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_reference_3_0.html#a2b2

```
var navigationControl=new BMap.NavigationControl({anchor:BMAP_ANCHOR_TOP_RIGHT,type:BMAP_NAVIGATION_CONTROL_SMALL});
map.addControl(navigationControl);
//map.removeControl(navigationControl);
```

#### 点标注覆盖物

> http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_reference_3_0.html#a3b2

##### 默认图标

```
//百度地图API功能
var map = new BMap.Map('map'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
//创建一个标注
var marker = new BMap.Marker(point);
//将标注添加到地图中
map.addOverlay(marker);
//设置动画
marker.setAnimation(BMAP_ANIMATION_DROP);
```

##### 自定义图标

```
//百度地图API功能
var map = new BMap.Map('map'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
//自定义一个icon，第二个参数是设置文本偏移量
var myIcon = new BMap.Icon('icon-name.gif',new BMap.Size(300,150));
//创建一个标注
var marker = new BMap.Marker(point,{icon:myIcon});
//将标注添加到地图
map.addOverlay(marker);
//设置动画
marker.setAnimation(BMAP_ANIMATION_DROP);
```

#### 文本标注覆盖物

```
//百度地图API功能
var map = new BMap.Map('map'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
//文本标注参数
var options = {
	position:point, //制定文本标注所在位置
	offset:new BMap.Size(-70,-100), //设置文本偏移量
};
//创建文本标注的对象
var label = new BMap.Label('欢迎你使用百度地图',options);
//设置文本标注样式
label.setStyle({
    color:'red', //字体颜色
    fontSize:'14px', //字体大小
    height:'40px',
    lineHeight:'40px',
    padding:'0 10px'
});
//将文本标注添加到地图中
map.addOverlay(label);
// var marker = new BMap.Marker(point);//创建一个标注点
// map.addOverlay(marker);//将标注点添加到地图中
// marker.setAnimation(BMAP_ANIMATION_BOUNCE);//标注点动画效果
```

#### html窗口信息对象

> http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_reference_3_0.html#a3b7

```
//百度地图API功能
var map = new BMap.Map('app'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
//窗口参数
var options = {width:300, height:300};
//窗口内容
var content = '<h2>www.百度.com</h2><img src="./img-name.gif" style="width: 50px;height: 50px;" />';
//创建窗口信息对象
var infoWindow = new BMap.InfoWindow(content,options);
//默认打开窗口信息对象
map.openInfoWindow(infoWindow,point);
```

### 常用事件

#### 点击

##### 点击地图为例

```
//百度地图API功能
var map = new BMap.Map('app'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
function showInfo(e){
	alert(e.point.lng + ',' + e.point.lat);//弹出点击点的经纬度
}
map.addEventListener('click',showInfo);
```

##### 点击标注点为例

```
//百度地图API功能
var map = new BMap.Map('app'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,15); //初始化地图,设置中心店坐标和地图缩放级别
//创建标注
var marker = new BMap.Marker(point);
map.addOverlay(marker);
marker.setAnimation(BMAP_ANIMATION_BOUNCE);
//创建窗口信息对象
var infoWindow = new BMap.InfoWindow('欢迎使用');
//给标注点添加点击事件
marker.addEventListener('click',function () {
	map.openInfoWindow(infoWindow,point);//打开窗口信息对象
});
```

### 示例demo

#### 全部demo

> http://lbsyun.baidu.com/jsdemo.htm

#### 常用demo

##### 带检索功能的信息窗口

> http://lbsyun.baidu.com/jsdemo.htm#d0_4

##### 多标注点的信息窗口

> http://lbsyun.baidu.com/jsdemo.htm#d0_5

### 地理解析

> http://lbsyun.baidu.com/jsdemo.htm#i7_1

```
//比如现在我只知道一个具体的地址,我想解析出来当前地址的坐标,并且标注在地图上
//创建一个地址解析器
var geo = new BMap.Geocoder();
geo.getPoint('上海市松江区江田东路185号',function (point) {
    if(point){//如果存在这个点
        //point相当于new BMap.Point(121.5,31.1)这个的结果
        
    }
});
```

### 逆地理解析

> http://lbsyun.baidu.com/jsdemo.htm#i7_2

```
//创建一个地址解析器
var geo = new BMap.Geocoder();
//创建一个点
var point = new BMap.Point(121.5,31.1);
//解析改点的地理信息
geo.getLocation(point,function (rs) {
	console.log(rs);
});
```

### 批量地理解析

> http://lbsyun.baidu.com/jsdemo.htm#i7_3
>
> http://lbsyun.baidu.com/jsdemo.htm#i7_4

下面是个经典的示例

```
//百度地图API功能
var map = new BMap.Map('app'); //创建api应用
var point = new BMap.Point(121.4,31.2);
map.centerAndZoom(point,10); //初始化地图,设置中心店坐标和地图缩放级别
//开启鼠标滚轮缩放
map.enableScrollWheelZoom(true);
//要解析的地址
var adds = [
    '上海市虹桥火车站',
    '上海浦东国际机场',
    '上海市松江区江田东路185号'
];
var geo = new BMap.Geocoder();
var index = 0;
function jiexi() {
    var add = adds[index];
    geosearch(add);
    index++;
}
function geosearch(add) {
    if(index < adds.length){
        setTimeout(window.jiexi,500);
    }
    geo.getPoint(add,function (point) {
        if(point){
            //下面以标注点示例
            var address = new BMap.Point(point.lng,point.lat);
            var marker = new BMap.Marker(address);
            map.addOverlay(marker);
        }
    });
}
```

### 坐标转换

> http://lbsyun.baidu.com/jsdemo.htm#a5_1

# 高德地图

## 获取秘钥key

> 登录高德地图开放平台https://lbs.amap.com/

> 创建应用，设置好白名单

> 应用设置好后即可得到密钥key

