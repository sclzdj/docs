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

text-overflow: clip|ellipsis|string;

参数说明：

- clip 修剪文本。
- ellipsis 显示省略符号来代表被修剪的文本。
- string 使用给定的字符串来代表被修剪的文本。

```
h2{
    width:100px; 
    overflow: hidden;
    white-space: nowrap;/*文字不换行*/
    text-overflow:ellipsis;
}
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

## 元素水平垂直居中

**如果元素有固定的宽高**

```
#方法一：
#box{
    width: 400px;
    height: 260px;
    background: mediumpurple;
    /*如果元素有固定的宽高*/
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
}

#方法二：
#box{
    width: 400px;
    height: 260px;
    background: mediumpurple;
    /*如果元素有固定的宽高*/
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -200px;
    margin-top: -130px;
}
```

**如果元素没有固定的宽高**

```
#box{
    width: 400px;
    background: mediumpurple;
    /*如果元素没有固定的宽高*/
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
}
```

## 关键帧动画

• Animation只应用在页面上已存在的DOM元素上，而且他跟Flash和JavaScript以及jQuery制作出来的动画效果又不一样，因为我们使用CSS3的Animation制作动画我们可以省去复杂的js,jquery代码。
**@Keyframes**
• keyframes即关键帧。一般和animation配合使用。用来安排运动过程。
**简写： animation: name duration timing-function delay iteration-count direction fill-mode**

分写形式：

**animation-name**
• 定义一个动画的名称。
**animation-duration**
• 指定动画播放时长，单位为s(秒),默认值为“0”。
**animation-timing-function**
• 动画的播放方式。和transition中的transition-timing-function一样，具有以下六种变换方式：ease;ease-in;ease-in-out;linear;cubic-bezier。
**animation-delay**
• 指定延迟时间。单位为s(秒)，其默认值也是0。这个属性和transition-delay使用方法是一样的。
**animation-iteration-count**
• 指定元素播放动画的循环次数，其默认值为“1”；infinite为无限次数循环。
**animation-direction**
• 是否应该轮流反向播放动画
• 如果 animation-direction 值是 "alternate"，则动画会在奇数次数（1、3、5 等等）正常播放，而在偶数次数（2、4、6 等等）向后播放。
**animation-fill-mode**
• 控制元素动画的填充模式。有3个值，forwards、backwards和both。
forwards：动画执行后，不返回初始状态。backwards：在延迟期间元素会保持运动的初始状态。both：forwards和backwards的效果同时存在。
**animation-play-state**
• 控制元素动画的播放状态。有两个值，running和paused其中running为默认值。他们的作用就类似于我们的音乐播放器一样，可以通过paused将正在播放的动画停下了，也可以通过running将暂停的动画重新播放。

```
<!--示例-->
<style type="text/css">
    #box{
        width: 300px;
        height: 200px;
        background: purple;
        /*指定运动规则*/
        /*animation-name: an;*/
        /*指定运动时间*/
        /*animation-duration: 3s;*/
        /*指定运动曲线*/
        /*animation-timing-function: linear;*/
        /*运动延迟*/
        /*animation-delay: 2s;*/
        /*指定运动次数*/
        /*animation-iteration-count: infinite;*/
        /*一行简写*/
        /*animation: hd 2s linear 1s infinite;*/
        animation: hd 2s infinite;
    }
    /*设置运动规则（关键帧）*/
    /*@keyframes an{
        form{
            width: 300px;
        }
        to{
            width: 900px;
        }
    }*/	
    @keyframes an{
        0%{
            width: 300px;
        }
        50%{
            width: 900px;
        }
        100%{
            width: 300px;
        }
    }		
</style>
```

## 弹性盒布局

• Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
• 任何一个容器都可以指定为Flex布局。
• 采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。

#### display:flex

容器默认存在两根轴：**水平的主轴**（main axis）和**垂直的交叉轴**（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

#### flex-direction

该属性决定主轴的方向（即项目的排列方向）
• row（默认值）：主轴为水平方向，起点在左端。
• row-reverse：主轴为水平方向，起点在右端。
• column：主轴为垂直方向，起点在上沿。垂直方向不要设置行高。
• column-reverse：主轴为垂直方向，起点在下沿。垂直方向不要设置行高。

#### justify-content

定义了项目在主轴上的对齐方式
• flex-start（默认值）：左对齐
• flex-end：右对齐
• center： 居中
• space-between：两端对齐，项目之间的间隔都相等。
• space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

#### align-items

定义项目在交叉轴上如何对齐。
• flex-start：交叉轴的起点对齐。
• flex-end：交叉轴的终点对齐。
• center：交叉轴的中点对齐。
• baseline: 项目的第一行文字的基线对齐。
• stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#### flex-wrap

 默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
• nowrap（默认）：不换行。
• wrap：换行，第一行在上方。
• wrap-reverse：换行，第一行在下方。

#### align-content

定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
• flex-start：与交叉轴的起点对齐。
• flex-end：与交叉轴的终点对齐。
• center：与交叉轴的中点对齐。
• space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
• space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
• stretch（默认值）：轴线占满整个交叉轴。

#### order

定义**项目**的排列顺序。数值越小，排列越靠前，默认为0。

#### flex

定义**项目**的比例。