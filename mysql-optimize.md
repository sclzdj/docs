

# MySQL优化

## 设计层次

### 1.存储引擎

**存储引擎就是特定的数据存储格式（方案）**

创建命令：

```
create table {table-name} () engine=myisam|innodb;
```

查看当前MySQL支持的存储引擎列表，执行命令：

```
show engines
```

#### Innodb

=5.5 默认的存储引擎，MySQL推荐使用的存储引擎。

提供事务，行级锁定，外键约束的存储引擎。

事务安全型存储引擎。更加注重数据的完整性和安全性。

##### 数据存储机制

数据，索引集中存储，存储于同一个表空间文件中。

> 表结构文件：/data/{table-name}.frm
>
> 表空间文件：/ibdata1 （所有innodb的表数据和索引都存放在这里）

通过配置可以让每张表一个空间

```
#show variables like "innodb_file_per_table";
set global innodb_file_per_table=1;
#show variables like "innodb_file_per_table";
```

配置之后表空间文件：/data/{table-name}.ibd

##### 记录存储机制

记录按照主键顺序存储，所以插入时做排序工作，效率低。

##### 特定功能

> 事务	
>
> 外键约束
>
> 维护数据完整性

##### 并发性处理

innodb是擅长处理并发的

> 行级锁定：row-level locking，实现了行级锁定，在一定情况下，可以选择行级锁来提升并发性。也支持表级锁定，innodb根据操作选择。
>
> 多版本并发控制, 	MVCC，效果达到无阻塞读操作。

#### MyISAM

<= 5.5 MySQL默认的存储引擎。

ISAM：Indexed Sequential Access Method(索引顺序[存取方法](http://baike.baidu.com/view/3401818.htm))的缩写，是一种文件系统。

擅长与处理 高速读与写。

##### 数据存储机制

表结构、数据、索引分别存储于不同的文件中。

> 表结构文件：/data/{table-name}.frm
>
> 表数据文件：/data/{table-name}.MYD
>
> 表索引文件：/data/{table-name}.MYI

##### 记录存储机制

数据的存储顺序为插入顺序，所以插入速度快，空间占用量小。

##### 特定功能

> 全文索引支持。（>=5.6 innodb 支持）
>
> 数据的压缩存储。{table-name}.MYD文件的压缩存储。

##### 压缩

> 优势：节省磁盘空间，减少磁盘IO开销。

> 特点：压缩表为**只读表**。

操作方式，进入mysql安装的bin目录执行命令：

```
#压缩
myisampack {table-name}

#注意，压缩后，需要重新修复索引
myisamchk -rq {table-name}

#解压缩（更新表必需先解压缩）
myisamchk –unpack {table-name}
#解压缩后要刷新一下表
flush table {table-name} 
```

##### 并发性处理

> 仅仅支持表级锁定
>
> 支持并发插入。写操作中的插入操作，不会阻塞读操作（其他操作）

#### Innodb PK myisam

> innodb：数据完整性，并发性处理，擅长更新，删除。

> myisam：高速查询及插入。擅长 插入，查询。

#### Archive

##### 存档型表

仅提供 插入和查询操作。非常高效 无阻塞的插入和查询。

常用场景：地区表

#### Memory

##### 缓存型表

数据存储于内存中，存储引擎。缓存型存储引擎。

#### mrg_myisam

MySQL提供一个可以将多个结构相同的myisam表，合并到一起的存储引擎

### 2.字段类型

选择原则

> **尽可能小（占用存储空间少）**
>
> Tinyint, smallint, mediumint,int, bigint
>
> Varchar(N) varchar(M)
>
> Datetime, timestamp

>**尽可能定长（占用存储空间固定）**
>
>Char,varchar
>
>Decimal（变长）, double(float)(定长)

> **尽可能使用整数**
>
> IPV4， int unsigned， varchar(15)
>
> Enum
>
> Set
>
> **多用位运算**

### 3.范式和逆范式

表设计应该遵循第三范式，设计阶段可以考虑逆范式。

**第一范式**

> 表之间数据完全无关联

**第二范式**

>表之间数据有关联字段，但表之间存在冗余字段

**第三范式**

>表之间数据有关联字段，表之间不存在冗余字段

**逆范式**

>一般用于统计字段设计，维护成本高，设计阶段可考虑。

### 4.垂直分表

表中存在多个字段。

常用字段 - 非常有字段

主要目的，减少每条记录的长度。

```
例如学生表可以分成：
基础表和额外表，两张表中记录为1:1的关系。

基础信息表 Student_base
	Id	name	age

额外信息表  Student_extra
	Id	籍贯	政治面貌
```

## 功能层次

### 1.索引

> 利用关键字，就是记录的部分数据（某个字段，某些字段，某个字段的一部分），建立与记录位置的对应关系，就是索引。

> 索引的关键字一定是排序的

#### 索引类型

**普通索引**

index：	对关键字没有要求。

**唯一索引**

unique index：	要求关键字不能重复。同时增加唯一约束。

**主键索引**

primary key：	要求关键字不能重复，也不能为NULL。同时增加主键约束。

**全文索引**

fulltext key：	关键字的来源不是所有字段的数据，而是从字段中提取的特别关键词。

#### 复合索引

索引关键字的来源可以是某个字段，也可以是某些字段。如果一个索引通过在多个字段上提取的关键字，称之为 复合索引。

```
alter table {table-name} add index ({field1}, {field2}...);
alter table {table-name} add unique index ({field1}, {field2}...);
alter table {table-name} add primary key ({field1}, {field2}...);
alter table {table-name} add fulltext key ({field1}, {field2}...);
```

#### 操作索引

索引可以起名字，但是**主索引不能起名字**，因为一个表仅仅可以有一个主索引，其他索引可以出现多个。名字可以省略，mysql会默认生成，通常使用字段名来充当

> 建表时

```
create table {table-name} (
	primary key ({field}),          #主键索引
	unique index {name}({field}),   #唯一索引
	index {name}({field}),          #普通索引
	fulltext key {name}({field}),   #全文索引
) engine=myisam;

#{name}索引名称可省略
```

> 更新时

```
alter table {table-name} add 
	primary key ({field}),       #主键索引
	unique index ({field}),      #唯一索引
	index ({field}),             #普通索引
	fulltext key ({{field});     #全文索引
```

如果表中存在数据，数据符合唯一或主键的约束才可能创建成功。

Auto_increment属性，依赖于一个primary key。

> 删除时

```
alter table {table-name} 
	drop primary key, #删除主键索引（要先去除auto_increment属性）
	drop index {name},#删除唯一索引、普通索引和全文索引方法相同
	drop index {name},
	drop index {name},
```

#### 索引的使用

**使用场景**

建立索引索引时，不要仅仅考虑where检索，同时考虑其他的使用场景。

（在所有的where字段上增加索引，就是不合理的）

>**索引检索**

> **索引排序**

> **索引覆盖**：索引拥有的关键字内容，覆盖了查询所需要的全部数据，此时，就不需要在数据区获取数据，仅仅在索引区即可。

**使用原则**

> **列独立**：如果需要某个字段上使用索引，则需要在字段参与的表达中，保证字段独立在一侧
>
> **左原则** ： 1、Like匹配模式必须要左边确定不能以通配符开头  2、一个索引关联多个字段，仅仅针对左边字段有效果。
>
> **OR的使用**：必须要保证 OR 两端的条件都存在可以用的索引，该查询才可以使用索引
>
> **智能选择**：即使满足了上面三个原则，MySQL也能弃用索引。弃用索引的主要原因：查询即使使用索引，会导致出现大量的 随机IO，相对于从数据记录的第一条遍历到最后一条的顺序IO开销，还要大。

#### 执行计划

可以通过在select语句前使用 **explain**，来获取该查询语句的执行计划，而不是真正执行该语句。

> type：查询方式    all（全表扫描） ref（基于索引查询）
>
> possible_keys：  可能使用到的索引
>
> key：真正选择使用的索引
>
> rows：从存储引擎获取到这么行记录，实现该查询，此值越小越快
>
> extra：额外信息  Using where，表示使用条件检索     Using filesort，表示使用文件排序（外部排序，内存外部） Using index，标识使用了索引覆盖

目前只有select语句才能获取到执行计划。

新版本会扩展其他语句的执行计划的获取。

#### 前缀索引

建立索引关键字通常会使用字段的整体作为索引关键字。

有时，即使使用字段前部分数据，也可以去识别某些记录。

注意：前缀索引不能用于索引覆盖。

```
alter table {table_name} add index {name}({field}({num}));
#使用field前num个字符建立的索引。
```

最大辨识度

```
#{count}为总记录数
select {count}/count(distinct {field} from {table-name};
```

num取值算法

```
#更换{num}值，找到与最大辨识度接近的一个点
select {count}/count(distinct substring({field},1,{num}) from {table-name};
```

#### 全文索引

全文索引索引的的关键字，不是整个字段数据，而是从数据中提取的关键词。

不支持中文

```
#建立全文索引
alter table {table-name} add fulltext key ({field1}, {field2}...);

#使用全文索引
select * from {table-name} where match({field1},{field2}...) against('{keywords}');

#返回的关键字的匹配度（关键字与记录的关联程度）
select match({field1},{field2}...) against('{keywords}') from {table-name};
```

#### 索引的数据结构

指的是mysql存储索引所采用的数据结构。

> **Hash**

> **B-Tree（B树）**
>
> 用户所维护的所有的索引结构 B-Tree结构
>
> B-Tree的结构如下：
>
> 每个节点，存储多个关键字。
>
> 关键字也会对应记录地址
>
> 以上设计为了解决，一次性磁盘IO开销，可以读取到更多的关键字数量。
>
> 如果是复合索引，关键字的排序先排左侧字段，在左侧字段相同的情况下，再排序右侧字段

#### 聚集索引,聚簇索引

> 在innodb的存储引擎上，主索引是与数据记录 存储在一起的（聚簇在一起的）。

> Innodb的其他索引，非主键索引（二级索引）：关键字对应的不再是记录的地址，而是记录的主键。可见，检索需要 二次检索。先检索到主键ID，在检索记录。

### 2.分区

partition，取代了水平分表、

将某张表数据，分别存储到不同的区域中

每个分区，就是独立的表。都要存储该分区数据的数据，索引等信息。

分区与存储引擎无关，是MySQL逻辑层完成的。

```
#查看当前数据库是否支持分区
show variables like 'have_partitioning';
```

#### Key – 取余

```
#{num}为分成几份
create table {table-name}(
	id int unsigned not null auto_increment,
	primary key (id)
)partition key (id) partitions {num};

#增加{num}个分区
alert table {table-name} add partitions {num}

#减少{num}个分区
alert table {table-name} coalesce partition {num}
```

#### Hash – 取余

```
#{num}为分成几份
create table {table-name}(
	id int unsigned not null auto_increment,
	birthday date,
	primary key (id)
)partition hash (month(birthday)) partitions {num};

#增加{num}个分区
alert table {table-name} add partitions {num}

#减少{num}个分区
alert table {table-name} coalesce partition {num}
```

#### List – 条件 – 列表

需要指定的每个分区数据的存储条件。

```
#根据生日字段将表分为春夏秋冬四个区
create table {table-name}(
	id int unsigned not null auto_increment,
	birthday date,
	primary key (id)
)partition hash (month(birthday)) (
	partition chun values in (1,2,3),
	partition xia values in (4,5,6),
	partition qiu values in (7,8,9),
	partition dong values in (10,11,12)
);

#增加other分区
alert table {table-name} add partition （
	partition other values in (13,14,15),
）;

#删除other分区(删除条件算法的分区，导致分区数据丢失)
alert table {table-name} drop partition other;
```

#### Range – 条件– 范围

```
#根据生日字段将表分为1980之前，1980~1990，1990~2000，2000之后四个区
create table {table-name}(
	id int unsigned not null auto_increment,
	birthday date,
	primary key (id)
)partition hash (month(birthday)) (
	partition p_min80 values less than (1980),
	partition p_80 values less than (1990),
	partition p_90 values less than (2000),
	partition p_max20 values less than maxvalue
);

#增加p_min70分区
alert table {table-name} add partition （
	partition p_min70 values less than (1970),
）;

#删除p_min70分区(删除条件算法的分区，导致分区数据丢失)
alert table {table-name} drop partition p_min70;
```

采用取余算法的分区数量的修改，不会导致已有分区数据的丢失，会重新分配数据到新分区。

删除条件算法的分区，导致分区数据**丢失**

**选择分区算法**

> 平均分配：就按照主键进行key(primary key)即可(非常常见)

> 按照某种业务逻辑分区：选择那种最容易被筛选的字段，整数型

### 3.查询缓存

> query_cache

> 将select的结果，存取起来共二次使用的缓存域

**操作方式**

```
#查看查询缓存状态
show variables like 'query_cache%';
#开启查询缓存
set global query_cache_type=1 ;
#设置查询缓存大小
set global query_cache_size=1024*1024*30;#单位：B 
```

**注意事项**

> 查询缓存存在判断是严格依赖于select语句本身的：严格保证SQL一致

>如果查询时包含动态数据，则不能被缓存

>  一旦开启查询缓存，MySQL会将所有可以被缓存的select语句都缓存。如果存在不想使用缓存的SQL执行，则可以使用 SQL_NO_CACHE语法提示达到目的
>
> ```
> select SQL_NO_CACHE * from {table-name}; #开启查询缓存后不缓存该条语句
> ```

### 4.锁

当客户端操作表（记录）时，为了保证操作的隔离性（多个客户端操作不能互相影响），通过加锁来处理。

**读锁**

> 又叫共享锁，乐观锁，S-lock
>
> 特征，阻塞其他客户端的写操作，不阻塞读操作。

```
SELECT * FROM {table_name} WHERE ... LOCK IN SHARE MODE #
```

**写锁**

> 又叫独占锁，排他锁，悲观锁，X-lock
>
> 特征，阻塞其他客户端的读，写操作。

```
SELECT * FROM {table_name} WHERE ... FOR UPDATE
```

**锁定粒度**

即锁的范围

**行级**

> 提升并发性，锁本身开销大

**表级**

> 不利于并发性，锁本身开销小。

## 架构层次

当单台MYSQL服务器无法满足当前网站流量时的优化方案。需要搭建mysql集群技术。

### 1.读写分离



### 2.主从复制

什么情况下使用主从复制

> 备份

> 读、写分离

> 跨地区的数据同步，这个一般使用环形

当向主服务器插入|修改|删除数据时，数据会自动同步到从服务器。

注意：主从复制是单向的，只能主 -> 从

#### 发射形

发射型（一主多从）：一般使用在：备份、读写分离。

#### 环形

环形（多主多从）：一般使用：当主服务器压力大时、跨地区的网站实现数据同步

在环形结构中，如果同时向三台服务器的同一表插入记录会出现“ID冲突的问题”。

解决办法：让三台服务器生成不同的ID；

第一台：1,4,7...

第二台：2,5,8..

第三台：3,6,9...

这个可以MYSQL的配置文件my.ini中设置：

```
auto_increment_increment=3;#每次加几
auto_increment_offset=1;#从几开始加
```

#### 主从的原理

Mysql中有一种日志叫做**bin日志**（二进制日志）。这个日志会记录下所有修改了数据库的SQL语句（insert,update,delete,ALTER TABLE,grant等等）。主从复制的原理其实就是把主服务器上的ＢＩＮ日志复制到从服务器上执行一遍，这样从服务器上的数据就和主服务器上的数据相同了。

#### 实际的配置

##### 主服务器

修改mysql的配置文件my.ini：

- 开启bin日志

```
log-bin=mysql-bin
```

-  为服务器指定一个server-id（主从服务器的ID值不能重复）

```
server-id=1
```

-  如果是**环形**的服务器需要添加以下项：

```
log-slave-updates = on   
```

登录MYSQL，执行SQL：

- 在主服务器上为从服务器创建一个用来同步数据的账号

```
#创建了一个只有REPLICATION SLAVE权限的账号：用户名：slave 密码：123456

GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' IDENTIFIED BY '123456';
```

- 在主服务器执行SQL查看主服务器当前bin日志的状态

```
show master status;

#file和postion对应值分别为从服务器SQL的master_log_file和master_log_pos配置
```

注意：每次修改数据时这两个值都会改变，所以在查看了这两个值之后，不要操作主服务器、直接到从服务器配置完成之后，否则这个值对应不上会同步失败。

##### 从服务器

修改mysql的配置文件my.ini：

- 开启bin日志

```
log-bin=mysql-bin
```

-  为服务器指定一个server-id（主从服务器的ID值不能重复）

```
server-id=2
```

-  如果是**环形**的服务器需要添加以下项：

```
log-slave-updates = on   
```

登录MYSQL，执行SQL：

- 设置从服务器并启动复制功能

```
stop slave;

change master to master_host='192.168.0.1',master_user='slave',master_password='123456',master_log_file='mysql-bin.000027',master_log_pos=372;

start slave;
```

- 查询从服务器的状态是否配置成功

```
show slave status;

#slave_IO_Running:Yes
#slave_SQL_Running:Yes
#上面两项显示yes则从服务器配置成功
```

- 在配置成功之前，主服务器上的数据不会自动到从服务器上来。所以需要在配置之前先把主服务器上的所有数据先手动的导到从服务器上来，然后配置完主从之后，数据以后就同步了。

### 3.负载均衡

## 合理sql

### 并发性的SQL

少用（不用）多表操作（subquery，join）

将复杂的SQL拆分多次执行。

### 查询缓存利用率

如果查询很原子（很小），会增加查询缓存的利用率

### 大量数据的插入

多条 insert

Load data into table

建议，先关闭约束及索引，完成数据插入，再重新生成索引及约束。

**针对于myisam**

```
Alter table {table-name} disable keys; #禁用索引约束

#大量数据插入

Alter table {table-name} enable keys; #启用索引约束
```

**针对innodb**

```
Drop index, drop constraint #注意要保留要保留主键

Begin transaction|set autocommit=0;#[数据本身已经按照主键值排序]

#大量数据插入

Commit;

Add index, add constraint
```

```
区分与每条记录的长度，以10量级为单位即可，不要过多。
多次执行相同结构别忘了prepare预编译的执行方式。

Insert into {table-name}values ();

Insert into {table-name}values ();

Insert into {table-name}values ();

Insert into {table-name}values ();

PK

Insert into {table-name}values (), (), (), (), ();
```

### 分页

Limit {offset}, {size};的使用，会大大提升无效数据的检索（被跳过）。

应该使用条件等过滤方式，将检索到的数据尽可能精确定位到需要的数据上。where {condition} limit {size};

Limit offset, size;

### rand()

尽量不要用order by rand()

建议，通过某种运算，先确定的随机主键，从数据表中获取数据。

## 慢日志

``` 
#查看慢日志
show variables like 'slow_query%';
#开启慢日志
set global slow_query_log =1 ;
#慢日志文件位置（一般就用默认的位置）
set global slow_query_file =D:... ;
#sql语句执行超过该时间临界点，就为慢查询
set global song_query_time  =10 ;#单位s
```

## my.ini

> max_connections  #支持的最大连接数

>key_buffer_size   #MyISAM键缓存大小

> table_cache    #MyISAM表缓存：缓存的是 表文件的句柄

> innodb_buffer_pool_size    #Innodb缓存，都是使用该缓存池例如，索引，事务日志缓存，等。