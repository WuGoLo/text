---
title: git介绍
toc: true
comments: true #是否可评论
categories:  #分类
- Git
tags:   #标签
- Git
---

## 	👉 git
>掌握git的常用命令操作
>能够使用代码托管平台

### 版本控制系统

##### 本地版本控制系统
借助软件我们可以记录下文件的每一次修改，如下图所示，文件被修改后，记录下了3个版本，
这样我们通过版本控制系统（软件）便可以非常方便的恢复到任意版本。如下图所示:
<!--more-->
![](media/01.jpg)
{% asset_img 01.jpg "" %}
这种类型的版本控制系统，功能比较单一，比如很难实现多人协同开发，所以现在几乎很少使用了。
##### 集中式版本控制系统
实际开发环境，一个项目通常是由多人协作共同完成的，如何让在不同终端上的开发者协同工作成了亟待解决的问题，
集中式版本控制系统便应运而生了。它通过单一的集中管理的服务器，保存所有文件的修订版本，
协同工作的开发者都通过客户端连到这台服务器，取出最新的文件或者提交更新。其代表为SVN，如下图所示:
![](media/02.jpg)
{% asset_img 02.jpg "" %}
这种方式很好解决了多人协同开发的问题，但是也有一个弊端，如果集中管理的服务器出现故障，将会导致数据（版本）丢失的风险，
另外协同开发者从集中服务器中更新数据时，严重依赖网络，如果网络不佳，也给开发带来诸多不便。

##### 分布式版本控制系统
分布式版本控制系统，则不需要中央服务器，每个协同开发者都拥有一个完整的版本库，
这么一来，任何协同开发者用的服务器发生故障，事后都可以用其它协同开发者本地仓库恢复。
由于版本库在本地计算机，也便不再受网络影响了。如果要将本地的修改，推送给其它协同开发者，
还需要一台共享服务器，所有开发者通过这台共享服务器同步和更新数据。如下图所示:
![](media/03.jpg)
{% asset_img 03.jpg "" %}
分布式版本控制系统弥补了前面两种版本控制系统的缺陷，成为了版本控制的首选方案。其代表就是Git。

### git准备工作
##### git简介

 Git最初是在2005年由Linux之父Linus TorvaLinus领导开发的一套为Linux内核维护的版本管理系统；
 Git日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标，它的速度飞快，极其适合管理大项目；
 它还有着令人难以置信的非线性分支管理系统，可以应付各 种复杂的项目开发需求。

##### git与svn的区别

- SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，
用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，
需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，
如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就纳闷了。

- Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，
这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，
那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，
你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

##### git安装

- Window安装
    + http://git-scm.com/download/win 下载Git客户端软件，和普通软件安装方式一样。
- Linux安装
    + CentOS发行版：sudo yum install git
    + Ubuntu发行版：sudo apt-get install git
- Mac安装
    + 打开Terminal直接输入git命令，会自动提示，按提示引导安装即可。

##### git工作原理
为了更好的学习Git，我们们必须了解Git管理我们文件的3种状态，
分别是已提交（committed）、已修改（modified）和已暂存（staged），
由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。
![](media/04.jpg)
{% asset_img 04.jpg "" %}
基本的Git工作流程如下：
1、在工作目录中修改文件。
2、暂存文件，将文件的快照放入暂存区域。
3、提交文件，找到暂存区域的文件，将快照永久性存储到Git仓库目录。

### git常见操作

##### 1.配置用户
```text
   git config --global user.name "zhoushugang"
   git config --global user.email "zhoushugang@itcast.cn"
```
配置用户的意义在于记录开发者信息，以便在版本控制记录开发者的操作行为。

注：--global 全局意思，不更换电脑不需要重复执行！！！

重复执行会覆盖以前的内容！！！！！！

##### 2.初始化仓库

**a)**需要将现有项目初始化为一个仓库：

```text
    git init
```
git init只是创建了一个名为.git的隐藏目录，这个目录就是存储我们历史版本的仓库。

执行此命令表示当前的项目可以被git工个来管理了！！！！！（一个项目只需要执行1次！！！）

**b)**已用git进行版本控制的仓库克隆到本地：
```text
    git clone git@gitee.com:zhoushugang/meituan.git
```
假如公司已有项目用了git，那我们就利用克隆，执行完这个命令，
会在当前目录下生成一个meituan目录（默认和仓库名称相同），这个便是已经使用git管理的项目。

##### 3.查看文件状态
新建文件后：已修改状态
```text
    git status
```
检测当前仓库文件的状态，注：git会忽略空的目录。

##### 4.添加文件到暂存区
```text
    //提交单个文件
    git add test.txt
    //提交单个目录文件
    git add css
    //提交所有文件
    git add .|*|-A
```
执行命令后：已暂存状态
修改文件后：已修改状态
```text
    git checkout test.txt
```
执行命令后：已暂存状态

##### 5.提交文件
将暂存区文件，全部提交到本地仓库存储。
```text
    // message 提交的备注信息
    git commit -m "message";
```
执行命令后：已提交状态

##### 6.查看提交历史
经过多次提交，查看提交历史。
```text
    git log
```

##### 7.恢复上一次提交
```text
    git reset --hard 242a
```

### git分支

在我们的现实开发中，需求往往是五花八门的，同时开发多个需求的情况十分常见，
比如当你正在专注开发一个功能时，突然有一个紧急的BUG需要你来修复，
这个时候我们当然是希望在能够保存当前任务进度，再去修改这个BUG，
等这个BUG修复完成后再继续我们的任务。如何实现呢？
**通过Git创建分支来解决实际开发中类似的问题。**

在Git的使用过程中一次提交称为历史记录（版本），并且会生成一个唯一的字符串，如下图：
![](media/05.jpg)
{% asset_img 05.jpg "" %}
当我们在初始化仓库的时候（实际上是产生第1次提交时），Git会默认帮我们创建了一个master的分支，并且有指针（HEAD）指到了末端。
指针（HEAD）用来标明当前处于哪个分支的哪个版本，如上图指的处于master分支的最后1个版本。

##### 1.创建分支 hotfix
```text
    git branch hotfix
```
新的分支会在当前分支原有历史版本的结点上进行创建，新建的子分支会继承父分支的所有提交历史，如下图:
![](media/06.jpg)
{% asset_img 06.jpg "" %}
##### 2.切换 hotfix 分支
```text
    git checkout hotfix
```
我们发现HEAD现在又指向了hotfix的末端，如下图:
![](media/07.jpg)
{% asset_img 07.jpg "" %}
##### 3.在 hotfix 修改bug
```text
    git add .
    git commit -m 'update bug'
```
这次的提交历史版本就会记录在hotfix这个分支上了，并且HEAD伴随hotfix在移动，如下图:
![](media/08.jpg)
{% asset_img 08.jpg "" %}
##### 4.切换 master 分支
```text
    git checkout master
```
当我们切换回master后，HEAD指向了master分支的末端，并且我们观察发现我们的文件内容还是原来的"模样"，如下图:
![](media/09.jpg)
{% asset_img 09.jpg "" %}
##### 5.在 master 继续开发
```text
        git add .
        git commit -m 'go on dev'
```
总结：当我们 'git checkout 分支名称'，HEAD会自动指向对应分支的末端，工作目录中的源码也会随之发生改变。
这个时候我们就在hotfix这个分支上修复了这个BUG，而我们原来在master分支上的操作并未受到影响。
但是这时的master分支并没有包含有hotfix的修复，如下图:
![](media/10.jpg)
{% asset_img 10.jpg "" %}
##### 6.合并 hotfix 分支
```text
    git merge hotfix
```
这时master会有两个父结点了，master便包含了hotfix里的修复了，如下图:
![](media/11.jpg)
{% asset_img 11.jpg "" %}
##### 6.删除 hotfix 分支
```text
    git branch -d hotfix
```
这时用来修复BUG创建的hotfix分支已经没有用处了，我们可以将它删除，如下图:
![](media/12.jpg)
{% asset_img 12.jpg "" %}

### git远程（共享）仓库
通过上面学习我们可以很好的管理本地版本控制了。
- 一种情形，可是如果我们下班回到家里突然来了灵感觉得有部分代码可以优化，
  如果能接着公司电脑上的代码继续写该有多好呀！！！
- 一种情形，假设项目比较大，不同的功能模块由不同的开发人员完成，不同模块儿之间又难免会依赖关系，
  这时如果我们的代码互相合并（融合）该有多好呀！！！

##### 1.创建共享仓库

Git要求共享仓库是一个以.git结尾的目录。

mkdir repo.git 创建以.git结尾目录

cd repo.git 进入这个目录

git init --bare 初始化一个共享仓库，也叫裸仓库 注意选项--bare

这样我们就建好了一个共享的仓库，但这时这个仓库是一个空的仓库，并且不允在这个仓库中进行任何修改。

##### 2.向共享仓库共享（同步）内容

将自已开发的项目同步到这个目录中，其它开发者就可以共享你开发的项目了。

1、进入到test目录

2、git push ../repo.git master

这样便把test中的项目同步进了repo.git中。

##### 3.从共享仓库里取出内容

1、新创建一个目录（模拟另一个开发者）

2、git clone ./repo.git demo

通过repo.git共享仓库，我们轻松得到了一个test的副本

##### 4.通过demo仓库向repo.git共享内容

进入到demo里，我们做一些修改

cd demo

git push ../repo.git master

##### 5.在仓库从repo.git获取共享的内容

cd test

git pull ../repo.git master

- 奇迹似乎发生了，我们轻松的将demo仓库里的内容，通过repo.git共享给了test仓库。
- 惊喜不断，问题也总是不断，我们发现我们这个共享的仓库只是放到了本地的，其它人是没有办法从我们这个共享仓库共享内容的！！！
- 然而现实是，办法总是有的，我们把这个共享的仓库放到一台远程服务器上，问题不就解决了吗？

##### 6.gitHub、gitLab和gitee(码云)

如果我们熟悉服务器的话，我们完全可以将上述的步骤在我们的远程服务器上进行操作，
然后再做一些登录权限的设置，就可非常完美的搭建一个共享服务器了。
其实为了更好的管理我们的仓库，一些第三方机构开发出了Web版仓库管理程序，通过Web界面形式管理仓库。

- ssh
- 简单说，SSH是一种网络协议，用于计算机之间的加密登录。如果一个用户从本地计算机，使用SSH协议登录另一台远程计算机，我们就可以认为，这种登录是安全的，即使被中途截获，密码也不会泄露。
- 配置步骤：
+ 使用git bash 执行 `ssh-keygen -t rsa `

+ 

###  一、首先，需要一个Git账号。

1、到GitHub上注册自己的账号。[https://github.com/](http://https//github.com/) 	

​	申请步骤基本和百度云盘差不多，还简单。

​	(1)、第一步：只需要输入用户名称，邮箱，密码

​	(2)、第二步：邮箱验证跳转

​	(3)、第三步就完成了。

  2、用刚注册的账号密码登陆Github

  3、点击右上角加号新建一个仓库【项目】

  ![](media\1.png)
{% asset_img 1.png "" %}
  4、填写完项目信息，点击create repository。如下图： 

  ![](media\2.png)
{% asset_img 2.png "" %}
  **注：**README.md建议先不选中，选中之后，新建的仓库中会新建一个说明文件README.md，链接本地git的时候需要必须pull到本地

~~~nginx
$ git pull --rebase origin master  // origin 为地址
$ git push -u origin master
~~~

  账户存在两个仓库的情况下可能会出现问题，**解决问题见下部的疑难问题解答 **

  **此时Github上就创建好了自己的一个仓库，可以理解为一个百度云的仓库，每个新建的仓库都会有一个地址链接**

  如图：![](media\8.png)
{% asset_img 8.png "" %}
  ### 二、其次，配置公钥和私钥

  **注**：如果准备使用上图仓库地址，这一步【二】可以省略，直接看第三步。

  1、安装并打开桌面gitBush软件，右键点击git bush here输入以下命令

` $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  `或者

`$ ssh-keygen -t rsa -C "youremail@example.com"`

// 引号内为Github账号的邮箱地址

  2、输入完成后，连续按三次回车，如果出现以下效果，则成功生成公钥和私钥

  截图如下：

  ![](media\10(2).jpg)
{% asset_img 10(2).jpg "" %}
  ```javascript
  代码分析：
  // 1、告诉你已经生成了公交和私钥  
  $ Generating public/private rsa key pair. 
  // 2、让你输入一个名字来保存内容，不用管，直接回车
  $ Enter file in which to save the key (/Users/lidaze/.ssh/id_rsa): 
  // 3、输入一次密码
  Enter passphrase (empty for no passphrase): 
  // 4、再输入一次密码
  Enter same passphrase again: 
  // 5、你的私钥已经保存在 /Users/lidaze/.ssh/id_rsa.
  Your identification has been saved in /Users/lidaze/.ssh/id_rsa.
  // 5.1、你的公钥已经保存在 /Users/lidaze/.ssh/id_rsa.pub.
  Your public key has been saved in /Users/lidaze/.ssh/id_rsa.pub.
  ```

  本地文件呈现截图

  ![](media\2.jpg)
{% asset_img 2.jpg "" %}
  3、复制公钥代码

  a、可以直接打开上图中的id_rsa.pub文件【通过记事本打开方式打开】

  ​	直接全选复制内部代码

  b、也可以输入命令：`$ pbcopy < ~/.ssh/id_rsa.pub `

  运行pbcopy < ~/.ssh/id_rsa.pub，可复制ssh key.

  **粘贴到页面的key中即可**。**title根据自己的需求填写，**

  字体加粗部分需要回到Github网站上操作如下：

  4、打开设置SSH key页面 ，账户头像下拉。

  ![](media\3.png)
{% asset_img 3.png "" %}
  依次如图操作

  ![](media\4.png)
{% asset_img 4.png "" %}
  复制的公钥**粘贴到页面的key中即可**。**title根据自己的需求填写，**

  ![](media\5.png)
{% asset_img 5.png "" %}
  最后点击Add SSH key。 

  **这样一个Github网站上的仓库就建立好了**

##   👉 处理git中的报错问题

警告一：

~~~cmd
warning：LF will be replaced by CRLF in ××××.××(文件名)
The file will have its original line ending in your working directory.

翻译：
在xxx.xx文件中LF将被CRLF替换。
在工作区（working directory）里，这个文件将会保持它原本的换行符。（line ending:行尾，换行）
~~~

解决：

注解：

​        **LF：Line Feed 换行**

​       **CRLF：Carriage Return Line Feed 回车换行键**

**1、不同操作系统下，处理行尾结束符的方法是不同的**：

　　windows下：CRLF（表示句尾使用回车换行两个字符，即windows下的"\r\n"换行）

​       unix下：LF（表示句尾，只使用换行）

​       mac下：CR（表示只使用回车）

**2、Git下处理“换行”（line ending）**

　　core.autocrlf是git中负责处理line ending的变量，可以设置3个值：true，false，inout。

（1）设置为true【config --global core.autocrlf **true**】

​          当设置成true时，这意味着你在任何时候添加(add)文件到git仓库时，git都会视为它是一个文本文件(text file)。

　　　它将把crlf变成LF。

（2）设置为false【config --global core.autocrlf **false**】

   　　当设置成false时，line endings将不做转换操作。文本文件保持原来的样子。

（3）设置为input时，添加文件git仓库时，git把crlf编程lf。当有人Check代码时还是lf方式。因此在window操作系统下，不要使用这个设置。

**3、更换电脑之后git clone出现：[Please make sure you have the correct access rights and the repository exists.问题的解决](https://www.cnblogs.com/cnlg123/p/7762889.html)**

原因：git 上ssh key的问题，两台电脑用的ssh key不同导致

解决：

1. 删除.ssh文件夹（直接搜索该文件夹）下的known_hosts(手动删除即可，不需要git）

2. 在下载好的Git中的bin目录下打开bash.exe输入命令ssh-keygen -t rsa -C "username" (注：username为你git上的用户名)，如果执行成功。返回：Generating public/private rsa key pair.Enter file in which to save the key (/Users/username/.ssh/id_rsa): //这里的username是电脑上的用户名，这个地址也是文件的存储地址，然后我们按回车，如果你以前有存储地址会返回/Users/your username/.ssh/id_rsaalreadyexists.Overwrite (y/n)?直接输入y回车。如果以前没有储存地址就会出现Enter passphrase(empty for no passphrase);也直接回车，两种情况回车后都会出现 Enter same passphrase again 然后接着回车会显示一长串内容其中还有一些..o.. o oo .oS. 之类的代码，这说明SSH key就已经生成了。文件目录就是：username/.ssh/id_rsa.pub.

 

3. 然后找到系统自动在.ssh文件夹下生成两个文件，id_rsa和id_rsa.pub，用记事本打开id_rsa.pub将全部的内容复制。

4. 打开https://github.com/，登陆你的账户，进入设置（Settings）找到![](Git使用笔记\1239999-20171031180654121-1601381633.jpg)

   ![](Git使用笔记\1239999-20171031181008293-1652901573.jpg)

5. 然后将你复制的内容粘贴到key中

   ![img](F:\hexo\source\_posts\React基础笔记\images\1239999-20171031181147121-1513370965.jpg)

   再点击Add SSH Key           

   ok还有最后一步

6. 仍然在bash.exe中输入ssh -T git@github.com然后会跳出一堆内容你只需输入yes回车就完事了，然后他会提示你成功了。

  然后你就可以正常使用git clone命令了，最后祝你好运哈哈！