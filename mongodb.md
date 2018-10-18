# MongoDB

## 概述

通常以key-value形式存储数据，没有表结构。主要特点：非关系型、分布式、易于水平扩展等。

默认使用端口 27017 和 28017（即在数据库的端口号上加1000）

主从复制：将主机数据复制到其他机器上。主要用于备份、故障恢复、读扩展（读写分离）等。 （不同服务器数据相同）

分片存储：分布式存储（易于扩展）

## 基本操作

#### 连接服务

```
mongo
#什么参数都不加默认连本地的test数据库
mongo ip地址:端口号/数据库名  #默认连接test数据库
#示例
mongo 127.0.0.1:27017/abc
```

#### 关闭原有连接

```
use admin
db.shutdownServer()
```

#### 开启（安装）服务

```
mongod --dbpath D:\mongodb\mdbs （数据库存放地址）
参数含义
--dbpath : 数据存放目录
--logpath : 日志文件路径
--bind_ip : 绑定IP地址
--port : 监听端口号，默认27017
--maxConns : 最大并发连接数
--logappend : 日志追加到现有日志文件中，而不是覆盖
--keyFile : 复制数据时使用的私有key
--auth : 安全模式运行，登录时需要用户名密码。
--cpu : 定期显示CPU和IO的利用率
--directoryperdb : 每个数据库中的数据存在一个单独的目录中
--journal : 启动日志
--nohttpinterface : 不启动WEB服务器接口
--noscripting : 关闭 shell
--repair : 修复所有的数据库
--nssize : 每个数据库的.ns文件的尺寸，默认是16M
--help : 查看所有的参数项

```

#### 关闭（卸载）服务

```
mongod --remove
```

#### 常用操作

##### 基本

```
show dbs 展示数据库
use admin 使用哪个数据库
db 显示当前在哪个数据库
show collections 展示该数据库的所有集合
help 系统级帮助
db.help()  数据库级别的帮助
db.test.help() 集合级别的帮助
```

##### 查询

```
db.test.find() 查看test集合的所有数据 
db.test.find().pretty() 更直观的查看test集合的数据
db.test.find().limit(5) 查询前5条数据
db.test.find().skip(5) 跳过前5条数据（相当于offset）
db.test.find().sort({age:1}) 按照age字段升序排列（1：升序 -1：倒序）
db.test.findOne() 查看第一条数据（后面不能跟.pretty()）
#第一个josn参数为查询条件 空json（即{}）也代表查所有
#第二个json为包含模式或排出模式（相当于查出哪些字段），两种模式不能混用，_id除外 
#包含模式{name:1,age:1}
#排出模式{name:0,age:0}
#特殊情况{name:1,age:1,_id:0}
```

##### 插入

```
db.test.insert({name:"zhangsan",age:18}) 在test集合中插入数据 数据格式是json
```

##### 修改

```
db.test.updateOne({name:"zhangsan"},{$set:{age:24}}) 修改第一条
db.test.updateMany({name:"zhangsan"},{$set:{age:24}}) 修改所有
db.test.update({name:"zhangsan"},{$set:{age:24}},{upsert:true}) 没有则插入新数据 
db.test.save({_id:"ObjectId(d33f31d1d9f8g90)",name:"wangwu",age:23}) 根据_id替换原有数据(不指定_id则添加数据，同insert)
```

```
#不推荐使用
db.test.update({name:"zhangsan"},{$set:{age:24}}) 修改数据只改第一条（第一个json是条件，$set后面的json是修改内容）
db.test.update({name:"zhangsan"},{$set:{age:24}},{multi:true}) 修改所有
```

##### 删除

```
db.test.drop() 删除test集合
db.dropDatabase() 删除当前数据库
db.test.deleteOne({name:"zhangsan"}) 删除第一条
db.test.deleteMany({name:"zhangsan"}) 删除所有
db.drop_collection("test") 删除整个集合及索引
```

```
#不推荐使用
db.test.remove({name:"zhangsan"}) 删除所有
db.test.remove({name:"zhangsan"},{justOne:true}) 删除第一条
```

##### 聚合查询

分组修饰符 $group

```
db.test.aggregate([{$group:{_id:'$team',count:{$sum:1}}}]) 按team字段分组，count字段为每组含有几条数据
db.test.aggregate([{$group:{_id:'$team',agesum:{$sum:'$age'},ageavg:{$avg:'$age'},agemin:{$min:'$age'},agemax:{$max:'$age'},agefirst:{$first:'$age'},agelast:{$last:'$age'}}}]) 
agesum 为每组年龄总和 
ageavg 为每组年龄平均数据
agemin 为每组年龄最小的数据
agemax 为每组年龄最大的数据
agefirst 为每组年龄第一条数据
agelast 为每组年龄最后一条数据
```

其它修饰符

```
$match    查找条件
$project  字段限制
$skip     跳过第几条
$limit    查出几条
$sort     排序
```

#### 查询条件

json示例

##### 比较条件

```
{age:25}        =  25
{age:{$gt:25}}  >  25
{age:{$gte:25}} >= 25
{age:{$lt:25}}  <  25
{age:{$lte:25}} <= 25
{age:{$ne:25}}  != 25
```

##### and和or

```
{name:"zhangsan",age:25}            AND
{$or:[{name:"zhangsan"},{age:25}]}  OR
```

##### 字段类型type

```
{name:{$type:1}} 或 {name:{$type:'double'}}  常用类型
'double'=>1,'string'=>2,'object'=>3,'array'=>4
```

##### in和nin

```
{age:{$in:[10,20,30]}}
{age:{$nin:[10,20] }}
```

##### not和mod (取模)

```
{age:{$mod:[3,1]}}  年龄对3取模余1 
{age:{$not:{$mod:[3,1]}}} 对上面的取反
```

##### exists 

判断键是否存在

```
判断值为 null 时要注意：
{friends :null}
该条件不仅查出值为null的记录，friends 键不存在的记录也会被取出来。如果想取出friends键存在，并且值为null的记录应该这样来取：
{friends:null,$exists:1}
```

##### where 

根据函数返回值来判断

```
如：取出年龄乘以10+5小于100的
{$where:function(){
	return ((this.age*10+5)<100);
}}   
this:代表当前这个记录
```

通过这个 $where 基本可以实现任意类型的查询。
不过不到必要时候不要用这个方法，因为它的速度比一般查询要慢很多。

##### all

判断某个数组字段包含多个值时

```
{city:{$all:["hangzhou","chengdu","guangzhou"]}}
```

##### size

判断某个数组字段的尺寸

```
{city:{$size:3}}
```

##### elemMatch

字段是一个数组，每一项是一个内嵌的对象

```
{comments:{$elemMatch:{score:{$gt:5}}}}
```

#### 索引

##### 创建

test集合创建索引，第一种写法

```
db.test.createIndex({username:1,username:-1}) 
为name字段创建升序索引，为age字段创建降序索引（除了整形和浮点型数据，其它一般设为1）
db.test.createIndex({username:1},{unique:true},{username:ui})
第二个json是唯一索引 第三个json是索引名字
```

test集合创建索引，第二种写法，只是单词换了，其它一模一样，推荐第一种

```
db.test.ensureIndex({username:1,username:-1}) 
为name字段创建升序索引，为age字段创建降序索引（除了整形和浮点型数据，其它一般设为1）
db.test.ensureIndex({username:1},{unique:true},{username:ui})
第二个json是唯一索引 第三个json是索引名字
```

##### 查看

查看test集合上的索引

```
db.test.getIndexes()
```

##### 删除

删除test集合上，username字段的索引

```
db.test.dropIndex(username)
```

删除test集合上所有的索引

```
db.test.dropIndexes();
```

## 权限机制

#### 添加、修改账号 

#### 查看账号

```
db.system.users.find()
```

#### 删除账号

```
db.system.users.remove({user:”sclzdj”})   删除test sclzdj
```

#### 权限生效

重新以验证模式启动mongod，只有在验证模式账号才会生效，并且必须要先有一个管理员账号

```
mongod --auth	
```

## 固定集合

固定集合是一种尺寸固定的环形的集合，按照顺序一个一个数据插入，当集合数据满了之后，再继续插入会覆盖掉前面的数据，继续掉入。

#### 特点

不能删除，abc  abcxz
更新不能导致文档移动 （修改后数据不能超过原数据尺寸）
没有任何索引
存储顺序就是插入顺序
插入速度极快
按同样顺序查询极快
自动淘汰旧的数据

#### 操作

##### 创建固定集合

```
db.createCollection(name,{capped:true,size:1024,max:100});
size : 集合的尺寸，单位字节
max : 集合中最大的记录数
淘汰机制主依据size，即在尺寸没有满时才会依据max来工作。
```

##### 普通集合转换为固定集合

```
db.runCommand(convertToCapped:"test",size:100000);
```

##### 查询自然排序

```
db.collection.find().sort({$natural:1})   按插入的顺序排序
db.collection.find().sort({$natural: -1})   相反插入顺序
```

## GridFS存储文件

GridFS用来在Mongodb中存储文件。主要使用mongofiles.exe命令来完成文件的上传、下载、删除等功能。

```
mongofiles put test.txt     #存储test.txt文件到mongodb中
mongofiles list             #查看文件列表
mongofiles search filename  #搜索某个文件
mongofiles get filename     #获取某个文件
mongofiles delete filename  #删除某个文件
```

## 主从复制

启动主服务器(a电脑）

```
mongod --master --bind_ip 192.168.1.111 --port 27017 --dbpath 数据目录 --logpath 日志目录
```

启动从服务器(b电脑）

```
mongod - -slave --source 192.168.1.111:27017 --dbpath 数据目录 --logpath 日志目录
```

## 备份与恢复

```
mongodump -d 要备份的数据库名 -o 输出目录
mongorestore -d 恢复到的数据库名 --drop 备份文件目录
```


备份实例：备份 test 数据库到d:\bk

```
mongodump -d test -o D:\bk
```


恢复实例： 恢复test数据库到test1数据库中

```
mongorestore -d test1 -drop D:\bk\test
```

## PHP操作

使用findOne()时返回一个数组
使用find()时返回的是一个游标

```
//连接地址："mangodb://{账号}:{密码}@{主机地址}/{数据库名称}"
$host="mangodb://root:123456@127.0.0.1/test";
//生成mongo对象
$mongo = new Mongo($host); 
//查询test数据库中的user集合的所有数据
$data = $mongo->test->user->find(); 
```

使用find()时返回的是一个游标，需要用以下格式来遍历数据：

```
//判断是否有下一条记录
while($data->hasNext()){
	$d  = $data->getNext();  // 取出下条记录，返回数组格式
}
```

操作方式和原生语法基本一样，主要区别有一下几点：

> 原生json格式，php是数组格式
>
> 原生是.语法，php是->语法
>
> php括号里面的参数和原生完全一致

实例1：备份 test 数据库到d:\bk
​	mongodump -d test -o D:\bk

实例2： 恢复test数据库到test1数据库中
​	mongorestore -d test1 -drop D:\bk\test