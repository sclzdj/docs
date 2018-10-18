# Redis

## 概述

>内存高速缓存数据库

> 监听端口6379

## 优势

redis对比memcache

##### 多样数据类型

> Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储。

##### 主从模式

> Redis支持master-slave(主—从)模式应用。

##### 数据持久化

> Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。

##### 单个key可以存很大

> Redis单个key的最大限制是1GB， memcache只能保存1MB的数据

## 基本操作

对key的基本操作，所有数据类型都可以使用

```
#选择数据库
select {db-index}

#指定key是否存在
exists {key} 

#删除给定key
del {key1} {key2}......{keyN}

#返回key的value类型
type {key}

#返回匹配指定模式的所有key
keys {pattern}  #类似于正则

#当前数据库随机返回一个key
randomkey

#重命名key
rename {oldkey} {newkey}

#返回当前数据库key的数量
dbsize

#指定key过期时间（s）
expire {key} {seconds}

#返回key剩余过期时间（s）
ttl {key}

#将key移动到指定数据库
remove {key} {db-index}

#删除当前数据库所有key
flushdb

#删除所有数据库所有key
flushall
```

### string

string是redis最基本的类型，redis的string可以包含任何数据。包括jpg图片或者序列化的对象。

单个value值最大上限是1G字节。 

如果只用string类型，redis就可以被看作加上持久化特性的memcache

```
#设置key的值
set {key} {value}

#获取key的值
get {key}

#一次设置多个key的值
mset {key1} {value1} {key2} {value2} ...... {keyN} {valueN}

#一次获取多个key的值
mget {key1} {key2} ...... {keyN}

#加加操作
incr {key}

#减减操作
decr {key}

#自增指定值
incrby {key} {int}

#自减指定值
decrby {key} {int}

#指定key后面追加value
append {key} {value}

#返回截取过的key
substr {key} {start-index} {end-index}
```

### list

是一个双向链表，既可以用作栈，也可以用作队列。

```
#在key左边添加一个元素
lpush {key} {member}

#在key右边添加一个元素
rpush {key} {member}

#从key左边删除一个元素
lpop {key}

#从key右边删除一个元素
rpop {key}

#返回list的长度，key不存在返回0，如果key类型不是list返回错误
llen {key}

#返回指定区间的元素
lrange {key} {start-index} {end-index}

#截取list，保留指定区间的元素
ltrim {key} {start-index} {end-index}

#设置list中指定下标的元素值
lset {key} {index} {member}

#删除count个和member相同的元素，num=0时全部删除
lrem {key} {num} {member}
```

### set

该类型应用场合：好友推荐、微博系统的关注关系

```
#添加一个元素到集合，成功返回1，已经存在返回0，对应的set集合不存在返回错误
sadd {key} {member}

#从key对应移除指定的元素
srem {key} {member1} {member2} ...... {memberN}

#从key1中移除member并添加到key2中
smove {key1} {key2} {member}

#返回元素个数
scard {key}

#判断member是否存在
sismember {key} {member}

#返回key集合所有的元素，返回结果是无序的
smembers {key}

#返回给定key的交集
sinter {key1} {key2} ...... {keyN}
#返回给定key的交集并存到keyStore集合中
sinterstore {keyStore} {key1} {key2} ...... {keyN}

#返回给定key的并集
sunion {key1} {key2} ...... {keyN}
#返回给定key的并集并存到keyStore集合中
sunionstore {keyStore} {key1} {key2} ...... {keyN}

#返回给定key的差集
sdiff {key1} {key2} ...... {keyN}
#返回给定key的差集并存到keyStore集合中
sdiffstore {keyStore} {key1} {key2} ...... {keyN}
```

### sort set

和set一样sorted set也是string类型元素的集合，不同的是每个元素都会关联一个权，通过权值可以有序的获取集合中的元素。

```
#添加元素到集合，元素存在，则对应更新score权
zadd {key} {score} {member}

#删除指定元素，1表示成功，如果元素不存在返回0
zrem {key} {member}

#返回元素个数
zcard {key}

#返回指定元素对应的score权值
zscore {key} {member}

#对应member元素的score权增加int幅度，返回更新后的score权
zincrby {key} {int} {member}

#返回指定元素在集合中的排名（下标），元素按score权升序排列
zrank {key} {member}
#返回指定元素在集合中的排名（下标），元素按score权降序排列
zrevrank {key} {member}

#返回指定下标区间元素，元素按score权升序排列
zrange {key} {start-index} {end-index}
#返回指定下标区间元素，元素按score权降序排列
zrevrange {key} {start-index} {end-index}

#返回指定score权区间元素
zrangebyscore {key} {min} {max}

#返回指定score权区间元素的个数
zcount {key} {min} {max}

#删除集合指定排名（下标）之间的元素
zremrangebyrank {key} {start-index} {end-index}

#删除集合指定score权之间的元素
zremrangebyscore {key} {min} {max}
```

### hash

hash数据类型是redis模仿数据库把一条记录信息给存储起来

```
#设置指定字段的值
hset {key} {field} {value}
#获取指定字段的值
hget {key} {field}

#设置全部指定字段的值
hmset {key} {field1} {value1} {field2} {value2} ...... {fieldN} {valueN}
#获取全部指定字段的值
hmget {key} {field1} {field2} ...... {fieldN}

#将指定的字段加上给定的int值
hincrby {key} {field} {int}

#测试指定的字段是否存在
hexists {key} {field}

#删除指定的字段
hdel {key} {field}

#返回字段数量
hlen {key}

#返回所有的字段名称
hkeys {key}

#返回虽有的字段值
hvals {key}

#返回所有的字段名称和字段值
hgetall {key}
```

## 持久化功能

### 快照持久化

又名snap shotting。该持久化默认开启，一次性把redis中全部的数据保存一份存储在硬盘中，如果数据非常多(10-20G)就不适合频繁持久化操作。

##### 配置项

进入redis安装目录，配置文件为redis.conf，其中：

```
#快照持久化的触发机制
save 900 1 		#900 秒内如果超过1个key被修改，则发起快照保存
save 300 10     #300秒超过10个key被修改，发起快照
save 60 10000   #60秒超过10000个key被修改，发起快照

#其它相关
dbfilename dump.rdb  #快照持久化备份文件保存的文件名称
dir ./               #快照持久化备份文件保存的目录
```

若有修改，请重启redis服务器

##### 手动发起快照保存

进入redis安装目录，执行命令：

```
redis-cli bgsave
```

### AOF持久化

又名append only file。本质：把用户执行的每个“写”指令(添加、修改、删除)都备份到文件中，还原数据的时候就是执行具体指令而已。默认关闭的。

##### 开启

进入redis安装目录，配置文件为redis.conf，其中：

```
appendonly yes                  #开启aof持久化
appenfilename appendonly.aof    #aof持久化备份文件名称
appendfsync everysec            #让操作系统来决定应该何时进行同步，有下面三个选择
	#always     //每次收到写命令就立即强制写入磁盘，最慢的，但是保证完全的持久化，不推荐使用
	#everysec   //每秒钟强制写入磁盘一次，在性能和持久化方面做了很好的折中，**推荐**
	#no         //完全依赖os，性能最好,持久化没保证
```

若有修改，请重启redis服务器

##### 优化

进入redis安装目录，执行命令：

```
redis-cli bgrewriteaof
```

## 主从模式

只需要在**从redis**上操作，进入redis安装目录，配置文件为redis.conf，其中：

```
slaveof {host} {port}    #连接**主redis**的地址
```

## PHP操作

redis中的许多指令就是php中对象调用的成员，参数与redis指令的参数基本一致

```
//在php里边，redis就是一个类，通过实例化对象，对象调用成员就可以使用redis
$redis = new Redis();

//连接redis服务
$redis -> connect($host,$prot);#主机地址，监听端口

//进入第3个数据库
$redis -> select(3);

//给redis设置key
$redis -> set('flower','rose');

//调用方法有多个参数情况（key-value格式）
$redis -> mset(array('addr1'=>'beijing','addr2'=>'tianjin','addr3'=>'shanghai'));

//调用方法有多个参数(v,v,v,v,v)
$result = $redis -> mget(array('addr1','addr2','addr3'));
```


