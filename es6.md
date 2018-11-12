# ES6

## 变量

### var

>函数外声明是全局变量，函数内声明是局部变量

>会被**预解析**

> 变量挂载在window对象中，比如`var dj=11; console.log(window.dj);`

### let

>一对花括号{ }视为一个作用域
>
>只在作用域内有效
>
>不能在作用域里面重复声明
>
>不影响其它作用域的值，嵌套也不影响

> 变量不再挂载在window对象中

## 常量

### const

>不能修改，具有只读性
>
>声明时必须赋值
>
>作用域和let相同

## 数据类型

### 基本类型

> null

> undefined

> string

> number

> boolean

### 引用类型

> object

### Symbol类型

数据是独一无二的，一般作为对象的属性名或数组的键名使用

```
//定义Symbol类型变量a，comment为注释
let a = Symbol('comment');
let obj = {
	[a]:1,//Symbol类型的属性名
	b:2,
	c:3,
}
obj.a = '123';//新增a属性并赋值
obj.b = '1234';//修改b属性的值
//普通类型的属性名对应的属性值的调用方法
console.log(obj.a);
console.log(obj['a']);
console.log(obj.b);
console.log(obj['b']);
console.log(obj.c);
console.log(obj['c']);
//Symbol类型的属性名对应的属性值的调用方法
console.log(obj[a]);
//修改Symbol类型的属性名对应的属性值
obj[a] = 'aaaaaa';
console.log(obj[a]);
```

### Set类型

> 类似于无重复数据的数组

>是一个对象

> Set结构的数据不允许有重复值存在

> 键值和键名是一模一样的

#### 简单创建

```
let s = new Set();
s.add(1);
s.add(2);
s.add(3);
s.add(2);
s.add(3);
s.add(4);
s.add(1);
console.log(s);

//初始化方法
let s=new Map([1,2,3,4,'3',2,3,3,3,1])
```

#### 数组去重

```
//第一种方法
    let a = [1,2,3,4,'3',2,3,3,3,1];
    console.log(a);
    let s = new Set(a);
    //将set结构数据变成数组
    //数组的扩展运算符...
    let arr = [...s];
    console.log(s);
    console.log(arr);

//第二种方法
    let a = [1,2,3,4,'3',2,3,3,3,1];
    console.log(a);
    let s = new Set(a);
    //Array.from()方法可以将set数据转成数组
    let arr = Array.from(s);
    console.log(s);
    console.log(arr);
```

#### 操作方法

```
let s = new s();
s.add(1);//添加
s.delete(1);//删除
s.has(1);//判断是否存在，返回boolean
s.clear();//清空
s.size();//长度
```

#### 遍历方法

可使用forEach遍历

```
let s=new Set(['1','b','2fd'])
s.forEach(v=>{
	console.log(v);
})
```

还会生成三个遍历器，其中键名遍历器和键值遍历器一模一样

>s.keys()    键名遍历器  遍历出来   `['1','b','2fd']`
>
>s.values()  键值遍历器  遍历出来   `['1','b','2fd']`
>
>s.entries()  键值和键名遍历器  遍历出来   `[['1','1'],['b','b'],['2fd','2fd']]`

遍历器只能用for of遍历

```
let s=new Set(['1','b','2fd'])
for (k of s.keys()) {
	console.log(k);
}
for (v of s.keys()) {
	console.log(v);
}
for (item of s.entries()) {
	console.log(item[0],item[1]);//键名item[0]和键值item[1]完全相同
}
```

### Map类型

>相当于改造后的对象数据
>
>键名支持任意类型，**传址类型要在外面先声明**

#### 基本使用

```
//Map结构的数据其实就是一种"键-值"对应关系
let m = new Map();
m.set(123,'abc');
console.log(m.get(123));

//初始化方法
let s=new Map([['1','1'],[22,'b'],[false,'2fd']])
```

#### 操作方法

```
let m = new Map();
m.set(123,'abc');//设置键名123的值
m.get(123);//得到键名123的值
m.delete(123);//删除键123和值
```

#### 遍历方法

可使用forEach遍历

```
let m=new Map([['1','1'],[22,'b'],[false,'2fd']])
m.forEach((v,k)=>{
	console.log(k);
	console.log(v);
})
```

还会生成三个遍历器

> m.keys()    键名遍历器  遍历出来   `['1',22,false]`
>
> m.values()  键值遍历器  遍历出来   `['1','b','2fd']`
>
> m.entries()  键值和键名遍历器  遍历出来   `[['1','1'],[22,'b'],[false,'2fd']]`

遍历器只能用for of遍历

```
let m=new Set([['1','1'],[22,'b'],[false,'2fd']]])
for (k of m.keys()) {
	console.log(k);
}
for (v of m.keys()) {
	console.log(v);
}
for (item of m.entries()) {
	console.log(item[0],item[1]);//键名item[0]和键值item[1]
}
```

#### 扩展运算符  ...

```
let m = new Map([['1','1'],[22,'b'],[false,'2fd']]])
let reM = [...m];//二维数组，结果结构示例[['1','1'],[22,'b'],[false,'2fd']]]
let reMk = [...m.keys()];//键名组成的一维数组
let reMv = [...m.values()];//键值组成的一维数组
let reMe = [...m.entries()];//二维数组，结果同reM
```

## 箭头函数

### 基本使用

> 不需要传入参数的话,需要写一个小括号
> 如果函数体只有一句话,那么可以不用加花括号和return
> 如果有两个或两个以上的参数,就必须加括号

```
let foo1 = ()=>6;

let foo2 = ()=>{
	let x=12;
	return x;
};

let foo3 = x=>x*x;

let foo4 = (x)=>x*x;

let foo5 = (x,y)=>x*y;

let foo6 = (x,y)=>{
	x++;
	y++;
	return x*y;
};
```

### 参数默认值

```
let foo = (x=0,y=0)=>{
	x++;
	y++;
	return x*y;
};
```

### 多余参数  ...

```
//f即为rest参数，它会获得函数调用的时候多余的参数
//arguments对象是获得所有传入的参数，箭头函数不支持
let foo = (x,y,...f)=>{
	console.log(f);
	return x+y;
}
let result = foo(2,3,4,5,6);
```

### 参数解构

#### 简单示例

```
let foo = ([x,y])=>{
	return x*y;
}
console.log(foo([6,7]));
```

应用场景示例

```
let arr = [
    [2,3],
    [7,8],
    [9,2],
]
var newarr = arr.map(([x,y])=>{
	return x*y;
})
console.log(newarr);
```

## 数组

### 扩展运算符  ...

可作用于set数据结构

```
let x = [3,4,5,6,7];
//数组扩展运算符
//相当于是将数组打散
let y = [8,...x,9,10];
console.log(y);
//[8,3,4,5,6,7,9,10]
```

### 遍历

#### forEach

```
let arr = ['苹果','西瓜','香蕉','芒果'];
//forEach方法不会改变原数组
//第一个参数表示当前遍历到的数据
//第二个参数表示当前遍历到的数据的序号
//第三个参数表示遍历的数组完整数据,即本身数据arr
arr.forEach((v,k,t)=>{

})
```

#### map

```
let arr = [5,6,7,8,9];
//map()方法不会改变原数组
//return的值会将原数组中的对应值替换掉
//第一个参数表示当前遍历到的数据
//第二个参数表示当前遍历到的数据的序号
//第三个参数表示遍历的数组完整数据,即本身数据arr
let newarr = arr.map((v,k,t)=>{
	return v*v;
})
console.log(arr);
console.log(newarr);
```

### 过滤

#### filter

```
var arr = [12,35,65,34,56,76,19,88];
//filter方法不会改变原数组
//filter方法的return只有两种情况,true或者false
//如果返回true就保留这个数据,如果返回false,就删除这个数据
//第一个参数表示当前遍历到的数据
//第二个参数表示当前遍历到的数据的序号
//第三个参数表示遍历的数组完整数据,即本身数据arr
var newarr = arr.filter((v,k,t)=>{
	return v>=60;
})
console.log(arr);
console.log(newarr);
```

### 解构赋值

#### 简单使用

```
//一般用法
//将结构解开,然后分别按顺序赋值
//如果右边缺少则相应的参数是undefined
let [x,y,z] = [5,6];
console.log(x);
console.log(y);
console.log(z);

//解构赋值遵循"模式匹配"的原则,只要是左右格式一样,就能正常赋值
let [x,[[[[y]]]],[[z]]] = [1,[[[[2]]]],[[3]]];
console.log(x,y,z);

```

#### 应用场景

##### 变量互相替换

```
let x = 1;
let y = 2;
[y,x] = [x,y];
```

## 对象

### 解构赋值

```
//	对象解构赋值的时候和顺序无关,只看对应的属性名和变量名
//	还可以起别名
let {url,name:mingzi} = {name:'这是名称',url:'www.baidu.com'};
console.log(mingzi);
console.log(url);
//  只要是对象的属性都可以结构，例如数组的长度
let arr = [3,4,5,6];
let {length:arrLen} = arr;
console.log(arrLen);
let str = 'sfafsa';
let {length:strLen} = arr;
console.log(strLen);
```

## 字符串

### 解构赋值

```
//按字母解构
let [a,b,c,d,e,f] ='houdun';
console.log(a);
console.log(c);
```

### 模板字符串

> 支持换行
>
> 支持单双引号
>
> 支持引用变量，用`${}`包起来

简单示例

```
let dj = '你好';
//请使用反单引号包起来即可
let str = `ho"djun"'r萨达e'n${dj}sf撒
发放sa
faf`;
console.log(str);
```

## 类 class

### 用函数模拟类

```
function ren(){
	this.name = '名字';
	this.age = '年龄';
	this.sex = '性别';
	this.run = function(){
		console.log('能跑');
	}
}
var zhangsan = new ren();
console.log(zhangsan.sex);
zhangsan.run();
```

### 简单创建

```
// class用来声明一个类，以创建动物类为例
class animal{
	// 构造函数
	// 在实例化类的时候,会自动执行构造函数
	// 需要定义给对象的属性,都要声明在constructor里面
	// 可以传递参数，参数可以设置默认值
	constructor(color='red',size='22cm',weight='24kg'){
		// console.log('类被实例化了');
		this.color = color;
		this.size = size;
		this.weight = weight;
	}
	// 类里面不同方法之间不需要用逗号隔开
	// 类里面的方法不需要用function来声明
	move(){
		console.log('能动');
	}
	say(){
		console.log('能说话');
	}
}
// 如果不需要传参,实例化的时候可以不用加括号
// let xiaohuang = new animal('red','10m',45kg);
// let xiaohuang = new animal('red','10m');
let xiaohuang = new animal;
console.log(xiaohuang.color);
xiaohuang.move();
xiaohuang.say();
```

### 继承

> 子类会继承父类的所有属性和方法

> 可以重写父类的方法

> 子类的构造方法constructor必须先执行super()方法后才可以使用this关键字，其它方法可以直接使用this关键字，this关键字一般用于设置或调用属性和当前类中的方法（不包括父类方法）

> 子类的任意方法中都可以使用super对象可以调用父类所有方法

>子类若也有构造方法，那么父类的传参失效，只有子类传参有效

```
//子类(以鱼类示例)
class fish extends animal{
    constructor(lin='鳞片',sai='鱼鳃'){
        super();
        this.lin = lin;
        this.sai = sai;
        super.say();
    }
    // 重写父类方法
    move(){
        this.swim();
    }
    // 子类特有的方法
    swim(){
        console.log('能游动');
    }
}
let shayu = new fish('鳞片--','鱼鳃--');
shayu.swim()
```

### 表达式

```
let a = class animal{
    constructor(){
    	this.color = '颜色';
    	this.size = '尺寸';
    }
}
//不能再用new animal()
let dj = new a();
console.log(dj.color);
```

## 异步处理

### Promise

#### 简单使用

```
let promise = new Promise((resolve,reject)=>{
    // resolve表示将状态变成成功完成
    // reject表示将状态变成失败完成
    setTimeout(()=>{
        if(true){
        	resolve({title:'21',content:'safa'}) //成功执行，并返回数据
        }else{
        	reject('bad error')  //执行失败，并返回错误消息
        }
    },2000)
})
promise.then((response)=>{//执行成功的处理
	console.log(response)
},(error)=>{//对错误的处理
	console.log(error)
})
```

#### 监控多个promise

```
let p1 = new Promise((resolve,reject)=>{
	let time = Math.random()*4000+1000;
	setTimeout(()=>{
		console.log('p1完成');
		resolve();
	},time)
})

let p2 = new Promise((resolve,reject)=>{
	let time = Math.random()*4000+1000;
	setTimeout(()=>{
		console.log('p2完成');
		resolve();
	},time)
})
let p3 = new Promise((resolve,reject)=>{
	let time = Math.random()*4000+1000;
	setTimeout(()=>{
		console.log('p3完成');
		resolve();
	},time)
})
let p4 = new Promise((resolve,reject)=>{
	let time = Math.random()*4000+1000;
	setTimeout(()=>{
		console.log('p4完成');
		resolve();
	},time)
})
// 监控多个Promise对象执行完成
let p = Promise.all([p1,p2,p3,p4]);
p.then(()=>{
	console.log('全部执行完毕！');
})
```

### async  await

> 异步函数

>属于es7以后的概念

```
//以做饭示例
let x = ()=>{//买菜函数
	console.log('去买菜啦！');
	let p = new Promise((resolve,reject)=>{
		setTimeout(()=>{
			console.log('买菜完毕！');
			resolve();
		},3000)
	})
	return p;
}
let y = async ()=>{//做菜函数
	console.log('刷锅');
	console.log('准备佐料');
	//遇到await程序会等待当前函数(x)执行完毕,再继续执行(阻塞)
	await x();
	console.log('切菜');
	console.log('炒菜');
	console.log('over');
}
y();
```

## 模块化

目前只支持高版本火狐浏览器

### 简单使用

index.html，a.js，b.js在同一目录

示例b.js

```
//一个js文件就是一个模块
//模块中的变量是不会被外界所访问到的
//导出数据需要用export命令
let dj = '标题';
let abc = '123';
//export导出数据的时候需要加花括号
export {dj,abc};
```

示例a.js

```
//用import命令可以导入其他模块中的数据
import {dj,abc} from './b.js';//注意前面的./不能省略
console.log(dj,abc);
```

示例index.html

```
<script type="module" src="./a.js"></script>
```

### 别名

as关键字

```
import {dj as dujun} from './b.js';

import {abc,dj as dujun，foo} from './b.js';
```

### 单个数据导出

#### export default

```
let dj = '123';
export default dj;
```

```
//名字随便起，不用花括号
import dj from './b.js';
//import dujun from './b.js';
```

### 混合使用

```
let dj = '123';
let foo = '123';
let abc = '123';
//混合导出
export default dj;
export {foo,abc};
```

```
//混合导入
import dj,{foo as f,abc} from './b.js';
```

### 导出数据特点

#### 传址