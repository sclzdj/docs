# memcache

## 概述

> 高性能的分布式的内存对象缓存系统

> Key-value型存储

> 监听端口 11211

## 基本操作指令

安装telnet远程登录memcache客户端

> set,add,replace三个命令参数相同，参数分别代表[键名，压缩标识，有效期(s)，键值长度]，键值是在执行该命令后输入

| 命令          | 描述                      | 示例                 |
| ------------- | ------------------------- | -------------------- |
| **get**       | 读取key                   | get mykey            |
| **set**       | 设置key                   | set mykey 0 60 5     |
| add           | 添加新key                 | add newkey 0 60 5    |
| replace       | 替换现有key               | replace key 0 60 5   |
| **append**    | 将数据追加到现有key之后   | append key 0 60 15   |
| **prepend**   | 将数据追加到现有key之前   | prepend key 0 60 15  |
| **incr**      | key自增多少               | incr mykey 2         |
| **decr**      | key自减多少               | decr mykey 5         |
| **delete**    | 删除指定key               | delete mykey         |
| **flush_all** | 删除所有key               | flush_all            |
|               | 指定所有key在多少秒内失效 | flush_all 900        |
| **stats**     | 打印统计信息              | stats                |
|               | 打印内存统计信息          | stats slabs          |
|               | 打印内存统计信息          | stats malloc         |
|               | 打印详细统计信息          | stats items          |
|               | 打印详细统计信息          | stats detail         |
|               | 打印详细统计信息          | stats sizes          |
|               | 重置统计信息              | stats reset          |
| version       | 打印版本                  | version              |
| verbosity     | 增加日志级别              | verbosity            |
| **telnet**    | 连接telnet会话            | telnet {host} {port} |
| **quit**      | 终止telnet会话            | quit                 |

## stats指令

| 标识                  | 含义                                                         |
| --------------------- | ------------------------------------------------------------ |
| pid                   | **memcache服务器的进程ID**                                   |
| uptime                | 服务器已经运行的秒数                                         |
| time                  | 服务器当前的unix时间戳                                       |
| version               | **memcache版本**                                             |
| pointer_size          | 当前操作系统的指针大小（32位系统一般是32bit）                |
| rusage_user           | 进程的累计用户时间                                           |
| rusage_system         | 进程的累计系统时间                                           |
| curr_items            | **服务器当前存储的items数量**                                |
| total_items           | **从服务器启动以后存储的items总数量**                        |
| bytes                 | **当前服务器存储items占用的字节数**                          |
| curr_connections      | 当前打开着的连接数                                           |
| total_connections     | 从服务器启动以后曾经打开过的连接数                           |
| connection_structures | 服务器分配的连接构造数                                       |
| cmd_get               | **get命令（获取）总请求次数**                                |
| cmd_set               | **set命令（保存）总请求次数**                                |
| get_hits              | **总命中次数**                                               |
| get_misses            | **总未命中次数**                                             |
| evictions             | 为获取空闲内存而删除的items数（分配给memcache的空间用满后需要删除旧的items来得到空间分配给新的items） |
| bytes_read            | 总读取字节数（请求字节数）                                   |
| bytes_written         | 总发送字节数（结果字节数）                                   |
| limit_maxbytes        | 分配给memcache的内存大小（字节）                             |
| threads               | 当前线程数                                                   |

## PHP操作

#### 基本操作

```
//生成memcache对象
$memcache= new Memcache(); 
//主机地址
$host="127.0.0.1";
//监听端口
$port="11211";
//连接memcache
$memcache->connect($host,$port);#主机地址，监听端口 

#####此处开始业务逻辑#####
//设置mykey值
$memcache->set("mykey","value",0,3600); #键名，键值，压缩标识，有效期(s)
//.....
#####此处业务逻辑结束#####

//清除所有key
$memcache->flush();
//关闭连接
$memcache->close();
```

```
#其它常用操作
//获取mykey值
$data = $mongo->get("mykey"); #键名
//新增mykey
$memcache->add("mykey","value",0,3600); #键名，键值，压缩标识，有效期(s)
//替换mykey
$memcache->replace("mykey","value",0,3600); #键名，键值，压缩标识，有效期(s)
//设置mykey值自增多少
$memcache->increment("mykey",2); #键名，增幅
//设置mykey值自减多少
$memcache->increment("mykey",2); #键名，减幅
//删除mykey
$memcache->delete("mykey"); #键名
```

> 标量类型（int,float,bool,string）存储之后全部转字符串类型

> 非标量类型（array,object,source,null）存储之后
>
> array、object和null可以正常取出并且类型不变（原理是利用序列化）
>
> source类型取出为整形0，结构和类型全变，所以不能存储（原因是不能序列化）

#### 分布式操作

注意：分布式操作对memcache服务器数量和顺序有严格的要求

```
//生成memcache对象
$memcache= new Memcache(); 
$memcache->addServer($host1,$port1);#主机地址1，监听端口1
$memcache->addServer($host2,$port2);#主机地址2，监听端口2
$memcache->addServer($host3,$port3);#主机地址3，监听端口3

#####此处开始业务逻辑#####
//设置mykey值
$memcache->set("mykey","value",0,3600); #键名，键值，压缩标识，有效期(s)
//.....
#####此处业务逻辑结束#####

//清除所有key
$memcache->flush();
//关闭连接
$memcache->close();
```

#### 扩展

```
getStats()获得状态信息
Pconnect()持久连接
getVersion()获得版本信息
addServer()向连接池中添加一个memcache服务器
getServerStatus()  用于获取一个服务器的在线/离线
```

#### session入memcache

修改php.ini

```
Session.save_handler = memcache
Session.save_path = tcp://host1:port1,tcp://host2:port2
```

## 懒惰删除

##### memcache

> **懒删除**：过期和删除的数据，在获取的时候才进行删除 
> **LRU原则**（Least Recently Used）:内存空间满了，会把最近最少使用缓存删除掉，不管是否过期

##### redis 

> 没设置过期时间的数据，即使内存空间满了，也不会删除，只会将其从内存移至硬盘。