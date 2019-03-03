# LESS

## 安装

```
$ node -v   #查看是否安装node.js
$ npm install -g less   #安装less -g表示全局安装
```

## 使用less

#### 1、在ide里面配置less编译

#### 2、使用koala编译工具

## 代码格式

#### 定义变量

```
@kared:#f00000;
```

#### 定义混合变量 （类似于函数）

```
.pix(@w:100px,@h){
  width:@w;
  height:@h;
}
```

#### 运算

```
@v:10px;
h1{
  width:@v*2;
  height:@v*3+20px;
}
```

#### 串联

```
#box{
  a{
    color: red;
    &.active{
      color: blue;
    }
    &:hover{
      opacity: 0.8;
    }
  }
}
```

#### 继承

```
.a{
  width: 100px;
  height: 100px;
}
.b{
  color: red;
}
.c{
  &:extend(.a,.b);
  background-color: green;
}
```

继承之后效果

```
.a,.c{
  width: 100px;
  height: 100px;
}
.b,.c{
  color: red;
}
.c {
  background-color: green;
}
```