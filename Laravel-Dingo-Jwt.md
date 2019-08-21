# Dingo

## 安装组件

### composer安装

```
composer require dingo/api:2.0.0-alpha2
```

### 生成配置文件

```
php artisan vendor:publish
```

选择对应dingo配置序号

### 修改配置文件

打开配置文件config/api.php

```
#接口围绕：x：本地私有环境 prs：公司内部APP或小程序使用 vnd：公开接口
'standardsTree' => env('API_STANDARDS_TREE', 'prs'),
#项目名称
'subtype' => env('API_SUBTYPE', 'blog'),
#版本号
'version' => env('API_VERSION', 'v1'),
#前缀（和域名二选一）
'prefix' => env('API_PREFIX', 'api'),
#域名（和前缀二选一）
'domain' => env('API_DOMAIN', null),
#接口报错返回详细信息
'debug' => env('API_DEBUG', false),
```

## 路由（版本和请求限制）

打开路由文件routes/api.php

```
$api=app(\Dingo\Api\Routing\Router::class);
#默认指定的是v1版本和前缀方式，则直接通过 {host}/{前缀}/{接口名} 访问即可
#namespace为API控制器公共命名空间
$api->version('v1',['namespace'=>'\App\Http\Controllers\Api'],function ($api){
	#api.throttle中间件是限制请求次数 每expires分钟只能请求limit次
    $api->group(['middleware'=>'api.throttle','limit'=>1000,'expires'=>1],function($api){
        $api->get('users','UserController@users');
        $api->get('contents','ContentController@contents');
    });
});

#默认指定的不是v2版本，需要先设置请求头 #Accept:application/[配置项standardsTrss].[配置项subtype].v2+json，再通过 {host}/{前缀}/{接口名} 访问
$api->version('v2',function ($api){
    $api->get('version',function(){
        return "v2";
    });
});
```

## 基础控制器

创建基础控制器BaseController

```
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use Dingo\Api\Routing\Helpers;

class BaseController extends Controller
{
    use helpers;
}
```

其他的接口控制器都继承BaseController控制器

## 响应生成器

### 响应类型

#### 响应一个数组

```
class UserController extends BaseController
{
	public function show($id)
	{
		$user = User::findOrFail($id);

		return $this->response->array($user->toArray());
	}
}
```

#### 响应一个元素

```
class UserController extends BaseController
{
	public function show($id)
	{
		$user = User::findOrFail($id);

		return $this->response->item($user, new UserTransformer);
	}
}
```

#### 响应一个元素集合

```
class UserController extends BaseController
{
	public function index()
	{
		$users = User::all();

		return $this->response->collection($users, new UserTransformer);
	}
}
```

#### 分页响应

```
class UserController extends BaseController
{
	public function index()
	{
		$users = User::paginate(25);

		return $this->response->paginator($users, new UserTransformer);
	}
}
```

#### 无内容响应

```
return $this->response->noContent();#204 #一般都用这个
```

#### 创建了资源的响应

```
return $this->response->created();#201
```

你可以在第一个参数的位置，提供创建的资源的位置。第二个参数提供内容信息。

```
return $this->response->created($location,$content);#201
```

#### 接受请但未处理响应

```
return $this->response->accepted();#202
```

#### 错误响应

这有很多不同的方式创建错误响应，你可以快速的生成一个错误响应。

```
// 一个自定义消息和状态码的普通错误。
return $this->response->error('This is an error.', 400);

//#400 一个 bad request 错误，第一个参数可以传递自定义消息。
return $this->response->errorBadRequest();   #一般的错误都用这个

//#401 一个未认证错误，第一个参数可以传递自定义消息。
return $this->response->errorUnauthorized();

//#403 一个服务器拒绝错误，第一个参数可以传递自定义消息。
return $this->response->errorForbidden();

//#404 一个没有找到资源的错误，第一个参数可以传递自定义消息。
return $this->response->errorNotFound();

//#405一个方法不允许错误，第一个参数可以传递自定义消息。
return $this->response->errorMethodNotAllowed();

//#500 一个内部错误，第一个参数可以传递自定义消息。
return $this->response->errorInternal();
```

### 添加额外的头信息

一旦你已经使用了上面的方法，你就可以自己添加额外的头信息

```
return $this->response->item($user, new UserTransformer)->withHeader('X-Foo', 'Bar');
```

### 添加 Meta 信息

一些数据转换层也许利用了 meta 数据。当你需要提供链接资源的额外数据这将会很有用。

```
return $this->response->item($user, new UserTransformer)->addMeta('foo', 'bar');
```

你也可以设置一个 meta 数据的数组，代替多次调用多次 addMeta 方法。

```
return $this->response->item($user, new UserTransformer)->setMeta($meta);
```

### 设置响应状态码

```
return $this->response->item($user, new UserTransformer)->setStatusCode(200);
```

## Transformers

### 示例

文章接口ContentController示例：

```
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Api\Transformers\ContentTransformer;
use Illuminate\Http\Request;
use Modules\Article\Entities\Content;

class ContentController extends BaseController
{
    public function contents(){
        return $this->response->paginator(Content::paginate(10),new ContentTransformer());
    }
    
    public function show($id)
    {
        $content=Content::find($id);
        return $this->response->item($content,new ContentTransformer());
    }
}
```

对应的ContentTransformer类示例：

```
namespace App\Http\Controllers\Api\Transformers;

use League\Fractal\TransformerAbstract;#必须引用
use Modules\Article\Entities\Content;

class ContentTransformer extends TransformerAbstract
{
	#要返回关联信息在这里面定义
	#并且要有对应的include方法和对应的Transforme类
	#下面示例关联分类：category
    protected $availableIncludes=['category'];
    
    public function transform(Content $content){
        return [
            'id'=>$content->id,
            'title'=>$content->title,
            'author'=>$content->author,
            'thumb'=>$content->thumb,
            'created_at'=>$content->created_at->toDateTimeString(),#时间处理
        ];
    }
    #此处为文章关联的分类，确保文章模型中有对应的belongsTo方法
    public function includeCategory(Content $content){
        return $this->item($content->category,new CategoryTransformer());
    }
}
```

对应的CategoryTransformer类示例：

```
namespace App\Http\Controllers\Api\Transformers;

use League\Fractal\TransformerAbstract;#必须引用
use Modules\Article\Entities\Category;

class CategoryTransformer extends TransformerAbstract
{
    public function transform(Category $category)
    {
        return [
            'id'=>$category->id,
            'name'=>$category->name,
        ];
    }
}
```

对应的访问方法：{host}/{前缀}/{接口名}?include=category

### 关联访问方法

{host}/{前缀}/{接口名}?include={关联1},{关联2},{关联3}...

## 导出文档

```
php artisan api:docs --name uun.dj/api --use-version v1 --output-file "C:\Users\Administrator\Desktop\api\uun.md"
```

> --name   文档主题 

> --use-version   API版本号

> --output-file   导出位置

代码注释必须按照规定格式，否则导出文档时报错，简单示例：

```
/**
 * 账号控制器
 * @Resource("/bs/system/admin")
 */
class AdminController extends AuthAdminController
{
    /**
     * 账号列表
     * @Versions({"v1"})
     * @Get("/index{?page,limit}")
     * @Request(contentType="application/json")
     * @Parameters({
     * 		@Parameter("page",type="int",default=1,required=false,description="页码"),
     *      @Parameter("limit",type="int",default="system",description="每页长度"),
     *      @Parameter("username",type="string",default=null,description="账号"),
     *      @Parameter("nickname",type="string",description="昵称"),
     *      @Parameter("status",type="options",description="状态[0,1]"),
     *      @Parameter("created_at_start",type="string",description="开始日期[Y-m-d]"),
     *      @Parameter("created_at_end",type="string",description="结束日期[Y-m-d]")
     * })
     * @Response(200,body=null)
     */
    public function index(Request $request)
    {
        //......
    }
    //......
}
```



# JWT

## 安装组件

### composer安装

```
composer require tymon/jwt-auth:dev-develop --prefer-source
```

### 生成配置文件

```
php artisan vendor:publish
```

选择对应jwt配置序号

### 修改配置文件

打开配置文件config/jwt.php

```
#令牌过期时间（分钟），设置null永久不过期
'ttl' => env('JWT_TTL', 60),
#令牌刷新时间（分钟），设置null永久随时刷新
'refresh_ttl' => env('JWT_REFRESH_TTL', 20160),
```

## 生成密钥

这是用来给你的token签名的钥匙，用下面命令生成：

```
php artisan jwt:secret
```

这将用JWT_SECRET=footbar更新.env文件

## 权限模型修改

下面示例用户模型：

```
use Illuminate\Notifications\Notifiable;#必须引用
use Illuminate\Foundation\Auth\User as Authenticatable;#必须引用
use Tymon\JWTAuth\Contracts\JWTSubject;#必须引用

class User extends Authenticatable implements JWTSubject
{
    use Notifiable;
    
    #必须定义
    #获取存储在JWT主题声明中的的标识符，一般就是主键
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }
    #必须定义
    #返回一个键值数组，包含添加到JWT的任何自定义声明
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```

## 配置验证守卫

### 修改auth配置文件

config/auth.php

守卫使用jwt保护来为借口身份验证提供支持

```
'guards' => [#守卫列表
        'api' => [   #守卫名称，可定义多个实现多守卫
            'driver' => 'jwt',  #使用jwt驱动
            'provider' => 'users', #使用哪个服务提供者，一般填表名
        ],
    ],
'providers' => [#服务提供者列表
        'users' => [#服务提供者名称，可定义多个服务提供者
            'driver' => 'eloquent',#使用模型驱动
            'model' => App\User::class,#模型名称
        ],
    ],
```

### 修改dingo配置文件

config/api.php

添加身份验证提供者

```
'auth' => [
        'jwt'=>\Dingo\Api\Auth\Provider\JWT::class,
    ],
```

## 路由定义

```
$api=app(\Dingo\Api\Routing\Router::class);

$api->version('v1',['namespace'=>'\App\Http\Controllers\Api'],function ($api){
    $api->group(['middleware'=>'api.throttle','limit'=>1000,'expires'=>1],function($api){
        #登录
        $api->post('login','AuthController@login');
        #退出
        $api->get('logout','AuthController@logout');
        #个人资料（可有可无）
        $api->get('info','AuthController@info');
        #刷新token（一般不定义）
        #$api->get('refresh','AuthController@me');
    });
});
```

## 验证控制器

需要验证的接口就继承验证控制器

```
namespace App\Http\Controllers\Api;

class AuthController extends BaseController
{
    public function __construct()
    {
        // 除login外都需要验证
        $this->middleware('jwt.auth:admin', ['except' => ['login']]);
    }
    /**
     * 响应token
     * @param $token
     * @return mixed
     */
    protected function respondWithToken($token)
    {
        return $this->response()->array([
            'access_token' => $token,
            'token_type'   => 'bearer',
            'expires_in'   => auth('admin')->factory()->getTTL() * 60,
        ]);
    }
    /**
     * 登录
     * @Versions({"v1"})
     * @Post("/bs/login/admin")
     * @Request(contentType="application/json")
     * @Parameters({
     *      @Parameter("username",type="string",required=true,description="账号"),
     *      @Parameter("password",type="string",required=true,description="密码")
     * })
     * @Response(200,body=null)
     */
    public function login()
    {
        $request = request(['username', 'password']);
        if (!isset($request['username']) || $request['username'] === '' || $request['username'] === null) {
            return $this->response->error('请输入帐号',422);
        }
        if (!isset($request['password']) || $request['password'] === '' || $request['username'] === null) {
            return $this->response->error('请输入密码',422);
        }
        if (!$token = auth($this->guard)->attempt($request)) {
            return $this->response->error('帐号或密码错误',422);
        }
        return $this->respondWithToken($token);
    }
    /**
     * 退出
     * @Versions({"v1"})
     * @Get("/bs/logout/admin")
     * @Request(contentType="application/json")
     * @Response(204,body=null)
     */
    public function logout()
    {
        auth('admin')->logout();
        return $this->response()->noContent();
    }
    /**
     * 账号资料
     * @Versions({"v1"})
     * @Get("/bs/info/admin")
     * @Request(contentType="application/json")
     * @Response(200,body=null)
     */
    public function info()
    {
        return $this->response()->item(auth('admin')->user(),new AdminTransformer());
    }
    /**
     * 刷新token
     * @Versions({"v1"})
     * @Get("/bs/refresh/admin")
     * @Request(contentType="application/json")
     * @Response(200,body=null)
     */
    public function refresh()
    {
        return $this->respondWithToken(auth('admin')->refresh());
    }
}
```

## 请求方式

需要设置请求头 

Authorization:Bearer {token}

# 状态码

### 错误类

> 400  请求错误
>
> 401  认证错误
>
> 403  拒绝服务
>
> 404  资源未找到
>
> 405  方法不允许
>
> 422  请求数据验证错误 
>
> 500  内部错误，token传错时也报此错误

### 成功类

> 200  成功     服务器已成功处理了请求
>
> 201  创建     表示服务器执行成功，并且创建了新的资源
>
> 202  已接受   服务器接受请求，但未处理
>
> 203  非授权信息  服务器成功执行了请求，但是返回的信息可能来自另一来源
>
> 204  空内容    服务器成功执行请求，但是没有返回信息
>
> 205  重置内容   服务器成功执行了请求，但是但是没有返回内容，与204不同，他需要请求者重置文档视图（比如，清除表单内容，重新输入）
>
> 206  部分内容  服务器成功执行了部分请求

### 权威

200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
429 Too Many Requests - 由于请求频次达到上限而被拒绝访问
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

### 判断方法

```
if({code}>=200 && {code}<=299){	
	#成功
}else{
	#错误
	#弹出信息
	//{message}--普通错误提示
	//{errors}--验证错误提示，是数组
}
```

