# Git系统学习

<p style="color:red">
  本文档夹带私货
  笔者是个菜🐔，如有问题，欢迎指出啊👏
<p/>

## Git基础与实操

### 版本控制工具简介

#### 起源--diff and patch

diff和patch是非常原始的版本控制工具，但从现在的git上面我们依然能看到diff 和 patch 的影子，很多思想都是一脉相承的。

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210325185525263.png" alt="image-20210325185525263" style="zoom:50%;" />

比如上图中比较两个文件差异的返回格式，如果我没记错的话，git工具中的返回格式基本与其一样。

patch就是一个基本的版本跳转工具，但是它的使用和现在的git相比就费劲太多了。

#### 版本控制工具的大规模使用--CVS & SVN

这俩东西从来没见过，如果真的在以后的项目中遇到这种版本控制工具，记得这俩都是集中式管理工具，安全性堪忧，然后CVS操作并非原子的，SVN优化了多人提交时的数据混乱问题应该就行了。

#### 开源分布式管理工具--Git诞生

在分布式系统中，存储资源并不算珍贵，因此Git存储着每一个版本的全部数据，来提供高可靠性。

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210325191121080.png" alt="image-20210325191121080" style="zoom:50%;" />

上图在某种程度上起时就是集中式系统和分布式系统的对比。

### Git安装与配置

#### Linux

Ubuntu10.10+/Debain

```shell
sudo aptitude install git #安装git的核心功能
sudo aptitude install git-doc git-svn git-email gitk #安装相关扩展功能
```

RHEL/Fedora/CentOS

```shell
yum install git #安装git的核心功能
yum install git-svn git-email gitk #安装扩展包
```

个人👤不推荐源码安装，软件管理包yyds，不过可以复习一下Linux命令

tar 用于解压/压缩文件 [扩展1](https://blog.csdn.net/kkw1992/article/details/80000653) [扩展2](https://blog.csdn.net/qq_40232872/article/details/79159753)

make命令没有接触过，似乎是一个第三方提供的编译工具（怀疑课件里面少了个yum install make 【狗头保命】）这个命令就不扩展了

整个源码安装过程：

```shell
#官网下载安装包 wget -b <官网下载链接> 注，尖括号<> 内为必填内容，方括号[] 内为选填内容
tar -jxvf <安装包> #-j 支持bzip2解压文件,-x 从压缩的文件中提取文件,-v 显示操作过程,-f 指定压缩文件
cd <解压目录>
make prefix=usr/local all
sudo make prefix=usr/local install
#下面开始安装文档
make prefix=usr/local doc info
sudo make prefix=usr/local install-doc install-html install-info
```

命令补全偷个懒，直接上图

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210325195649714.png" alt="image-20210325195649714" style="zoom:33%;" />

#### Windows

[贴个链接得了](https://git-scm.com/download/win)

emm，不推荐图形化管理工具，不过没咋在Windows下开发过，可能很好用，所以...[贴个链接](http://code.google.com/p/tortoisegit/)

#### Git配置

git为配置文件提供了三个生效级别：

1. 本机下所有用户均生效；<span style="color:red">优先级最低</span>
2. 本机下某用户均生效；<span style="color:red">优先级第二</span>
3. 单一项目生效；<span style="color:red">优先级最高</span>

这就使我们可以灵活的配置git。（优先级问题猜测是配置文件加载顺序造成的？从上到下依次加载，导致项目级别优先级最高。当然这个设计非常合理，这里就是瞎bb）

```shell
git config --system <具体配置的key> <配置的value> #系统级别配置
git config --global <具体配置的key> <配置的value> #用户级别配置
git config --local <具体配置的key> <配置的value> #项目级别配置
```

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210325201219875.png" alt="image-20210325201219875" style="zoom: 50%;" />

##### 各种配置配置

```shell
#个人信息配置
git config --global user.name "your name"
git config --global user.email "your email"
#文本换行符配置
git config --global core.autocrlf true #crlf与lf转换，true时代表本地为crlf而远端为lf
git config --global core.autocrlf input #本地不转换，远端将crlf转换为lf
git config --global core.autocrlf false #不转换
#中文编码支持
git config --global gui.encoding utf-8 #编码字符集
git config --global i18n.commitEncoding utf-8 #提交字符集
git config --global logOutputEncoding utf-8 #输出字符集
#中文路径显示配置
git config --global core.quotepath false
#认证配置
git config --global credential.helper store #设置缓存口令
git config http.sslverify false
```

### Git基本命令

首先介绍一下Git对于文件管理的三个分区

- 工作区：我们日常工作时显示可见的项目文件
- 暂存区：我们执行git add . 后，所有改动的文件被存储到暂存区
- 版本库：执行git commit -a 后，暂存区文件被提交到版本库

值得注意的是，<span style="color:red">工作区、暂存区和版本库都是基于本地的</span>

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210331150409530.png" alt="image-20210331150409530" style="zoom:50%;" />

 [上图详解](https://www.runoob.com/git/git-workspace-index-repo.html)

```shell
git	init [projectName]               #初始化，创建工作区与版本库  注：尖括号<>内为必填内容，方括号[]内为选填内容

git	add <fileName.txt>					     #将某文件加入git追踪
	  commit -m <message>				       #提交

git	status 							             #显示现有状态

git	diff [HEAD --] <fileName.txt>		 #显示现有状态与最后提交状态的不同
	  reset --hard HEAD^[^…]           #更改头指针（向前1【+n】个版本）
	  reset --hard <commit id>			   #更改头指针（指向commit ID）
	  log[ --pretty=oneline]				   #显示历史版本 
	  reflow							             #显示历史命令操作
    checkout <file-name> 				     #撤销未提交的修改

git remote add <origin> git@github.com:shanheguren/<repository name>
                                     #关联远程库
    push [-u] <origin> master        #推送master分支所有内容
git clone <adress>                   #克隆adress的库中所有内容

git branch -b <branch-name>			     #创建分支并切过去
	  checkout <branch-name>			     #切换到branch-name支流
 	  merge [--no-ff] [-m <message>] <branch-name>  				 
             								         #合并branch-name与master
    rebase <source> <target>         #将source的分支节点中不同的几次提交直接复制到target中，一般不建议使用
    branch -d <branch-name>			     #删除branch-name支流
git switch -c <branch-name>			 
	  checkout -b <branch-name>			   #创建并切换到branch-name分支
	  switch <branch-name>				     #切换到已有分支

git clone git@github.com:michaelliao/bootstrap
git remote -v                        #显示所有远程库
git rm -r --cached .                 #清除缓存

```

### 本地基本提交推送+分支上开发与日志查看

练习课，感觉没啥可记的东西，记录几条命令吧

```shell
git commit --amend 				#用于修改提交的备注信息
git log [-N]        			#查看最近N次的日志
git log since <timestamp> #查看自timestamp起的日志
git chery-pick <commitId> #从其他分支pull一个提交并生成新的commitId
```

### 冲突处理

我直接上IDEA，IDEA的冲突处理插件真的好用

### SSH配置

Setting->SSH Keys->generate one

记得改邮箱

### Group & Project

group是project集合，project间关联紧密

Member中可添加组织成员，添加时可配置成员权限。

### Fork & Merge Request

<span style="color:red">不允许直接将自己的代码推送到组织仓库</span>

需要在自己的Fork仓库中完成代码推送，然后通过Merge Request进行推送。

Merge Request时需要配置Assignee来做审核，审核本次提交是否被允许

### Merge Request 解决冲突

分为本地解决和线上解决两种方法。

本地可解决所有冲突，线上对于部分冲突无力修改，比如本地删除了某个文件，而merge的目标库对该文件做了修改，此时线上无法解决

本地解决时，将目标分支pull下来，然后手动处理冲突，处理完成后在执行一次push命令即可完成冲突处理

### Merge Request检视点

emmm，开箱即用....没啥好记的东西

### Issue

项目管理工具？

## Git深入学习

to be continued





