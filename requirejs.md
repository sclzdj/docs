# RequireJS

只演示我自己常用的目录结构，其它代码都是以此结构为基准。可更换结构，但对应目录地址也要相应改动。

## 目录结构

> css[d]
>
> >libs[d]#第三方css包
> >
> >> bootstrap.min.css[f]
> >>
> >> font-awesome.min.css[f]
> >
> >custom.css[f]#其它自写css
> >
> >app.css[f]#其它自写css
> >
> >......
>
> js[d]
>
> > libs[d]#第三方js包
> >
> > > require.min.js[f]#必须
> > >
> > > css.min.js[f]#必须
> > >
> > > lodash.min.js[f]
> > >
> > > jquery.min.js[f]
> > >
> > > bootstrap.min.js[f]
> > >
> > > vue.min.js[f]
> >
> > main.js[f]#必须，入口文件
> >
> > util.js[f]#自定义工具库，可定义多个
> >
> > objs.js[f]#自定义多函数库（很少用）
> >
> > obj.js[f]#自定义单函数（很少用）
> >
> > custom.js[f]#其它自写js
> >
> > app.js[f]#其它自写js
> >
> > ......
>
> imgs[d]
>
> > ......
>
> index.php[f]
>
> ......

## 引入

index.php文件html代码的头部载入示例

```
<!--requirejs包-->
<script src="./js/libs/require.min.js"></script>
<!--入口文件-->
<script src="./js/main.js"></script>
```

## 入口文件

```
require.config({
    baseUrl: './js/',
    paths: {
        'css': 'libs/css.min',
        'lodash': 'libs/lodash.min',
        'jquery': 'libs/jquery.min',
        'bootstrap': 'libs/bootstrap.min',
        'vue': 'libs/vue.min',
        'util': 'util',#自定义工具库
        'objs': 'objs',#单函数库，很少用
        'obj': 'obj'#单函数库，很少用
    },
    shim: {
        'bootstrap': {#依赖关系
            'deps': ['jquery', 'css!../css/libs/bootstrap.min.css', 'css!../css/libs/font-awesome.min.css']//依赖项
        },
        'obj': {//单函数库，很少用
            'exports': 'msg'
        },
        'objs': {//多函数库，很少用
            'init': function () {
                return {
                    success: success,
                    error: error
                };
            }
        }
    }
});
```

## 使用方法

### 类库

index.php文件html代码的script标签中使用示例

```
require(['jquery','lodash'],function($,_){
	$('body').css('backgroundColor','red');
	alert(_.join(['a', 'b', 'c'], '~'));
});
```

### 工具类

js文件代码（util.js）示例

```
define(['lodash'],function(_){
    return {
        change:function(){
            require(['jquery'],function ($) {
                $('body').css('backgroundColor','red');
            });
        },
        join:function () {
            alert(_.join(['1', '2', '3'], '-'));
        },
        chunk:function () {
            alert(_.chunk(['a', 'b', 'c', 'd'], 2));
        }
    };
});
```

main.js入口文件示例

```
//...
	paths: {
		//...
        'util': 'util'
        //...
    },
//...
```

index.php文件html代码的script标签中使用示例

```
require(['util'],function(util){
	util.change();
	util.join();
	util.chunk();
});
```

### 多函数库

js文件代码（objs.js）示例

```
function success(){
    alert('success');
}
function error(){
    alert('error');
}
```

main.js入口文件示例

```
//...
	paths: {
		//...
        'objs': 'objs'
        //...
    },
//...
//...
	shim: {
        'objs': {
            'init': function () {
                return {
                    success: success,
                    error: error
                };
            }
        }
    }
//...
```

index.php文件html代码的script标签中使用示例

```
require(['objs'],function(objs){
	objs.success();
	objs.error();
});
```

### 单函数

js文件代码（objs.js）示例

```
function msg(){
    alert('msg');
}
```

main.js入口文件示例

```
//...
	paths: {
		//...
        'obj': 'obj'
        //...
    },
//...
//...
	shim: {
        'obj': {
            'exports': 'msg'
        }
    }
//...
```

index.php文件html代码的script标签中使用示例

```
require(['obj'],function(obj){
	obj();
});
```

### 