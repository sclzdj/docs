# VUE

## 构建应用

### 简单构建

```
<div id="app">
   
</div>
<script src="vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            
        },
        methods: {//方法

        }
    });
</script>
```

### 完整构建

```
<div id="app">
    <!-- 文本框 -->
    <div>
        <label>文本框：</label>
        <input type="text" v-model="textName">
    </div>
    <!-- 多行文本框 -->
    <div>
        <label>文本框：</label>
        <textarea v-model="textareaName"></textarea>
    </div>
    <!-- 单选按钮 -->
    <div>
        <label>单选按钮：</label>
        <div v-for="v in selectOptions">
            <input type="radio" v-model="radioName" :value="v.id">{{v.value}}
        </div>
    </div>
    <!-- 多选按钮 -->
    <div>
        <label>单个多选框：</label>
        <input type="checkbox" v-model="checkboxStatus"><!--选中为true，未选中为false-->
    </div>
    <div>
        <label>多个多选框：</label>
        <div v-for="v in selectOptions">
            <input type="checkbox" v-model="checkboxName" :value="v.id">{{v.value}}
        </div>
    </div>
    <!-- 单选下拉 -->
    <div>
        <label>单选下拉：</label>
        <select v-model="selectName">
            <option value=""></option>
            <option v-for="v in selectOptions" :value="v.id">{{v.value}}</option>
        </select>
    </div>
    <!-- 多选下拉 -->
    <div>
        <label>多选下拉：</label>
        <select v-model="selectMultipleName" multiple size="5">
            <option value="">请选择</option>
            <option v-for="v in selectOptions" :value="v.id">{{v.value}}</option>
        </select>
    </div>
</div>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script src="lodash.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            textName: '',
            textareaName: '',
            radioOptions: [
                {id: 1, value: 'v1'},
                {id: 2, value: 'v2'},
                {id: 3, value: 'v3'},
            ],
            radioName: '',
            checkboxOptions: [
                {id: 1, value: 'v1'},
                {id: 2, value: 'v2'},
                {id: 3, value: 'v3'},
            ],
            checkboxName: [],
            checkboxStatus: false,
            selectOptions: [
                {id: 1, value: 'v1'},
                {id: 2, value: 'v2'},
                {id: 3, value: 'v3'},
            ],
            selectName: '',
            selectMultipleName: [],
            n1: 1,
            n2: 2,
            word: '',
            result: ''
        },
        methods: {//方法

        },
        computed: {//计算属性
            sum: function () {
                return this.n1 * 1 + this.n2 * 1;//*1是为了使字符串型转整型或浮点型
            }
        },
        mounted(){//挂载方法
            
        },
        watch: {//监听属性
            word: _.debounce(//减少后台压力请求，每5秒执行一次
                function (newV, oldV) {
                    console.log("新值：" + newV + "，旧值：" + oldV);
                    axios.get('****.php?word=' + newV).then(function (response) {//axios异步请求
                        console.log(response);
                        app.result = response.data;//注意此处不能用this.result，而应该用app.result
                    });
                }, 5)
        }
    });
</script>
```

#### 方法

```
<div id="app">

</div>
<script src="vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            num: 1
        },
        methods: {//方法
			numIncr:function(){
                this.num++;
			}
        },
</script>
```

#### 计算属性

```
<div id="app">

</div>
<script src="vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            n1: 1,
            n2: 2,
        },
        computed: {//计算属性
            sum: function () {
                return this.n1 * 1 + this.n2 * 1;//*1是为了使字符串型转整型或浮点型
            }
        }
    });
</script>
```

#### 监听

```
<div id="app">

</div>
<script src="vue.js"></script>
<script src="axios.js"></script>
<script src="lodash.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            word: '',
            result: ''
        },
        watch: {//监听属性
            word: _.debounce(//减少后台压力请求，每5秒执行一次
                function (newV, oldV) {
                    console.log("新值：" + newV + "，旧值：" + oldV);
                    axios.get('****.php?word=' + newV).then(function (response) {//axios异步请求
                        console.log(response);
                        app.result = response.data;//注意此处不能用this.result，而应该用app.result
                    });
                }, 5)
        }
    });
</script>
```

## 指令

### 基本指令

#### v-bind: 绑定属性

> 可简写成冒号
>
> 能绑定任何属性

```
#绑定类属性
:class="'className'" #使用className类
:class="className" 使用data.className的值类
:class="{className1:true,className2:false}" #使用className1类，不使用使用className2类

#绑定样式属性
:style="{color:'red',fontSize:'14px'}" 
```

#### v-once:只绑定一次

>不会随着数据改变而改变

#### v-text:绑定元素内容

> 会原样实体输出内容

#### v-html:绑定元素HTML内容

> 会输出html内容

#### v-once:只绑定一次

#### v-if:如果条件

> dom对象完全移除

```
<!-- 需要挨在一起 -->
<span v-if="age<20">小孩</span>
<span v-else-if="age<30">青年</span>
<span v-else-if="age<50">壮年</span>
<span v-else>老年</span>
```

#### v-show:显示条件

> dom对象只是隐藏

```
<!-- 需要挨在一起 -->
<span v-show="age<20">小孩</span>
<span v-show="age<=20 && age<30">青年</span>
<span v-show="age<=30 && age<50">壮年</span>
<span v-show="age>=50">老年</span>
```

#### v-for:循环

使用in或者of都行

```
<!--操作索引数组（不带序号）-->
<tr v-for="v in list">
    <td>{{v}}</td><!--数值-->
</tr>
<!--操作索引数组（带序号）-->
<tr v-for="(v,k) in list">
    <td>{{k+1}}</td><!--序号-->
    <td>{{v}}</td><!--数值-->
</tr>
<!---------------------------------->
<!---------------------------------->
<!--操作关联数组（不带序号）-->
<tr v-for="(v,k) of list">
    <td>{{k}}</td><!--键值-->
    <td>{{v}}</td><!--数值-->
</tr>
<!--操作关联数组（带序号）-->
<tr v-for="(v,k,index) of list">
    <td>{{index}}</td><!--序号-->
    <td>{{k}}</td><!--键值-->
    <td>{{v}}</td><!--数值-->
</tr>
```

#### v-cloak:去除闪屏

```
<div id="app" v-cloak>

</div>
```

### 表单相关指令

#### v-model: 绑定表单项

```
<div id="app">
    <!-- 文本框 -->
    <div>
        <label>文本框：</label>
        <input type="text" v-model="textName">
    </div>
    <!-- 多行文本框 -->
    <div>
        <label>文本框：</label>
        <textarea v-model="textareaName"></textarea>
    </div>
    <!-- 单选按钮 -->
    <div>
        <label>单选按钮：</label>
        <div v-for="v in selectOptions">
            <input type="radio" v-model="radioName" :value="v.id">{{v.value}}
        </div>
    </div>
    <!-- 多选按钮 -->
    <div>
        <label>单个多选框：</label>
        <input type="checkbox" v-model="checkboxStatus"><!--选中为true，未选中为false-->
    </div>
    <div>
        <label>多个多选框：</label>
        <div v-for="v in selectOptions">
            <input type="checkbox" v-model="checkboxName" :value="v.id">{{v.value}}
        </div>
    </div>
    <!-- 单选下拉 -->
    <div>
        <label>单选下拉：</label>
        <select v-model="selectName">
            <option value=""></option>
            <option v-for="v in selectOptions" :value="v.id">{{v.value}}</option>
        </select>
    </div>
    <!-- 多选下拉 -->
    <div>
        <label>多选下拉：</label>
        <select v-model="selectMultipleName" multiple size="5">
            <option value="">请选择</option>
            <option v-for="v in selectOptions" :value="v.id">{{v.value}}</option>
        </select>
    </div>
</div>
<script src="vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',//管理的dom对象
        data: {//普通属性
            textName: '',
            textareaName: '',
            radioOptions:[
                {id:1,value:'v1'},
                {id:2,value:'v2'},
                {id:3,value:'v3'},
            ],
            radioName: '',
            checkboxOptions:[
                {id:1,value:'v1'},
                {id:2,value:'v2'},
                {id:3,value:'v3'},
            ],
            checkboxName: [],
            checkboxStatus: false,
            selectOptions:[
                {id:1,value:'v1'},
                {id:2,value:'v2'},
                {id:3,value:'v3'},
            ],
            selectName:'',
            selectMultipleName:[],
        }
    });
</script>
```

##### 表单修饰符

v-modle后面可以跟一些修饰符

> .number 让字符串数字变成整型数值

> .trim   忽略左右空格

> .lazy  懒更新  鼠标失去焦点时才更新数据

### 事件相关指令

#### v-on: 绑定事件

> 可简写成**@**符号
>
> 能绑定任何事件

##### 鼠标点击

```
@click #绑定点击事件
@dblclick #绑定双击事件
@click.left #绑定点击左键事件
@contextmenu.prevent #绑定点击右键事件并禁用默认右键事件
@click.middle #绑定点击中间键击事件
```

##### 表单提交事件

```
@submit #提交事件
@submit.prevent #提交事件并禁止默认提交行为
```

#### 事件修饰符

支持多个修饰符串联

##### 基本修饰符

> .prevent   禁止默认行为
>
> .stop   阻止冒泡
>
> .self  只点击本身才触发，冒泡过来的不触发
>
> .capture   默认向上冒泡，添加了此修饰符后则向下冒泡
>
> .once   只有效一次 #2.1.4 新增

其它

> .passive   #2.3.0 新增
>
> >Vue 还对应addEventListener中的passive选项提供了 .passive 修饰符
>
> ```
> <!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
> <!-- 而不会等待 `onScroll` 完成  -->
> <!-- 这其中包含 `event.preventDefault()` 的情况 -->
> <div @scroll.passive="onScroll">...</div>
> ```
>
> 这个 `.passive` 修饰符尤其能够提升移动端的性能。不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你*不*想阻止事件的默认行为。

##### 鼠标修饰符

> @click.left 左键
>
> @click.right 右键（因为右键默认有彩蛋，所以一般不用这个）
>
> @contextmenu.prevent 右键（一般用这个）
>
> @click.middle 中键

##### 键盘修饰符

> .13  支持任意ASCII码（13是回车，65是a）
>
> .enter  回车键
>
> .tab  切换键
>
> .delete  删除键
>
> .esc  退出键
>
> .space  空格键
>
> .ctrl   必须按住ctrl并结合其它任意键才能触发
>
> .shift   必须按住shift   并结合其它任意键才能触发
>
> .alt    必须按住alt并结合其它任意键才能触发

### 自定义指令

主要围绕下面这三个钩子触发事件

> bind：数据绑定到元素上时

> update：绑定数据更新时

> inserted：元素插入到父组件上时

只需要bind和update时并且过程一样时可用函数替代，在表单项上一般都是这样使用

#### 全局指令

```
<div id="app">
    <input type="text" v-model="color" v-color="color">
    <h1 :style="{color:color}">{{title}}</h1>
</div>
<script src="vue.js"></script>
<script>
    Vue.directive('color',{
        bind(el,bind){
            var color = bind.value ? bind.value : 'red';
            el.style.cssText = "color:" + color;
        },
        update(el,bind){
            var color = bind.value ? bind.value : 'red';
            el.style.cssText = "color:" + color;
        },
        inserted(el, bind){
            el.focus();
        }
    });
    new Vue({
        el: '#app',
        data: {
            title: 'php',
            color: 'red'
        }
    });
</script>
```

#### 局部指令

```
<div id="app">
    <span v-star="color">标题</span>
    <input type="text" v-model="color" v-focus>
    <h1 v-hide="false">php</h1>
</div>
<script src="vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            title: 'houdunren.com',
            color: 'red'
        },
        directives: {
            star(el, bind){//bind update 一起用的简写形式
                var color = bind.value ? bind.value : 'red';
                el.style.cssText = "color:" + color;
            },
            focus: {
                inserted(el, bind){
                    el.focus();
                }
            },
            hide(el, bind){//bind update 一起用的简写形式
                console.log(bind);
                if (bind.value) {
                    el.style.cssText = "display:none";
                }
            }
        }
    });
</script>
```

## 组件

>用new Vue()构建的可视为顶部组件，并且只能有一个

> 子组件的模板必须有个顶级标签

### 全局组件

```
<div id="app">
    <dj-slide></dj-slide>
    <dj-slide></dj-slide>
</div>
<script src="vue.js"></script>
<script>
    Vue.component('djSlide', {
        template: '<h1>sclzdj</h1>' //模板
    });
    new Vue({
        el: '#app'
    });
</script>
```

### 局部组件

```
<div id="app">
    <dj-news></dj-news>
    <dj-news></dj-news>
</div>
<script src="vue.js"></script>
<script>
    var djNews = {
        template: "<h2>sclzdj</h2>",//模板
    };
    new Vue({
        el: '#app',
        components: {djNews:djNews}  //组件
    });
</script>
```

#### 构建

```
<div id="app">
    <dj-news></dj-news>
    <dj-news></dj-news>
</div>
<script type="text/x-template" id="djNews">
    <div>
        <li v-for="(v,k) in news">
            {{v.title}}
            <button @click="del(k)">删除</button>
        </li>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djNews = {
        template: "#djNews",//模板
        data:function(){//普通属性
            return {
                news: [
                    {title: 'cms'},
                    {title: 'php'}
                ]
            };
        },
        methods: {
            del(k){
                this.news.splice(k, 1);
            }
        }
    };
    new Vue({
        el: '#app',
        components: {djNews:djNews} //组件
    });
</script>
```

#### 传递参数

> **不加冒号则是字符串**

> **加冒号则是表达式，并且可以传递父组件的data属性**

```
<div id="app">
    <dj-news prop1="sclzdj" :prop2="true" :prop3="name"></dj-news>
</div>
<script type="text/x-template" id="djNews">
    <div>
        <h1>{{prop1}}</h1>
        <h1 v-show="prop2">prop2</h1>
        <h1>{{prop3}}</h1>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djNews = {
        template: "#djNews",//模板
        props:['prop1','prop2','prop3'],//参数
        data:function(){//普通属性
            return {
            
            };
        }
        
    };
    new Vue({
        el: '#app',
        components: {djNews:djNews}, //组件
        data:{//普通属性
            name:'dujun'
        }
    });
</script>
```

#### 参数验证

```
props: {
    demo: {//参数demo
        type: [String,Number,Boolean,Function,Object,Array],//类型
        required: true,//是否必须，默认false
        default:'',//默认值
        validator(v){//验证器
            return v===1 || v===0;
        }
    },
    list: {//参数list，示例
        type: Array,//规定只能是数组
        default(){//赋了一个默认值
            return [{title: 'php'}];
        }
    },
    status: {//参数lstatus，示例
        type: [Number,Boolean],//规定只能是数字或者布尔值
        default:true  //默认为true
    }
}
```

#### 参数修饰符

##### .sysn

> 同步父子组件的属性  #注意：有些版本淘汰了该属性

```
<div id="app">
    <!--syns同步父子组件的list和goods数据-->
    <dj-goods :list.syns="goods" @sum="sum()"></dj-goods>
    总价：{{totalPrice}}
</div>
<script type="text/x-template" id="djGoods">
    <div>
        <table border="1">
            <tr v-for="v in list">
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
                <td><input type="text" v-model="v.num"></td>
            </tr>
        </table>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djGoods = {
        template: "#djGoods",//模板
        props:['list'],
        data:function(){//普通属性
            return {

            };
        }
    };
    var app = new Vue({
        el: '#app',//管理的dom对象
        components: {djGoods}, //组件
        data: {//普通属性
            goods:[
                {title:'php',num:2,price:100},
                {title:'js',num:2,price:200},
                {title:'sql',num:2,price:300}
            ]
        },
        computed:{
            totalPrice(){
                var totalPrice=0;
                this.goods.forEach((v)=>{
                    totalPrice+=v.num*v.price;
                });
                return totalPrice;
            }
        }
    });
</script>
```

#### $emit用法

> 触发组件自定义方法

```
<div id="app">
    <dj-goods :list="goods" @sum="sum()"></dj-goods><!--@sum就是自定义方法-->
    总价：{{totalPrice}}
</div>
<script type="text/x-template" id="djGoods">
    <div>
        <table border="1">
            <tr v-for="v in list">
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
                <td><input type="text" v-model="v.num" @keyup="total()"></td>
            </tr>
        </table>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djGoods = {
        template: "#djGoods",//模板
        props:['list'],
        data:function(){//普通属性
            return {

            };
        },
        methods: {
            total(){
                this.$emit('sum');////重点用法，这里触发//////////
            }
        }
    };
    var app = new Vue({
        el: '#app',//管理的dom对象
        components: {djGoods}, //组件
        data: {//普通属性
            goods:[
                {title:'php',num:2,price:100},
                {title:'js',num:2,price:200},
                {title:'sql',num:2,price:300}
            ],
            totalPrice:0
        },
        methods: {//方法
            sum(){
                this.totalPrice=0;
                this.goods.forEach((v)=>{
                    this.totalPrice+=v.num*v.price;
                });
            }
        },
        mounted(){
            this.sum();
        }
    });
</script>
```

#### 内容分发slot

##### 单个

```
<div id="app">
    <!--为空就显示默认内容-->
    <dj-card>实际内容</dj-card>
</div>
<script type="text/x-template" id="djCard">
    <div>
       <slot>默认内容</slot>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djCard = {
        template: "#djCard",//模板
        data:function(){//普通属性
            return {

            };
        }
    };
    var app = new Vue({
        el: '#app',//管理的dom对象
        components: {djCard}, //组件
        data: {//普通属性

        }
    });
</script>
```

##### 多个

```
<div id="app">
    <dj-card>
        <h4 slot="top">实际内容top</h4>
        <p slot="moddle">实际内容moddle</p>
        <i slot="bottom">实际内容bottom</i>
        <!--只要没有slot命名的都显示在其它内容内面-->
        <div>其它实际内容1</div>
        <span>其它实际内容2</span>
    </dj-card>
</div>
<script type="text/x-template" id="djCard">
    <div>
        <!--只要有name命名的都不能有默认内容-->
        <slot name="top"></slot>
        <slot name="moddle"></slot>
        <slot name="bottom"></slot>
        <slot>其它默认内容</slot>
    </div>
</script>
<script src="vue.js"></script>
<script>
    var djCard = {
        template: "#djCard",//模板
        data:function(){//普通属性
            return {

            };
        }
    };
    var app = new Vue({
        el: '#app',//管理的dom对象
        components: {djCard}, //组件
        data: {//普通属性

        }
    });
</script>
```

##### bootstrap表单面板

```
<link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
<div id="app">
    <form action="" class="form-horizontal">
        <bs-panel>
            <h4 slot="title">PHP讨论</h4>
            <bs-input title="标题" value="thinkphp" slot="body"></bs-input>
            <bs-input title="点击数" value="100" slot="body"></bs-input>
            其它内容
        </bs-panel>
    </form>
</div>
<script type="text/x-template" id="bsPanel">
    <div class="panel panel-default">
        <div class="panel-heading">
             <slot name="title"></slot>
        </div>
        <div class="panel-body">
            <slot name="body"></slot>
        </div>
        <div>
            <slot></slot>
        </div>
    </div>
</script>
<script type="text/x-template" id="bsInput">
    <div class="form-group">
        <label for="" class="col-sm-2 control-label">{{title}}</label>
        <div class="col-sm-10">
            <input type="text" class="form-control" :value="value">
        </div>
    </div>
</script>
<script>
    var bsPanel = {
        template: "#bsPanel"
    };
    var bsInput = {
        template: "#bsInput",
        props: ['title', 'value']
    };
    new Vue({
        el: '#app',
        components: {bsPanel, bsInput}
    });
</script>
```

##### 父级组件中定义子组件的模板结构scope

简单用法示例

```
<div id="app">
    <dj-list>
        <!--会显示这个结果 { "title": "标题", "price": "价格" }-->
        <template scope="attr">
            {{attr}}
        </template>
    </dj-list>
</div>
<script type="text/x-template" id="djList">
    <ul>
        <slot title="标题" price="价格"></slot>
    </ul>
</script>
<script src="vue.js"></script>
<script>
    var djList = {
        template: "#djList"
    };
    new Vue({
        el: '#app',
        components: {djList},
        data:{

        }
    });
</script>
```

经典用法示例

```
<div id="app">
    <dj-list :data="news">
        <template scope="v">
            <li>
                <h1>{{v.field.title}}</h1>
            </li>
        </template>
    </dj-list>
</div>
<script type="text/x-template" id="djList">
    <ul>
        <slot v-for="v in data" :field="v"></slot>
    </ul>
</script>
<script src="vue.js"></script>
<script>
    var djList = {
        template: "#djList",
        props:['data']
    };
    new Vue({
        el: '#app',
        components: {djList},
        data:{
            news:[
                {title:'php'},
                {title:'js'},
                {title:'mysql'}
            ]
        }
    });
</script>
```

#### 动态设置组件 :is

示例：

```
<div id="app">
    <!--下面是:is的使用方法-->
    <div :is="formType"></div>
    <input type="radio" v-model="formType" value="hdInput"> 文本框
    <input type="radio" v-model="formType" value="hdTextarea"> 文本域
</div>
<script src="vue.js"></script>
<script>
    var hdInput = {
        template: "<div><input/></div>",
    };
    var hdTextarea = {
        template: "<div><textarea></textarea></div>",
    };
    new Vue({
        el: '#app',
        components: {hdInput,hdTextarea},
        data:{
            formType:"hdTextarea"
        }
    });
</script>
</body>
```

## 显隐过渡动画

可以用transition标签包住任何东西控制过渡动画，包括组件和路由视图

### 自己定义写样式

简单例子

```
<div id="app">
    <button @click="type=!type">切换</button>
    <transition name="dj">
        <h1 v-if="type">内容......</h1>
    </transition>
</div>
<script>
    new Vue({
        el: '#app',
        data:{
            type:false
        }
    });
</script>
<script src="vue.js"></script>
<style>
    /*类前缀dj对应的是transition的name属性值*/
    /*enter：显示开始时*/
    /*enter-active：显示过程中*/
    /*enter-to：显示结束时*/
    /*leave：隐藏开始时*/
    /*leave-active：隐藏过程中*/
    /*leave-to：隐藏结束时*/
    .dj-enter,.dj-leave-to{
        opacity: 0;
    }
    .dj-enter-active,.dj-leave-active{
        transition: all 5s;
        color:red;
    }
    .dj-enter-to,.dj-leave{

    }
</style>
```

css扩展例子

```
<div id="app">
    <button @click="type=!type">切换</button>
    <transition name="dj">
        <h1 v-if="type">内容......</h1>
    </transition>
</div>
<script src="vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data:{
            type:false
        }
    });
</script>
<style>
    .dj-enter-active{
        animation: show-in 1s;
        transition: all 1s;
    }
    .dj-leave-active{
        animation: hide-out 1s;
        transition: all 1s;
    }
    .dj-enter,.dj-leave-to{
        opacity: 0;
    }
    /*关键帧*/
    @keyframes show-in {
        0%{
            transform: translate(200px,0);
        }
        100%{
            transform: translate(0,0);
        }
    }
    @keyframes hide-out {
        0%{
            transform: translate(0,0);
        }
        100%{
            transform: translate(200px,0);
        }
    }
</style>
```

### 利用第三方库

#### css (推荐)

使用animate.css示例

```
<link rel="stylesheet" href="animate.css">
<div id="app">
    <button @click="type=!type">切换</button>
    <!--enter：显示开始时-->
    <!--enter-active：显示过程中-->
    <!--enter-to：显示结束时-->
    <!--leave：隐藏开始时-->
    <!--leave-active：隐藏过程中-->
    <!--leave-to：隐藏结束时-->
    <!--后面跟上后缀 -class  -->
    <transition enter-active-class="animated slideInDown" leave-active-class="animated zoomOut">
        <h1 v-if="type">内容......</h1>
    </transition>
</div>
<script src="vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data:{
            type:false
        }
    });
</script>
```

#### js

使用velocity.css示例

```
<div id="app">
    <button @click="type=!type">切换</button>
    <!--before-enter：显示开始时-->
    <!--enter：显示过程中-->
    <!--after-enter：显示结束时-->
    <!--before-leave：隐藏开始时-->
    <!--leave：隐藏过程中-->
    <!--after-leave：隐藏结束时-->
    <!-- :css="false"是不使用css定义的动画效果 -->
    <transition @before-enter="beforeEnter" @enter="enter" @leave="leave" :css="false">
        <h1 v-if="type">内容......</h1>
    </transition>
</div>
<script src="vue.js"></script>
<script src="velocity.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            type: false
        },
        methods: {
            beforeEnter(el){
                el.style.opacity=0;
            },
            enter(el,done){//动画执行完成是必须执行done函数
                Velocity(el,{opacity:1},{ duration: 2000 ,complete:done})
            },
            leave(el,done){//动画执行完成是必须执行done函数
                Velocity(el,{opacity:0,rotateZ:'-360deg'},{ duration: 2000 ,complete:done})
            }
        }
    });
</script>
```

## 变异方法

可以用下面的方法操作data普通属性数据

> this.list.push()     数组尾部追加元素
>
> this.list.pop()         数组尾部移除元素
>
> this.list.unshift()   数组头部追加元素
>
> this.list.shift()        数组头部移除元素
>
> this.list.splice({offset},{length})    数组删除元素
>
> this.list.sort(function(a,b){return a > b;})  数组排序元素
>
> this.list.reverse()   数组翻转元素
>
> this.list.filter(function(item){return true;}) 数组筛选元素

```
<div id="app">
    <li v-for="(v,k) in comments">
        {{v.id}} - {{v.content}}
        <button v-on:click="remove(k)">删除</button>
    </li>
    <textarea v-model="current_content" cols="30" rows="10"></textarea><br>
    <button v-on:click="push('end')">发表到后面</button>
    <button v-on:click="push('pre')">发表到前面</button>
    <br>
    <button v-on:click="del('last')">删除最后一条评论</button>
    <button v-on:click="del('first')">删除第一条评论</button>
    <br>
    <button v-on:click="sort()">按照编号排序</button>
    <button v-on:click="reverse()">反转顺序</button>
    <br>
    <input type="text" v-on:keyup.enter="search" v-model="search_content">
</div>
<script src="vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            //搜索内容
            search_content:'',
            //当前用户输入内容
            current_content: '',
            comments: [
                {id: 2, content: 'PHP'},
                {id: 4, content: 'CMS'},
                {id: 1, content: '学生'},
                {id: 3, content: '老师'},
            ]
        },
        methods: {
            search(){
                this.comments = this.comments.filter((item)=>{//筛选
                    var reg  = new RegExp(this.search_content,'i');
                    return reg.test(item.content);
                })
            },
            sort(){
                this.comments.sort(function (a, b) {//排序
                    return a.id > b.id;
                })
            },
            reverse(){
                this.comments.reverse();//翻转
            },
            remove(k){
                this.comments.splice(k, 1);//删除
            },
            push(type){
                var id = this.comments.length+1;
                var content = {id:id,content: this.current_content}
                switch (type) {
                    case 'end':
                        this.comments.push(content);//尾部添加
                        break;
                    case 'pre':
                        this.comments.unshift(content);//头部添加
                        break;
                }
                this.current_content = '';
            },
            del(type){
                switch (type) {
                    case 'last':
                        this.comments.pop();//尾部删除
                        break;
                    case 'first':
                        this.comments.shift();//头部删除
                        break;
                }
            }
        }
    });
</script>
```

## 状态管理模式

### vuex

https://vuex.vuejs.org/zh/

数据在各组件中同步共享

#### 简单使用

##### state

仓库数据

```
<div id="app">
    <lists></lists>
</div>
<script type="text/x-template" id="lists">
    <div>
        <table border="1" width="90%">
            <tr><th>编号</th><th>名称</th><th>价格</th></tr>
            <tr v-for="v in goods">
                <td>{{v.id}}</td>
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
            </tr>
        </table>
    </div>
</script>
<script src="vue.js"></script>
<script src="vuex.js"></script>
<script>
    let lists = {
        template: '#lists',
        computed: {
            goods(){
                return this.$store.state.goods;//state数据调用方式
            }
        }
    };
    let store = new Vuex.Store({//定义vuex
        state: {//仓库数据
            goods: [
                {id: 1, title: 'iphone7Plus', price: 100},
                {id: 2, title: 'cms系统', price: 200}
            ]
        }
    });
    var app = new Vue({
        el: '#app',
        store,//要把vuex注入进来
        components: {lists}
    })
</script>
```

##### getters

计算仓库数据

```
<div id="app">
    <lists></lists>
    <footer-cart></footer-cart>
</div>
<script type="text/x-template" id="lists">
    <div>
        <table border="1" width="90%">
            <tr><th>编号</th><th>名称</th><th>价格</th><th>数量</th><th>小计</th></tr>
            <tr v-for="v in cart">
                <td>{{v.id}}</td>
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
                <td><input type="text" v-model="v.num"></td>
                <td>{{v.totalPrice}}</td>
            </tr>
        </table>
    </div>
</script>
<script type="text/x-template" id="footerCart">
    <div>
        <h4>总价：{{totalPrice}}</h4>
    </div>
</script>
<script src="vue.js"></script>
<script src="vuex.js"></script>
<script>
    let lists = {
        template: '#lists',
        computed: {
            cart(){
                return this.$store.getters.cart;//getters数据调用方式
            }
        }
    };
    let footerCart = {
        template: '#footerCart',
        computed: {
            totalPrice(){
                return this.$store.getters.totalPrice;//getters数据调用方式
            }
        }
    };
    let store = new Vuex.Store({//定义vuex
        state: {//仓库数据
            cart: [
                {id: 1, title: 'iphone7Plus', price: 100,num:1},
                {id: 2, title: 'cms系统', price: 200,num: 2}
            ]
        },
        getters: {//计算仓库数据
            totalPrice(state){
                var sum=0;
                state.cart.forEach((v)=>{
                    sum+=v.price*v.num;
                });
                return sum;
            },
            cart(state){
                state.cart.forEach((v)=>{
                    v.totalPrice=v.price*v.num;
                });
                return state.cart;
            }
        }
    });
    var app = new Vue({
        el: '#app',
        store,//要把vuex注入进来
        components: {lists,footerCart}
    })
</script>
```

##### mutations

修改仓库数据的方法

```
<div id="app">
    <lists></lists>
    <footer-cart></footer-cart>
</div>
<script type="text/x-template" id="lists">
    <div>
        <table border="1" width="90%">
            <tr><th>编号</th><th>名称</th><th>价格</th><th>数量</th><th>小计</th><th>操作</th></tr>
            <tr v-for="(v,k) in cart">
                <td>{{v.id}}</td>
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
                <td><input type="text" v-model="v.num"></td>
                <td>{{v.totalPrice}}</td>
                <td><button type="button" @click="del(k)">删除</button></td>
            </tr>
        </table>
    </div>
</script>
<script type="text/x-template" id="footerCart">
    <div>
        <h4>总价：{{totalPrice}}</h4>
    </div>
</script>
<script src="vue.js"></script>
<script src="vuex.js"></script>
<script>
    let lists = {
        template: '#lists',
        computed: {
            cart(){
                return this.$store.getters.cart;
            }
        },
        methods:{
            del(k){
                this.$store.commit('del',{k});//mutations方法调用方式
            }
        }
    };
    let footerCart = {
        template: '#footerCart',
        computed: {
            totalPrice(){
                return this.$store.getters.totalPrice;
            }
        }
    };
    let store = new Vuex.Store({//定义vuex
        state: {//仓库数据
            cart: [
                {id: 1, title: 'iphone7Plus', price: 100,num:1},
                {id: 2, title: 'cms系统', price: 200,num: 2}
            ]
        },
        getters: {//计算仓库数据
            totalPrice(state){
                var sum=0;
                state.cart.forEach((v)=>{
                    sum+=v.price*v.num;
                });
                return sum;
            },
            cart(state){
                state.cart.forEach((v)=>{
                    v.totalPrice=v.price*v.num;
                });
                return state.cart;
            }
        },
        mutations: {//修改仓库数据
            del(state,param){//param为传递的参数
                state.cart.splice(param.k,1);
            }
        }
    });
    var app = new Vue({
        el: '#app',
        store,//要把vuex注入进来
        components: {lists,footerCart}
    })
</script>
```

##### actions

ajax加载数据

```
<div id="app">
    <lists></lists>
</div>
<script type="text/x-template" id="lists">
    <div>
        <table border="1" width="90%">
            <tr><th>编号</th><th>名称</th><th>价格</th></tr>
            <tr v-for="v in goods">
                <td>{{v.id}}</td>
                <td>{{v.title}}</td>
                <td>{{v.price}}</td>
            </tr>
        </table>
    </div>
</script>
<script src="vue.js"></script>
<script src="vuex.js"></script>
<script src="axios.min.js"></script>
<script>
    let lists = {
        template: '#lists',
        computed: {
            goods(){
                return this.$store.state.goods;
            }
        }
    };
    let store = new Vuex.Store({//定义vuex
        state: {//仓库数据
            goods: []
        },
        mutations: {
            setGoods(state,param){
                state.goods=param.goods;
            }
        },
        actions: {//ajax加载数据
            getGoods(store){//要把store注入进来
                axios.get('http://localhost/73.php').then(function (response) {
                    store.commit('setGoods',{goods:response.data});
                }).catch(function (error) {
                    console.log(error);//ajax请求出错
                });
            }
        }
    });
    var app = new Vue({
        el: '#app',
        store,//要把vuex注入进来
        components: {lists},
        mounted(){
            this.$store.dispatch('getGoods');//actions方法调用方法
        }
    })
</script>
```

#### vuex模块化

> namespaced 默认为false
>
> 当为false时，state是模块局部的，getters、mutations、actions是全局的
>
> 当为true时，state、getters、mutations、actions都是全局的
>
> 全局调用方式和非模块化开发一样
>
> state 模块局部调用方法  this.$store.state.cart.goods
>
> getters 模块局部调用方法  this.$store.getters['cart/goods']
>
> mutations 模块局部调用方法  this.$store.commit('cart/del',{param});
>
> actions 模块局部调用方法  this.$store.dispatch('cart/getGoods');

```
const cartModule = {
	namespaced:true,
    state:{},
    getters:{},
    mutations:{},
    actions:{}
}

const newsModule = {
	namespaced:true,
    state:{},
    getters:{},
    mutations:{},
    actions:{}
}

let store = new Vuex.Store({
	modules:{
		cart:cartModule,
		news:newsModule
	}
});
```

## 单页面路由

### vue-router

https://router.vuejs.org/zh/

#### 简单使用

```
<div id="app">
    <router-link to="/php">PHP</router-link>
    <router-link to="/cms">CMS</router-link>
    <router-view></router-view>
</div>
<script src="vue.js"></script>
<script src="vue-router.js"></script>
<script>
    const php={
        template:'<h1>php</h1>'
    };
    const cms={
        template:'<h1>cms</h1>'
    };
    //要把组件交给路由器
    let router = new VueRouter({
        routes:[
            {path:'/php',component:php},
            {path:'/cms',component:cms}
        ]
    });
    var app=new Vue({
        el: '#app',
        router
    });
</script>
```

#### 参数

>路由path里面加 : 则代表是参数

> 参数存放在$route.params里面，使用方式和普通属性基本一致

```
<div id="app">
    <router-link to="/content">内容</router-link>
    <router-view></router-view>
</div>
<script type="text/x-template" id="content">
    <div>
        cid:{{$route.params.cid}} - id:{{$route.params.id}}
        <br>
        <button @click="show">检测参数</button>
    </div>
</script>
<script src="vue.js"></script>
<script src="vue-router.js"></script>
<script>
    const content={
        template:'#content',
        methods:{
            show(){
                console.log(this.$route.params);
                console.log(this.$route.params.cid);
                console.log(this.$route.params.id);
            }
        }
    };
    let router = new VueRouter({
        routes:[
            {path:'/content/:cid/:id.html',component:content}
        ]
    });
    var app=new Vue({
        el: '#app',
        router
    });
</script>
```

#### 参数限制

> 参数后面跟一个小括号，里面填写限制的正则表达式即可

```
{path:'/content/:id(\\d{2})',component:content}
```

> 后面跟问号则表示该参数可有可无

```
{path:'/content/:id?',component:content}
```

```
<!--利用这个特性我们可以为参数设置默认值-->

<div id="app">
    <router-link to="/content">内容</router-link>
    <router-view></router-view>
</div>
<script type="text/x-template" id="content">
    <div>
        id:{{id}}
    </div>
</script>
<script>
    const content = {
        template: '#content',
        data() {
            return {
                id: 0
            };
        },
        mounted() {
            this.id = this.$route.params.id;
            if (!this.id) {
                this.id = 1;//设置参数id默认值为1
            }
        }
    };
    let router = new VueRouter({
        routes: [
            {path: '/content/:id?', component: content},
        ]
    });
    var app=new Vue({
        el: '#app',
        router
    });
</script>
```

#### 名称

地址栏依然显示的是path

```
<li v-for="v in news">
	<router-link :to="{name:'content',params:{id:v.id}}">
		{{v.title}}
	</router-link>
</li>

{path: '/content/:id', component: content,name:'content'}
```

#### 嵌套

父路由对应的组件中添加

```
<router-view></router-view>
```

子路由写进父路由的children中

```
{path: '/list', component: list, name: 'list', children:[
	{path: '/content/:id', component: content,name: 'content'}
]}
```

解决子路由组件的mounted挂载方法只执行一次的问题

```
//在子路由组件中操作
//把子路由组件的原mounted挂载方法定义成普通方法，然后在挂载方法和监听路由中都执行

watch:{
    '$route'(to,from){//监听路由
        this.load();
    }
},
mounted(){
    this.load();
},
methods:{
    load(){//原本要执行的挂载内容
			
    }
}
```

#### js控制路由跳转

```
<li v-for="v in news">
	<a href="" @click.prevent="go(v.id)">{{v.title}}</a>
</li>

//注意go方法应该写在对应的组件的methods中去
methods:{
    go(id){
        //var url = '/content/'+id;
        //var url = {path:'/content/'+id};
        var url = {name:'content',params:{id:id}};
        this.$router.push(url);//会生成历史记录，一般用这个
        //this.$router.replace(url);//不会生成历史记录
    }
}
```

#### 一路由多组件

```
<style>
.menu{border:solid 2px #999;padding:10px;display: block;}
.news{float:left;border:solid 1px red;padding:20px;width:300px;}
.slide{float:left;border:solid 1px red;padding:20px;width:300px;}
</style>
<div id="app">
    <router-view class="menu"></router-view>
    <router-view class="news" name="news"></router-view>
    <router-view class="slide" name="slide"></router-view>
</div>
<script type="text/x-template" id="menu">
    <div>
        <a href="http://php.com">php</a>
        <a href="http://www.mysql.com">mysql</a>
    </div>
</script>
<script type="text/x-template" id="news">
    <div>
        <li v-for="v in news">{{v.title}}</li>
    </div>
</script>
<script type="text/x-template" id="slide">
    <div>
        <li v-for="v in data">{{v.title}}</li>
    </div>
</script>
<script src="vue.js"></script>
<script src="vue-router.js"></script>
<script>
    const menu = {
        template: '#menu',
    };
    const news = {
        template: '#news',
        data(){
            return {
                news:[
                    {title:'baidu.com'},
                    {title:'souhu.com'},
                ]
            }
        }
    };
    const slide = {
        template: '#slide',
        data(){
            return {
                data:[
                    {title:'百度'},
                    {title:'搜狐'},
                ]
            }
        }
    };
    let router = new VueRouter({
        routes: [
            {path: '/', components: {
                default: menu,//default代表未命名的视图
                news: news,
                slide: slide
            }}
        ]
    });
    var app=new Vue({
        el: '#app',
        router
    });
</script>
```

#### 重定向

```
{path: '/content/:id', component: content, name: 'content'},
{path: '/about', redirect:{name:'content',params:{id:3}}}  //重定向到/content/3
```

#### 别名

地址栏会显示别名而不会显示path，但是实际走的是path

```
{path: '/content/:id', component: content, name: 'content'},
{path: '/content/2', alias:['/about','/we']}, 
{path: '/content/3', alias:['/contact']}
```

#### 去除锚点#

必须在正式项目中的index.html文件使用才完全有效，否则有些页面会有问题

```
let router = new VueRouter({
	mode:'history',//去除锚点#
	routes
});
```

#### 404路由

可以理解为404页面

```
{path: '/', component: home},
{path: '/content/:id.html', component: content,name:'content'},
{path: '*', component: NotFound}  //最后跟上这个就可以实现
```

## 调试工具

### vue-devtools

https://github.com/vuejs/vue-devtools

## 脚手架

### vue-cli

https://cli.vuejs.org/zh/

#### v3

##### 基本使用

打包编译后必须把项目部署到服务器上才能使用

```
#安装
cnpm install -g @vue/cli
#查看版本号
vue --version

#安装全局扩展
#cnpm install -g @vue/cli-service-global
#vue serve  #启动快速原型
#vue build  #打包编译快速原型

#创建一个项目 
vue create --help #c查看创建帮助
vue create {project-name}

#UI基面创建和管理项目
#vue ui

#进入项目目录
cd {project-dir}

#开启项目服务
npm run server
#打包编译项目
npm run build
```

##### less和scoped

```
<style scoped lang="less">
/***scope代表只在当前组件内有效***/
/***lang="less"代表使用less语法***/

</style>

#进入项目目录
cd {project-dir}
######必须先结束项目服务（ctrl+c）#######
#安装less-loader
cnpm install less-loader --save-dev
#安装less
cnpm install less --save-dev
#重新开启项目服务
npm run server
```

##### 配置

在项目跟目录下创建配置文件vue.config.js

> 修改端口

```
module.exports = {
    devServer: {
        port: 8088,//端口号
    }
}
```

> 其它配置

```

module.exports = {
 // 基本路径
 baseUrl: '/',
 // 输出文件目录
 outputDir: 'dist',
 // eslint-loader 是否在保存的时候检查
 lintOnSave: true,
 // use the full build with in-browser compiler?
 // https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only
 compiler: false,
 // webpack配置
 // see https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md
 chainWebpack: () => {},
 configureWebpack: () => {},
 // vue-loader 配置项
 // https://vue-loader.vuejs.org/en/options.html
 vueLoader: {},
 // 生产环境是否生成 sourceMap 文件
 productionSourceMap: true,
 // css相关配置
 css: {
  // 是否使用css分离插件 ExtractTextPlugin
  extract: true,
  // 开启 CSS source maps?
  sourceMap: false,
  // css预设器配置项
  loaderOptions: {},
  // 启用 CSS modules for all css / pre-processor files.
  modules: false
 },
 // use thread-loader for babel & TS in production build
 // enabled by default if the machine has more than 1 cores
 parallel: require('os').cpus().length > 1,
 // 是否启用dll
 // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#dll-mode
 dll: false,
 // PWA 插件相关配置
 // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
 pwa: {},
 // webpack-dev-server 相关配置
 devServer: {
  open: process.platform === 'darwin',
  host: '0.0.0.0',
  port: 8080,
  https: false,
  hotOnly: false,
  proxy: null, // 设置代理
  before: app => {}
 },
 // 第三方插件配置
 pluginOptions: {
  // ...
 }
}
```

#### v2

https://github.com/vuejs/vue-cli/tree/v2

##### 基本使用

```
#安装
cnpm install -g vue-cli
#创建项目
#vue init <template-name> <project-name>
vue init webpack my-project
```

## 细节问题

### 表单项name相同时用key区分

```
<!-- 示例 -->
<div id="hdcms">
    <template v-if="regType=='mail'">
        邮箱: <input type="text" name="username" key="mail-register">
    </template>
    <template v-else>
        手机: <input type="text" name="username" key="phone-register">
    </template>
    <br>
    <input type="radio" value="mail" v-model="regType"> 邮箱注册
    <input type="radio" value="phone" v-model="regType"> 手机注册
</div>

```

### this作用域

```
var app = new Vue({
    el: '#hdcms',
    computed:{
      stus(){
          if(this.type=='all'){
              return this.user;
          }else{
              /*错误示例*/
              // return this.user.filter(function(v){
              //     return v.sex ==this.type;//此时this已不再是app对象
              // });
              /*正确示例*/
              //第一种方法：调用app对象
              return this.user.filter(function(v){
                  return v.sex ==app.type;
              });
              //第二种方法：使用es6语法
              // return this.user.filter((v)=>{
              //     return v.sex ==this.type;
              // });
          }
      }
    },
    data: {
        type:'all',
        user:[
            {name:'小明',sex:'boy'},
            {name:'小强',sex:'boy'},
            {name:'小丽',sex:'girl'},
            {name:'小梅',sex:'girl'}
        ]
    }
});
```

## 其它

### axios简单ajax请求

```
<!-- 简单使用 -->
<script src="axios.js"></script>
<script>
axios.get('****.php).then(function (response) {
    console.log(response);
}.catch(function(error){
    console.log(error);
}));   
</script>
```

### lodash后台请求减压

```
<script src="lodash.js"></script>
<script>
	_.debounce(function () {}, 5);//减少后台压力请求，每5秒执行一次
</script>
```

