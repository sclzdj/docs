# SVN

## 简介

官网：https://tortoisesvn.net

SVN: subversion  子级版本  (子级源代码版本控制管理软件)

svn:全称Subversion，是代码版本管理软件，管理着随时间改变的数据。这些数据放置在一个中央资料档案库 (repository) 中。 这个档案库很像一个普通的文件服务器, 不过它会记住每一次文件的变动。这样你就可以把档案恢复到旧的版本, 或是浏览文件的变动历史。 许多人会把版本控制系統想像成某种 “时光机器”。

使用svn可以很好地协调一个团队共同开发同一个项目，而不会出现代码冲突、覆盖的情况。

## 作用

① 多人开发同一个项目不会出现代码覆盖情况。

② 针对一个文件可以创建许多不同版本，并且可以随时查看不同版本的内容。

③ 公司领导可以通过svn查看每个人的工作情况

## 安装

分为服务端和客户端

下载地址：https://tortoisesvn.net/downloads.html

## 基本使用

#### 创建仓库

服务端命令，应该进入服务器端安装文件夹再执行

```
svnadmin  create  h:/svnServer/app/shop
```

db/revprops     是日志信息文件目录

db/revs     是存储源程序文件目录（以仓库本身格式存储）

#### 启动仓库服务

服务端命令，应该进入服务器端安装文件夹再执行

该svn服务走svn协议，端口号码是3690

如果是在windows系统用cmd窗口开启的，开启后就不要关闭该窗口，linux系统则开启后台运行

```
svnserve  -d(独立端口运行)  -r(仓库地址)   仓库地址
svnserve  -d  -r  h:/svnServer/app/shop     //启动shop仓库服务
```

#### 本地客户端与仓库连接

- 右键------> checkout

- 在对话框中输入svn主机名和本地目录后点击确定
- 连接成功后会在本地目录多一个.svn文件夹；并且会把**仓库所有文件克隆到本地目录**

#### 初次操作

① 右键------> SVN---->Add    (本地的.svn对该文件形成管理)

② 右键------> Commit   (本次的文件提交给svn仓库)

③ 可以在空白的矩形区域写上注释再提交

这时可能会出现权限问题，可以看下面的 文档解决

#### 初次使用

① 第一次使用svn，执行checkout指令，与svn仓库取得联系的同时并把仓库的全部文件更新到本地

② 本地文件提交给svn仓库，执行commit指令

③ 仓库最新的程序文件更新到本地，执行update指令

后期Commit和Update是频繁使用的指令

#### 更新仓库文件到本地

 右键------> Update g

#### 匿名账号权限

进入服务端仓库文件夹，打开conf/svnserver.conf文件

```
anon-access=write   //开启匿名账号权限，去掉前面的#号
```

#### 同一文件的不同版本切换

一个程序文件可以在svn仓库里边形成许多不同版本，并可以随时查看。

```
右键---> TortoiseSVN---> Show Log    #查看版本日志

右键---> TortoiseSVN---> Update to revision---> 对话框中的Show Log  #将文件更新到具体的某个版本
```

#### 同一文件的两个版本对比

```
右键---> TortoiseSVN---> Show Log--->对话框选中两个版本点击右键--->Compare revisons #对比
```

## 冲突解决

#### 文件覆盖的解决

① 给每个文件分配一个“令牌”，谁拿到令牌谁就有权利开发该文件

(同一个程序文件同一个时间点只允许有一个人开发)

② 给每个文件设置一个版本号码，提交的时候如果服务器的版本等于本地版本号码就允许提交，否则不允许提交(本地号码 小于  服务器版本)

#### 什么是冲突

广义角度的冲突，提交程序文件 本地版本号码 小于 服务器版本号码

狭义角度的冲突，多个程序员对同一个文件同一处代码的修改再共同提交文件的时候回产生冲突。

#### 解决方法

##### 修改的代码不在同一处

报文件过时的提示。

执行update操作，把仓库最新的文件更新到本地，并和本地文件做Merge融合操作（svn自动智能融合）。继续提交文件即可。

##### 修改的代码在同一处

报文件有冲突的提示。

解决就是通过update把最新的版本更新到本地，文件稍作修改后继续提交。

更新到本地后会生成4个文件，示例index.php

- index.php   合并后的文件（需要稍作修改的文件，修改好后把下面三个文件删除后在提交即可）
- index.php.mine   本地修改后的文件
- index.php.r34   大家都没修过前的文件
- index.php.r35   对方修改后的文件

## 多仓库运行

#### 同时启动多个仓库服务

把所有仓库的**上级目录**当成服务给启动起来。

```
svnserve  -d  -r  d:/svnServer/app/

主机名：svn://localhost  ---------------->app目录

svn://localhost/student---------->与student仓库取得联系

svn://localhost/book------------->与book仓库取得联系

svn://localhost/shop-------------->与shop仓库取得联系
```

#### 更换svn主机名

- 删除.svn文件，断开与仓库的联系

- 重新checkout通过svn新主机名建立与仓库的联系

## 文件颜色标志

① 蓝色加号：本地的.svn对该文件有形成管理

② 绿色对号：本地文件、.svn管理的版本文件、仓库文件 三者一致

③ 红色叹号：本地文件  与  .svn和仓库文件 不一致(用户自己修改了该文件)

④ 黄色叹号：表示该文件正处于冲突状态

## 账号和权限

服务端配置文件

conf/authz  权限配置文件（默认未开启）

conf/passwd  账号和密码配置文件（默认未开启）

conf/svnserve.conf     svn的主配置文件

#### 设置账号

- 开启authz和passwd；打开conf/svnserve.conf文件，去掉下面两行前面的#号

  ```
  password-db = passwd
  
  authz-db = authz
  ```

- 创建两个账号；打开conf/passwd文件，[users]下面加上

  ```
  mary = mary123
  linken = linken123
  ```

#### 设置权限

##### 读写权限

打开conf/authz 文件；设置用户权限，r是读，w是写

```
#单仓库
#[/]
#linken = rw
#mary = r
#* =   

#多仓库
[shop:/]
linken = rw
mary = r
* =   

#其中* =这个代表其他用户没有任何权限
```

##### 分组权限

打开conf/authz 文件；设置分组成员，其中组内成员需要先创建好账号

```
#设置分组
[groups]
group1 = zhangsan,lisi,wangwu
group2 = zhangsan2,lisi2,wangwu2

[shop:/]
@group1 = rw
@group2 = r
linken = rw
mary = r
* =   

#其中@group1 = rw这个代表group1组中的所有成员具有读写权限
```

##### 目录权限

打开conf/authz 文件；设置目录权限，下面示例只开启wechat目录的权限（其中这个wechat目录必须先创建好后提交到仓库）；注意这里用户checkout时要关联到具体目录，例：svn://localhost/shop/wechat

```
#单仓库
#[/wechat]
#linken = rw
#* =   

#多仓库
[shop:/wechat]
linken = rw
* =   
```

## 开启启动项服务

只适合windows系统设置方式，直接运行`services.msc`，可查看本地服务设置 

设置好后，服务端就不用一直挂在窗口，会像mysql一样后台运行

打开cmd直接输入，其中binPath=和start=后面的空格不能少

```
sc create 服务名称 binPath= “安装目录/svnserve.exe -r 版本库地址目录  --service”  start= auto

#开启svn启动项服务
sc create svnd binPath= “d:/svnServer/server/bin/svnserve.exe -r d:/svnServer/app  --service” start= auto

#删除svn启动项服务
sc delete svnd
```

注：如果遇到windows账号权限问题运行不成功的话，可以分别把上面的代码输入到svnd-create.bat文件和svnd-delete文件中，然后右键选择以管理员身份运行

## 清除缓存

右键---> TortoiseSVN---> Settings--> Saved Data  

里面有很多项缓存可以清除，可以选择全部清除

## linux命令

#### 常用命令

**1、将文件checkout到本地目录** 
svn checkout path（path是服务器 上的目录）
例如：svn checkout svn://192.168.1.1/pro/domain
简写：svn co

**2、往版本库中添加新的文件** 
svn add file
例如：svn add test.php(添加test.php)
svn add *.php(添加当前目录下所有的php文件)

**3、将改动的文件提交到版本库** 
svn commit -m “LogMessage“ [-N] [--no-unlock] PATH(如果选择了保持锁，就使用–no-unlock开关)
例如：svn commit -m “add test file for my test“ test.php
简写：svn ci

**4、加锁/解锁** 
svn lock -m “LockMessage“ [--force] PATH
例如：svn lock -m “lock test file“ test.php
svn unlock PATH

**5、更新到某个版本** 
svn update -r m path
例如：
svn update如果后面没有目录，默认将当前目录以及子目录下的所有文件都更新到最新版本 。
svn update -r 200 test.php(将版本库中的文件test.php还原到版本200)
svn update test.php(更新，于版本库同步。如果在提交的时候提示过期的话，是因为冲突，需要先update，修改 文件，然后清除svn resolved，最后再提交commit)
简写：svn up

**6、查看文件或者目录状态** 
1）svn status path（目录下的文件和子目录的状态，正常状态不显示）
【?：不在svn的控制中；M：内容被修改；C：发生冲突；A：预定加入到版本库；K：被锁定】
2）svn status -v path(显示文件和子目录状态)
第一列保持相同，第二列显示工作版本号，第三和第四列显示最后一次修改的版本号和修改人。
注：svn status、svn diff和 svn revert这三条命令在没有网络的情况下也可以执行的，原因是svn在本地的.svn中保留了本地版本的原始拷贝。
简写：svn st

**7、删除 文件** 
svn delete path -m “delete test fle“
例如：svn delete svn://192.168.1.1/pro/domain/test.php -m “delete test file”
或者直接svn delete test.php 然后再svn ci -m ‘delete test file‘，推荐使用这种
简写：svn (del, remove, rm)

**8、查看日志** 
svn log path
例如：svn log test.php 显示这个文件的所有修改记录，及其版本号的变化

**9、查看文件详细信息** 
svn info path
例如：svn info test.php

**10、比较差异** 
svn diff path(将修改的文件与基础版本比较)
例如：svn diff test.php
svn diff -r m:n path(对版本m和版本n比较差异)
例如：svn diff -r 200:201 test.php
简写：svn di

**11、将两个版本之间的差异合并到当前文件** 
svn merge -r m:n path
例如：svn merge -r 200:205 test.php（将版本200与205之间的差异合并到当前文件，但是一般都会产生冲突，需要处理一下）

**12、SVN 帮助** 
svn help
svn help ci

#### 扩展命令

**13、版本库下的文件和目录列表** 
svn list path
显示path目录下的所有属于版本库的文件和目录
简写：svn ls

**14、创建纳入版本控制下的新目录** 
svn mkdir: 创建纳入版本控制下的新目录。
用法: 1、mkdir PATH…
2、mkdir URL…
创建版本控制的目录。
1、每一个以工作副本 PATH 指定的目录，都会创建在本地端，并且加入新增
调度，以待下一次的提交。
2、每个以URL指定的目录，都会透过立即提交于仓库中创建。
在这两个情况下，所有的中间目录都必须事先存在。

**15、恢复本地修改** 
svn revert: 恢复原始未改变的工作副本文件 (恢复大部份的本地修改)。revert:
用法: revert PATH…
注意: 本子命令不会存取网络，并且会解除冲突的状况。但是它不会恢复
被删除的目录

**16、代码 库URL变更** 
svn switch (sw): 更新工作副本至不同的URL。
用法: 1、switch URL [PATH]
2、switch –relocate FROM TO [PATH...]

1、更新你的工作副本，映射到一个新的URL，其行为跟“svn update”很像，也会将
服务器上文件与本地文件合并。这是将工作副本对应到同一仓库中某个分支或者标记的
方法。
2、改写工作副本的URL元数据，以反映单纯的URL上的改变。当仓库的根URL变动
(比如方案名或是主机名称变动)，但是工作副本仍旧对映到同一仓库的同一目录时使用
这个命令更新工作副本与仓库的对应关系。
我的例子：svn switch --relocate http://59.41.99.254/mytt http://www.mysvn.com/mytt 

**17、解决 冲突** 
svn resolved: 移除工作副本的目录或文件的“冲突”状态。
用法: resolved PATH…
注意: 本子命令不会依语法来解决冲突或是移除冲突标记；它只是移除冲突的
相关文件，然后让 PATH 可以再次提交。

**18、输出指定文件或URL的内容。** 
svn cat 目标[@版本]…如果指定了版本，将从指定的版本开始查找。
svn cat -r PREV filename > filename (PREV 是上一版本,也可以写具体版本号,这样输出结果是可以提交的)

**19、 查找工作拷贝中的所有遗留的日志文件，删除进程中的锁 。**

当Subversion改变你的工作拷贝（或是.svn 中 的任何信息），它会尽可能的小心，在修改任何事情之前，它把意图写到日志文件中去，然后执行log文件中的命令，然后删掉日志文件，这与分类帐的文件系统 架构类似。如果Subversion的操作中断了（举个例子：进程被杀死了，机器死掉了），日志文件会保存在硬盘上，通过重新执行日志文 件，Subversion可以完成上一次开始的操作，你的工作拷贝可以回到一致的状态。

这就是svn cleanup 所作的：它查找工作拷贝中的所有遗留的日志文件，删除进程中的锁。如果Subversion告诉你工作拷贝中的一部分已经“锁定 ”了，你就需要运行这个命令了。同样，svn status 将会使用L 显示锁定的项目：

$ svn status
L somedir
M somedir/foo.c

$ svn cleanup
$ svn status
M somedir/foo.c

**20、 拷贝用户的一个未被版本化的目录树到版本库。**
svn import 命令是拷贝用户的一个未被版本化的目录树到版本库最快的方法，如果需要，它也要建立一些中介文件。

$ svnadmin create /usr/local/svn/newrepos $ svn import mytree file:///usr/local/svn/newrepos/some/project Adding mytree/foo.c Adding mytree/bar.c Adding mytree/subdir Adding mytree/subdir/quux.h Committed revision 1.

在上一个例子里，将会拷贝目录mytree 到版本库的some/project 下：

$ svn list file:///usr/local/svn/newrepos/some/project bar.c foo.c subdir/

注意，在导入之后，原来的目录树并没有 转化成工作拷贝，为了开始工作，你还是需要运行svn checkout 导出一个工作拷贝。

**另附：为SVN 加入Email通知** 
可以通过Subversion的Hook脚本的方式为SVN 加入邮件列表功能 
编译安装了Subversion后 在源码的tools 下有一个comm-email.pl的Perl脚本，在你的档案目录下有一个hooks目录，进入到hooks目录把post-commit.tmpl 改名为post-commit并给它可执行的权限。 
更改post-commit脚本 把comm-email.pl脚本的决对路径加上，否则 SVN 找不到comm-email.pl 

REPOS="$1" 
REV="$2" 
/usr/local/svn /resp/commit-email.pl "$REPOS" "$REV" email@address1.com email@address2.com 
\#log-commit.py --repository "$REPOS" --revision "$REV" 

最后一行是用来记日志的 我不用这个功能 所以注释掉了

​	