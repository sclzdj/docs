# CSS3

## 选择器

```
element>element    div>p  选择父元素为 <div> 元素的所有 <p> 元素。
element1~element2  p~ul   选择前面有 p>元素的每个 ul 元素。
element+element    div+p  选择紧接在 <div> 元素之后的所有 <p> 元素。
```

#### 属性选择器

```
[attribute] [target]               选择带有 target 属性所有元素。
[attribute=value] [target=_blank]  选择 target="_blank" 的所有元素。
[attribute^=value] a[src^="https"] 选择其 src 属性值以 "https" 开头的每个 a 元素
[attribute$=value] a[src$=".pdf"]  选择其 src 属性以 ".pdf" 结尾的所有 a 元素
[attribute*=value] a[src*="abc"]   选择其 src 属性中包含 "abc" 子串的每个 a 元素
```

#### 伪类选择器

```
:root :root               选择文档的根元素。
:empty p:empty            选择没有子元素的每个p元素（包括文本节点）
:target #news:target      选择当前活动的 #news 元素。
::selection ::selection   选择被用户选取的元素部分。
:not(selector) :not(p)    选择非 p元素的每个元素。
```

#### 文本选择器

```
:first-letter p:first-letter 选择每个 <p> 元素的首字母。
:first-line p:first-line     选择每个 <p> 元素的首行。
:before p:before             在每个 <p> 元素的内容之前插入内容。
:after p:after               在每个 <p> 元素的内容之后插入内容。
```

#### 表单选择器

```
:focus input:focus       选择获得焦点的 input 元素。
:enabled input:enabled   选择每个启用的 <input> 元素。
:disabled input:disabled 选择每个禁用的 <input> 元素
:checked input:checked   选择每个被选中的 <input> 元素。
```

#### 子选择器

```
:first-of-type p:first-of-type 选择属于其父元素的首个p元素的每个p元素
:last-of-type p:last-of-type 选择属于其父元素的最后p元素的每个p元素
:only-of-type p:only-of-type 选择属于其父元素唯一的p元素的每个p元素
:only-child p:only-child 选择属于其父元素的唯一子元素的每个p元素
:nth-child(n) p:nth-child(2) 选择属于其父元素的第二个子元素的每个p元素
:nth-last-child(n) p:nth-last-child(2) 同上，从最后一个子元素开始计数
:nth-of-type(n) p:nth-of-type(2) 选择属于其父元素第二个元素的每个p元素
:nth-last-of-type(n) p:nth-last-of-type(2) 同上，但是从最后一个子元素开始计数
:last-child p:last-child 选择属于其父元素最后一个子元素每个p元素
:first-child p:first-child 选择属于父元素的第一个子元素的每个 p元素
```

## 文本样式

#### 文字大小

**px:**
使用具体像素点为单位，好处是比较稳定和精确。但在浏览器中放大或缩放浏览
页面时会存在一个问题
**em:**
em是以父级大小为参考的单位。好处是字体可以自适应。但父元素标签发生改
变时字体大小将不确定。
**rem：**
rem是相对于根元素<html>，这样就意味着，我们只需要在根元素确定一个参
考值即可。示例：

```
html {font-size: 62.5%;/*10 ÷ 16 × 100% = 62.5%*/}
body {font-size: 1.4rem;/*1.4 × 10px = 14px */}
h1 { font-size: 2.4rem;/*2.4 × 10px = 24px*/}
```

#### 文字不换行

```

```

#### 文本阴影

语法：text-shadow: h-shadow v-shadow blur color

说明：

- h-shadow 水平阴影的位置。允许负值
- v-shadow 垂直阴影的位置。允许负值
- blur 模糊距离
- color 阴影的颜色

#### 文本分栏

- column-width   栏宽

- column-count   列数

- column-gap      列间距

- column-rule      分隔线

  说明：-moz是针对firefox的设置，-webkit是针对chrome与苹果safari的设置

## 盒子样式

#### 盒子阴影

语法：box-shadow: h-shadow v-shadow blur spread color

- h-shadow 水平阴影的位置。允许负值
- v-shadow 垂直阴影的位置。允许负值
- blur 模糊距离
- spread 阴影的尺寸
- color 阴影的颜色

#### 盒子尺寸

- min-width 元素最小可接受的宽度
- max-width 元素最大可接受的宽度
- min-height 元素最小可接受的高度
- max-height 元素最大可接受的高度

#### 盒子空间

属性允许您以特定的方式定义匹配某个区域的特定元素

box-sizing: content-box|border-box;

- content-box   border和padding不计算入width之内

- border-box     border和padding计算入width之内，其实就是怪异模式了

#### 背景线性渐变

语法：linear-gradient( [<point> || <angle>,]? <stop>, <stop> [, <stop>]* )
示例：background: linear-gradient(top right,green,red,blue);    右上角渐变三种颜色

#### 轮廓线

• outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用
• 轮廓线不同于边框，不占用空间尺寸。
• 只有在获得焦点或者激活时呈现
属性值：
outline-color    规定边框的颜色
outline-style     规定边框的样式
outline-width   规定边框的宽度

## 过渡动画

• 过渡效果允许css的属性值在一定的时间区间内平滑地过渡。这种效果可以在鼠标单击、
获得焦点或对元素任何改变中触发，并圆滑地以动画效果改变CSS的属性值
**transition简写形式:**
• transition: transition-property transition-duration transition-timing-function transition-delay
**transition-property:**
• 指定应用过渡的属性，值为：none(没有属性改变)；all（所有属性改变）这
个也是其默认值；indent（元素属性名）。
**transition-duration:**
• 指定元素 转换过程的持续时间，单位为s（秒）或者ms(毫秒) 
**transition-delay：**
• 指定动画开始执行前的延迟时间，单位为s（秒）或者ms(毫秒)
**transition-timing-function:**
• transition-timing-function的值允许你根据时间的推进去改变属性值的变换速率，
transition-timing-function有6个可能值：
• ease：（逐渐变慢）默认值，等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0)
• linear：（匀速）
• ease-in：(加速)
• ease-out：（减速）
• ease-in-out：（加速然后减速）

## 变形动画

语法：**transform**: rotate | scale | skew | translate
参数值说明：
• rotate(<angle>) ：指定一个2D旋转，正数顺时针，负数逆时针旋转
• translate(x,y)：设置移动效果
• scale(number,number)：缩放效果
• skew(angle,angle)       扭曲效果  skew(30deg,60deg)
**transform-origin**：设置旋转元素的基点位置
语法：transform-origin: 20% 40%;