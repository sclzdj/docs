# laravel下用pjax

## 安装组件

```
composer require spatie/laravel-pjax
```

## 注册中间件

app/Http/Kernel.php文件中间件middleware注册  

```
\Spatie\Pjax\Middleware\FilterIfPjax::class,
```

## 视图引入源码包

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.pjax/2.0.1/jquery.pjax.min.js"></script>
```

## 页面代码示例

### html代码

```
<div id="pjax-container">
    <!--pjax加载动画-->
    <div id="loading">
        <div class="spinner">
            <div class="rect1"></div>
            <div class="rect2"></div>
            <div class="rect3"></div>
            <div class="rect4"></div>
            <div class="rect5"></div>
        </div>
    </div>
    <!--pjax加载动画结束-->
    <div id="app">
        @yield('content')
    </div>
</div>
```


### js代码

```
<script>
    if ($.support.pjax) {//浏览器是否支持pjax
        $(document).on('click', '[pjax] a,a[pjax]', function(event) {//点击
            $.pjax.click(event, {container: '#pjax-container'});//定义加载区域
        });
        $(document).on('submit', 'form[pjax]', function(event) {//提交
            $.pjax.submit(event, '#pjax-container');//定义加载区域
        });
        //定义pjax有效时间，超过这个时间会整页刷新
        $.pjax.defaults.timeout = 1200;
        //显示加载动画
        $(document).on('pjax:click', function() {
            $("#loading").show();
        });
        //隐藏加载动画
        $(document).on('pjax:end', function() {
            $("#loading").hide();
        });
    }
</script>
```

### css代码

```
<style>
    #loading {
        background-color: rgba(238, 238, 238, 0.6);
        display: none;
        position: absolute;
        left: 0;
        top: 0;
        right: 0;
        z-index: 2000;
        bottom: 0;
        padding-top: 10%;
    }
    #loading .spinner {
        margin: 100px auto;
        width: 50px;
        height: 60px;
        text-align: center;
        font-size: 10px;
    }
    #loading .spinner > div {
        background-color: rgba(0, 0, 0, 0.2);
        height: 100%;
        width: 6px;
        display: inline-block;
        -webkit-animation: stretchdelay 1.2s infinite ease-in-out;
        animation: stretchdelay 1.2s infinite ease-in-out;
    }
    #loading .spinner .rect2 {
        -webkit-animation-delay: -1.1s;
        animation-delay: -1.1s;
    }
    #loading .spinner .rect3 {
        -webkit-animation-delay: -1s;
        animation-delay: -1s;
    }
    #loading .spinner .rect4 {
        -webkit-animation-delay: -0.9s;
        animation-delay: -0.9s;
    }
    #loading .spinner .rect5 {
        -webkit-animation-delay: -0.8s;
        animation-delay: -0.8s;
    }
    @-webkit-keyframes stretchdelay {
        0%,
        40%,
        100% {
            -webkit-transform: scaleY(0.4);
        }
        20% {
            -webkit-transform: scaleY(1);
        }
    }
    @keyframes stretchdelay {
        0%,
        40%,
        100% {
            transform: scaleY(0.4);
            -webkit-transform: scaleY(0.4);
        }
        20% {
            transform: scaleY(1);
            -webkit-transform: scaleY(1);
        }
    }
</style>
```

