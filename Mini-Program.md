# 微信小程序

我的账号sclzffdj@qq.com

开发文档https://developers.weixin.qq.com/miniprogram/dev/index.html?t=18110616

## 简单创建流程

1、在项目根目录创建app.js、app.json、app.wxss(非必须)
2、在app.js脚本中，用App()方法注册（声明）小程序
3、在app.json中用花括号创建一个对象
4、创建pages目录
5、在pages目录中创建首页对应的目录index
6、在index目录中创建首页对应的index.js、index.json、index.wxss、index.wxml文件
7、在index.js中用Page()函数声明当前页面
8、在index.json文件中用花括号声明一个json对象
9、在app.json文件中，添加一个pages属性，属性值是一个数组，将首页的路径写到数组中

## 尺寸rpx换算

1、将设计图等比修改一下尺寸，将宽度改为750px。这样的话就相当于是以iphone6为参照的设计图了
2、在iphone6中 1px相当于2rpx
3、如果你量出来某个元素宽度是20px，在小程序中给该元素设置尺寸的时候应该写成40rpx

## wx:if

```
<view wx:if="{{length > 5}}"> 1 </view>
<view wx:elif="{{length > 2}}"> 2 </view>
<view wx:else> 3 </view>
```

## wx:if vs hidden

因为 `wx:if` 之中的模板也可能包含数据绑定，所以当 `wx:if` 的条件值切换时，框架有一个局部渲染的过程，因为它会确保条件块在切换时销毁或重新渲染。

同时 `wx:if` 也是**惰性的**，如果在初始渲染条件为 `false`，框架什么也不做，在条件第一次变成真的时候才开始局部渲染。

相比之下，`hidden` 就简单的多，组件始终会被渲染，只是简单的控制显示与隐藏。

一般来说，`wx:if` 有更高的切换消耗而 `hidden` 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 `hidden` 更好，如果在运行时条件不大可能改变则 `wx:if` 较好。

## wx:for

### 默认

#### index -- item

### 自定义

wx:for-index    

wx:for-item

### 唯一标识符

**wx:key="id"**

> id代表的是item的id字段，也可以是其他的唯一字段，比如username，必须是唯一的

**wx:key="*this"**

> *this代表的是item本身

### block标签

> for循环渲染多个标签要用这个标签

```
<block wx:for="{{[1, 2, 3]}}">
  <view> {{index}}: </view>
  <view> {{item}} </view>
</block>
```

## 模板template

### 简单使用

wxml代码：

```
<!-- 定义模板 -->
<template name="tplName">
  <view>标题：{{title}}</view>
  <view>内容：{{content}}</view>
</template>

<!-- 使用模板 其它页面需要使用import导入模板-->
<template is="tplName" data="{{...tplData}}" /> 
```

js代码：

```
App({
    data:{
        tplData:{
            title:'这是标题',
            content:'这是内容'
        }
    }
})
```

## 导入

都是以当前文件目录为基准导入

### import

> 只导入页面的所有template模板

```
<import src="../../index.wxml" />
```

### include

> 导入页面除了template标签和wxs标签外的所有内容

```
<include src="../../index.wxml" />
```

## 事件

### bind

>会形成事件流，即会有冒泡情况

### catch

> 不会形成事件流。会阻止冒泡情况

## 模块wxs

> wxs中的模块是用来定义各种功能的，每个功能是一个单独的模块，互相独立，互不干涉
>
> 模块是完全封闭的，跟外界没有关系

### 第一种方式

全部写在wxml文件中

```
<!-- 定义dj模块 -->
<wxs module="dj">
    var title = '这是标题';
    var foo = function(){
      console.log('打印的内容！');
    }
    module.exports = {
      t:title,
      f:foo
    }
</wxs>
<!-- 调用dj模块 -->
<text>{{dj.t}}</text>
<text>{{dj.f()}}</text>

```

### 第二种方式

定义index.wxs文件

```
var title = '这是标题';
var foo = function(){
	console.log('打印的内容！');
}
var sad = function(num){
    console.log(num+1);
}
module.exports = {
	t:title,
	f:foo,
	s:sad
}
```

wxml文件调用

```
<!-- 引入模块,模块引入的时候必须用相对路径 -->
<wxs src="./index.wxs" module="dj"></wxs>
<!-- 调用dj模块 -->
<text>{{dj.h}}</text>
<text>{{dj.f()}}</text>
<text>{{dj.s(6)}}</text>
```

## wepy

开发文档

V1：https://tencent.github.io/wepy/document.html#/

V2：https://wepyjs.github.io/wepy-docs/

以下都是wepy第一版的使用方式，第二版还是内测版，并且有很大的变化，不能完全按照下面的进行开发，可以参考自研小程序“鲸活动”，第二版遗留了一些问题未解决，比如html解析报错，最好等第二版正式发行后使用。

### 简单使用

#### 创建

```
cnpm install wepy-cli -g #全局安装或更新WePY命令行工具

wepy list #查看模板列表

#创建一个standard模板（模认模板，开启了promise、Async Function等）
wepy init standard myproject 

cd myproject #进入项目目录

cnpm install #安装依赖

npm run dev #监听编译
```

#### 整洁首页

备份原页面pages/index.wpy=>index.wpy.bak

修改原页面为整洁的页面pages/index.wpy

```
<style lang="less">

</style>

<template>

</template>

<script>
  import wepy from 'wepy'

  export default class Index extends wepy.page {

  }
</script>

```

### phpstrom配置高亮

> 打开Settings，搜索Plugins，搜索Vue.js插件并安装，若已安装则跳过

>打开Settings，搜索File Types，找到Vue.js Template，在Registered Patteerns添加*.wpy即可

### 开启async await

在app.wpy文件中类的构造方法中加上下面这句即可开启

```
constructor() {
    super()
    this.use('promisify')  //加上这一句
    this.use('requestfix')
}
```

### 解析HTML

克隆下来

```
git clone https://github.com/icindy/wxParse.git 
```

把文件复制到小程序项目

```
cp -r wxParse/wxParse myproject/src  #复制到src目录下面
```

在app.wpy中引入

```
<style lang="less">
@import 'wxParse/wxParse.less';
...
```

在需要解析html的页面

```
import wepy from 'wepy'
import util from '@/service/util'
import WxParse from '@/wxParse/wxParse.js'  // 第一步：引入

export default class Article extends wepy.page {
    data = {
      article: {}
    }
    async getArticle(id) {
      let response = await util.request('category/article', 'get', {id})
      if (response !== false) {
        this.article = response
        // 第二步：开始解析html
        let that = this
        WxParse.wxParse('content', 'html', response.content, that, 5)
        // 结束解析html
        this.$apply()
      }
    }
    onLoad = (options) => {
      this.getArticle(options.id)
    }
}
```

模板用法

```
<template>
    <import src="../wxParse/wxParse.wxml"/>

    <view>
        <template is="wxParse" data="{{wxParseData:content.nodes}}"/>
    </view>
</template>
```

### 下拉刷新

开启页面下拉功能

```
config = {
	enablePullDownRefresh: true
}
```

执行下拉函数

```
// 下拉刷新
onPullDownRefresh() {
    // 显示顶部加载框
    wepy.showNavigationBarLoading()
    // 重新执行初始化函数
    this.onLoad()
    // 隐藏顶部加载框
    wepy.hideNavigationBarLoading()
    // 停止下拉动作
    wepy.stopPullDownRefresh()
}
```

### 上拉加载更多

```
<template>
    <view class="weui-cells__title">分类列表</view>
    <view class="weui-cells weui-cells_after-title">
        <repeat for="{{categorys}}" key="index" index="index" item="item">
            <navigator url="/pages/list?cid={{item.id}}" class="weui-cell weui-cell_access" hover-class="weui-cell_active">
                <view class="weui-cell__bd">{{item.name}}</view>
                <view class="weui-cell__ft weui-cell__ft_in-access"></view>
            </navigator>
        </repeat>
    </view>
    <view class="container" wx:if="{{!pageMore}}" style="color: #CCCCCC;font-size: 12px;line-height: 30px;">我是有底线的</view>
</template>

<script>
  import wepy from 'wepy'
  import util from '@/service/util'

  export default class Category extends wepy.page {
    config = {
      enablePullDownRefresh: true
    }
    data = {
      page: 1,
      pageSize: 50,
      pageMore: true,
      categorys: []
    }

    async getCategorys() {
      let response = await util.request('category/index', 'get', {page: this.page, pageSize: this.pageSize})
      if (response !== false) {
        this.categorys = response
        this.$apply()
      }
    }
    
	// 上拉加载更多
    async onReachBottom() {
      // 显示加载框
      wepy.showLoading({title: '玩命加载中'})
      // 请求更多数据
      let response = await util.request('category/index', 'get', {page: this.page + 1, pageSize: this.pageSize})
      // 隐藏加载框
      wepy.hideLoading()
      // 赋值数据
      if (response !== false) {
        if (response.length === 0) {
          this.pageMore = false
          this.$apply()
        } else {
          this.page++
          this.categorys = this.categorys.concat(response)
        }
        this.$apply()
      }
    }

    // 下拉刷新
    onPullDownRefresh() {
      // 显示顶部加载框
      wepy.showNavigationBarLoading()
      // 重新执行初始化函数
      this.page = 1
      this.pageSize = 50
      this.pageMore = true
      this.categorys = []
      this.onLoad()
      // 隐藏顶部加载框
      wepy.hideNavigationBarLoading()
      // 停止下拉动作
      wepy.stopPullDownRefresh()
    }

    onLoad = () => {
      this.getCategorys()
    }
  }
</script>
```

### 工具类

util.js

```
import wepy from 'wepy'

const host = 'http://www.sclzdj.cn/index.php/api'

const util = {
  // 消息提示框
  showToast: (title, duration = 2000, icon = 'none') => {
    wepy.showToast({title, icon, duration})
  },
  // 显示消息并跳转，会保留当前页面
  showToastNavigateTo: (message = '马上跳转', url = 'index', duration = 1000, icon = 'none') => {
    util.showToast(message, duration, icon)
    setTimeout(() => {
      wepy.navigateTo({url})
    }, duration)
  },
  // 显示消息并跳转，会关闭当前页面
  showToastRedirectTo: (message = '马上跳转', url = 'index', duration = 1000, icon = 'none') => {
    util.showToast(message, duration, icon)
    setTimeout(() => {
      wepy.redirectTo({url})
    }, duration)
  },
  // 显示消息并跳转至导航页
  showToastSwitchTabo: (message = '马上跳转', url = 'index', duration = 1000, icon = 'none') => {
    util.showToast(message, duration, icon)
    setTimeout(() => {
      wepy.switchTab({url})
    }, duration)
  },
  // 请求接口
  request: async (url, method = 'get', data = {}, options = {}, guard = 'users') => {
    wepy.showLoading({title: '加载中...'})
    if (options.header) {
      if (!options.header['content-type']) {
        options.header['content-type'] = 'application/json'
      }
    } else {
      options.header = {'content-type': 'application/json'}
    }
    let params = {
      url: host + '/' + url + '.html',
      method: method.toUpperCase(),
      data,
      dataType: options.dataType || 'json',
      responseType: options.responseType || 'text',
      header: options.header
    }
    let response = await wepy.request(params)
    wepy.hideLoading()
    if (response.statusCode >= 200 && response.statusCode < 300) {
      return response.data
    } else {
      if (response.statusCode === 401) {
        wepy.removeStorageSync(guard + '_token')
        util.showToastRedirectTo('登录凭据认证错误，请重新登录', 'login')
        return false
      }
      util.showToast(response.data.message || response.errMsg)
      return false
    }
  },
  // 登录后请求
  authRequest: async (url, method = 'get', data = {}, options = {}, guard = 'users') => {
    let token = util.getToken(guard)
    if (token) {
      if (options.header) {
        if (!options.header['content-type']) {
          options.header['content-type'] = 'application/json'
          options.header['authorization'] = token.token_type + ' ' + token.token
        }
      } else {
        options.header = {'content-type': 'application/json', 'authorization': token.token_type + ' ' + token.token}
      }
      return await util.request(url, method, data, options,guard)
    } else {
      return util.showToastRedirectTo('登录状态过期或无效，请重新登录', 'login')
    }
  },
  // 登录
  login: async (data, guard = 'users') => {
    let response = await util.request('login/' + guard, 'post', data)
    if (response !== false) {
      let storage = {
        token: response.access_token,
        token_type: response.token_type,
        expire_time: response.expires_in > 0 ? new Date().getTime() + response.expires_in * 1000 : response.expires_in
      }
      wepy.setStorageSync(guard + '_token', storage)
      return response.message
    } else {
      return false
    }
  },
  // 退出
  logout: async (url = 'login', guard = 'users') => {
    let response = await util.authRequest('logout/' + guard, 'get')
    if (response !== false) {
      wepy.removeStorageSync(guard + '_token')
      return util.showToastRedirectTo(response.message, url)
    } else {
      return false
    }
  },
  // 获取token
  getToken: (guard = 'users') => {
    let storage = wepy.getStorageSync(guard + '_token')
    if (storage) {
      if (storage.expire_time === 0 || (storage.expire_time > 0 && storage.expire_time > new Date().getTime())) {
        return {
          token: storage.token,
          token_type: storage.token_type
        }
      }
    }
    return false
  }
}

export default util
```

### 文件上传

#### 上传类

upload.js

```
import wepy from 'wepy'
import util from '@/service/util'

const uploadHost = 'http://www.sclzdj.cn/index.php/api/upload'

const upload = {
  upload: async (tempFilePath, name = 'file', formData = {'type': 'image'}) => {
    let response = await wepy.uploadFile({
      url: uploadHost,
      filePath: tempFilePath,
      name: name,
      formData: formData
    })
    if (response.statusCode >= 200 && response.statusCode < 300) {
      return JSON.parse(response.data)
    } else {
      util.showToast(response.data.message || response.errMsg)
      return false
    }
  },

  image: async (sizeType = ['original', 'compressed'], sourceType = ['album', 'camera']) => {
    let data = await wepy.chooseImage({
      count: 1,
      sizeType: sizeType,
      sourceType: sourceType
    })
    wepy.showLoading({title: '上传中...'})
    let response = await upload.upload(data.tempFilePaths[0])
    wepy.hideLoading()
    return response
  },

  images: async (count = 9, sizeType = ['original', 'compressed'], sourceType = ['album', 'camera']) => {
    let data = await wepy.chooseImage({
      count: count > 9 ? 9 : count,
      sizeType: sizeType,
      sourceType: sourceType
    })
    return data.tempFilePaths
  },

  video: async (compressed = false, maxDuration = 60, camera = 'back', sourceType = ['album', 'camera']) => {
    let data = await wepy.chooseVideo({
      compressed: compressed,
      maxDuration: maxDuration,
      camera: camera,
      sourceType: sourceType
    })
    wepy.showLoading({title: '上传中...'})
    let response = await upload.upload(data.tempFilePaths[0], 'file', {'type': 'video'})
    wepy.hideLoading()
    return response
  }
}

export default upload
```

#### 调用方法

wpy文件中调用

```
  import wepy from 'wepy'
  import util from '@/service/util'
  import upload from '@/service/upload'

  export default class Index extends wepy.page {
    data = {
      upload_img: '',
      upload_imgs: []
    }

    async uploadImg() {
      let response = await upload.image()
      if (response !== false) {
        this.upload_img = 'http://www.sclzdj.cn/' + response.data.path
        this.$apply()
      }
    }

    async uploadImgs() {
      let response = await upload.images()
      wepy.showLoading({title: '上传中...'})
      response.forEach(async (v, k) => {
        let res = await upload.upload(v)
        if (res !== false) {
          this.upload_imgs = this.upload_imgs.concat(['http://www.sclzdj.cn/' + res.data.path])
          this.$apply()
        }
        if (k + 1 === response.length) {
          wepy.hideLoading()
        }
      })
    }
 }
```

## weUI

样式库https://github.com/wepyjs/wepy-weui-demo

### 简单使用

克隆下来

```
git clone git@github.com:wepyjs/wepy-weui-demo.git
```

把样式文件复制到小程序项目

```
cp -r wepy-weui-demo/src/style myproject/src/style 
```

引入样式文件到app.wpy

```
<style lang="less">
@import 'style/weui.less';
...
```

### 底部导航栏

```
tabBar: {
    list: [{
      pagePath: 'pages/index',
      text: '首页',
      iconPath: 'images/index.png',
      selectedIconPath: 'images/index-active.png'
    }, {
      pagePath: 'pages/category',
      text: '分类',
      iconPath: 'images/category.png',
      selectedIconPath: 'images/category-active.png'
    }, {
      pagePath: 'pages/my',
      text: '我的',
      iconPath: 'images/my.png',
      selectedIconPath: 'images/my-active.png'
    }]
}
```

### 幻灯片

```
<swiper indicator-dots="true"
        autoplay="true" interval="5000" duration="1000">
    <repeat for="{{slides}}" key="index" index="index" item="item">
        <swiper-item>
            <navigator url="/pages/article?id={{item.id}}}" class="navigator-hover">
                <image src="{{item.pic}}" class="slide-image" style="width: 100%;"/>
            </navigator>
        </swiper-item>
    </repeat>
</swiper>
```

### 图文列表

```
<view class="weui-panel weui-panel_access">
    <view class="weui-panel__hd">图文组合列表</view>
    <view class="weui-panel__bd">
        <navigator url="" class="weui-media-box weui-media-box_appmsg" hover-class="weui-cell_active">
            <view class="weui-media-box__hd weui-media-box__hd_in-appmsg">
                <image class="weui-media-box__thumb" src="{{icon60}}" />
            </view>
            <view class="weui-media-box__bd weui-media-box__bd_in-appmsg">
                <view class="weui-media-box__title">标题一</view>
                <view class="weui-media-box__desc">由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。</view>
            </view>
        </navigator>
        <navigator url="" class="weui-media-box weui-media-box_appmsg" hover-class="weui-cell_active">
            <view class="weui-media-box__hd weui-media-box__hd_in-appmsg">
                <image class="weui-media-box__thumb" src="{{icon60}}" />
            </view>
            <view class="weui-media-box__bd weui-media-box__bd_in-appmsg">
                <view class="weui-media-box__title">标题二</view>
                <view class="weui-media-box__desc">由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。</view>
            </view>
        </navigator>
    </view>
    <view class="weui-panel__ft">
        <view class="weui-cell weui-cell_access weui-cell_link">
            <view class="weui-cell__bd">查看更多</view>
            <view class="weui-cell__ft weui-cell__ft_in-access"></view>
        </view>
    </view>
</view>
```

### 头像

```
<style lang="less">
    .head-img {
        text-align: center;
        .icon {
            width: 100px;
            height: 100px;
            border-radius: 50%;
        }
    }
</style>
<template>
    <view class="head-img">
        <image class="icon" mode="aspectFill" src="http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg"></image>
        <view class="container">
            sclzdj
        </view>
    </view>
</template>
```

### 登录页面

```
<style lang="less">

</style>
<template>
    <form @submit="login">
        <view class="weui-cells weui-cells_after-title">
            <view class="weui-cell weui-cell_input">
                <view class="weui-cell__hd">
                    <view class="weui-label">账号</view>
                </view>
                <view class="weui-cell__bd">
                    <input name="username" class="weui-input" placeholder="请输入账号"/>
                </view>
            </view>
            <view class="weui-cell weui-cell_input">
                <view class="weui-cell__hd">
                    <view class="weui-label">密码</view>
                </view>
                <view class="weui-cell__bd">
                    <input name="password" class="weui-input" placeholder="请输入密码"/>
                </view>
            </view>
        </view>
        <button class="weui-btn" type="primary" form-type="submit">登录</button>
        <button class="weui-btn" type="default" @tap="goIndex">首页</button>
    </form>
</template>

<script>
  import wepy from 'wepy'
  import util from '@/service/util'

  export default class Login extends wepy.page {
    methods = {
      goIndex() {
        wepy.switchTab({url: 'index'})
      },
      async login(e) {
        let formData = e.detail.value
        if (formData.username === '' || formData.password === '') {
          util.showToast('账号或密码不能为空')
          return false
        }
        let response = await util.login(formData, 'users')
        if (response !== false) {
          util.showToastSwitchTabo(response, 'my')
        }
      }
    }
  }
</script>

```

### 个人中心页面

```
<style lang="less">
    .head-img {
        text-align: center;
        .icon {
            width: 100px;
            height: 100px;
            border-radius: 50%;
        }
    }
</style>
<template>
    <view wx:if="{{userIsLogin !== false}}">
        <view class="head-img">
            <image class="icon" mode="aspectFill" src="{{userInfo.head_img}}"></image>
            <view class="container">
                {{userInfo.username}}
            </view>
        </view>
        <view>
            <button class="weui-btn" type="primary" @tap="logout">退出登录</button>
        </view>
    </view>

</template>

<script>
  import wepy from 'wepy'
  import util from '@/service/util'

  export default class My extends wepy.page {
    config = {
      navigationBarTitleText: '个人中心'
    }
    data = {
      userIsLogin: util.getToken(),
      userInfo: {}
    }
    methods = {
      async logout() {
        await util.logout('login', 'users')
      }
    }

    async getUserInfo() {
      let response = await util.authRequest('index/user')
      if (response !== false) {
        this.userInfo = response
        this.$apply()
      }
    }

    onLoad = () => {
      if (util.getToken() === false) {
        wepy.redirectTo({url: 'login'})
      } else {
        this.getUserInfo()
      }
    }
  }
</script>

```

