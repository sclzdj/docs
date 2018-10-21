# 登录权限

## 修改配置文件

打开配置文件config/auth.php

### 添加守卫

guards项

```
'admin' => [//填守卫名称
    'driver' => 'session',//填存储驱动
    'provider' => 'bs_admins',#使用哪个服务提供者，一般填表名
],
```

### 添加服务提供者

providers项

```
'bs_admins' => [//服务提供者名称
    'driver' => 'eloquent',//填驱动机制
    'model' => \App\Model\Admin\Admin::class,//填模型名
],
```

## 登录功能示例  

### 移动Auth控制器目录和视图目录到对应文件夹下  

如果没有Auth目录 可执行命令生成

```
php artisan make:auth
```



### 添加路由Auth::routes();到对应位置

只是要登录功能的话只用加下面三条路由

```
Route::get('login','Auth\LoginController@showLoginForm');
Route::post('login','Auth\LoginController@login');
Route::post('logout','Auth\LoginController@logout');
```

### 修改Login控制器

#### 修改登录后跳转地址

```
protected $redirectTo = '/admin/index/index'; //默认/home
```

使用phpstrom编辑器时覆盖方法应该使用快捷键ctrl+o，否则覆盖方法时要注意引用的类

#### 覆盖showLoginForm方法

```
public function showLoginForm(){
    return view('admin/auth/login');//对应视图
}
```

#### 覆盖username方法

```
public function username(){
    return 'username';//默认是email
}
```

#### 覆盖guard方法

```
protected function guard(){
    return \Auth::guard('admin');//默认是web
}
```

#### 覆盖logout方法

```
public function logout(Request $request){
    $this->guard()->logout();
    $request->session()->invalidate();
    return $this->loggedOut($request) ?: redirect('/admin/login');//退出后跳回登录页
}
```

#### 覆盖validateLogin方法

```
protected function validateLogin(Request $request){
    $this->validate($request, [
        $this->username() => 'required',
        'password' => 'required',
    ],[
        $this->username().'.required'=>'账号必填',
        'password.required'=>'密码必填',
    ]);//加验证中文提示
}
```

### 更改登录视图表单相应提交地址

### 修改Admin模型头部继承

```
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
class Admin extends Authenticatable
{
    use Notifiable;
    //....
}
```

### 数据迁移admin文件添加

```
$table->rememberToken();
```

模型工厂也要填充该字段

### 已登录时再进入登录页跳转修改

#### 修改Login控制器

```
public function __construct(){
    $this->middleware('guest:admin')->except('logout');//加守卫admin
}
```

#### 修改guest中间件类

\App\Http\Middleware\RedirectIfAuthenticated@handle方法

```
public function handle($request, Closure $next, $guard = null){
    switch ($guard){
        case 'admin':
            $path='/admin/index/index';
            break;
        default:
            $path='/';
            break;
    }
    if (Auth::guard($guard)->check()) {
        return redirect($path);
    }
    return $next($request);
}
```

### 路由其中添加需要登录验证的中间件

```
//后台
Route::group(['prefix' => 'admin', 'namespace' => 'Admin'], function() {
    Route::group(['middleware' => 'auth:admin'], function() {//加守卫
        //这里面写需要登录的路由
        Route::resource('admin', 'AdminController')->middleware('permission');
    });
    //这下面写不需要登录的路由
    Auth::routes();
});
```

### 需要登录的方法如果为登录跳回登录页

覆盖错误异常App\Exceptions\Handler类的父类unauthenticated方法
在App\Exceptions\Handler类中添加以下方法

```
protected function unauthenticated($request,\Illuminate\Auth\AuthenticationException $exception) {
    if($request->expectsJson()){
        response()->json(['message' => $exception->getMessage()], 401);
    }else{
        $guards=$exception->guards();
        if(in_array('admin',$guards)){
            $url='/admin/login';
        }else{
            $url='/login';
        }
        return redirect()->guest($url);
    }
}
```

### 账号或密码错误时中文提示

```
@if ($errors->has('username'))
    {{$errors->first('username')=='These credentials do not match our records.'?'账号或密码错误':$errors->first('username')}}
@endif
```

暂时只想到这个方法，应该还有更专业的方法

## 注册功能示例

依照登录功能自己摸索，很简单的。

## 修改密码示例

依照登录功能自己摸索，很简单的。

## 找回密码示例

依照登录功能自己摸索，很简单的。