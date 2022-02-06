# git版本控制系统学习

![image-20220206151350525](C:\Users\JJJ\AppData\Roaming\Typora\typora-user-images\image-20220206151350525.png)

## 0、常用指令

```c++
//将本地的分支版本上传到远端并合并
git push [远端仓库代号A] [本地分支名]:[远程分支名]
当本地分支名与远程分支名相同，则可以省略冒号：
git push [远端仓库代号A] [本地分支名]
默认远端仓库代号是origin。
git remote add [自己起的远程仓库代号A] [远程仓库地址]		//在本地仓库添加一个远程仓库 这条指令可以修改远端仓库代号

fetch与pull的区别：pull=fetch + merge
git fetch [关联的远端仓库代号A] [分支名]
git pull [关联的远端仓库代号A] [分支名]	//拉取并与本地分支进行merge
    
//merge远端分支与本地分支：
git merge [要merge的远端仓库代号A]/[远端仓库的分支]
 
//复制远端仓库到本地
git clone [远端仓库] [本地目录]
    
git checkout -b	[dev1] [dev2]	//基于dev2分支建立分支dev1并切换到分支dev1
   
git add [filename]  //将文件由工作目录添加到暂存区
git commit -m'变更理由'  //提交暂存区的文件到版本历史
```

**本地创建远端存在的feature分支：**
第一种情况：
1、git branch -av
      //

* master                  9fc2dac add woaini
  remotes/origin/master 9fc2dac add woaini
  //
  **如果remotes也没有feature分支时，用git fetch** **[远端仓库代号A]**
  2、git fetch [远端仓库代号A]
  3、git branch -av
  //
  
* master                   9fc2dac add woaini
  remotes/origin/feature ff95b00 add git command
  remotes/origin/master  9fc2dac add woaini
  //
  
  4、**再用git checkout -b feature origin/feature 即可创建本地feature分支。**

第二种情况：
本地仓库是远端git clone来的
1、git branch -av
      //

* master                     17f0864 Initial commit
  remotes/origin/feature   17f0864 Initial commit
  remotes/origin/master    9fc2dac add woaini
  //
  **如果remotes已有feature分支时，用git checkout -b feature origin/feature 即可创建本地feature分支。**
  
  这个知识点与上面的图对应了。

**git push时需要注意远端仓库有无更新**：

远端有更新时，首先git fetch [远端仓库代号A]，

再git merge [要merge的远端仓库代号A]/[远端仓库的分支]

最后再git push.

**开发前与远端同步**：git pull

## 1、git配置

```c++
git config [--global --local --system] user.name 'your name'
git config [--global --local --system] user.email 'your email'
git config [--global --local --system] --list  //查看配置信息
    
local：区域为本仓库(常用)
global: 当前用户的所有仓库(常用)
system: 本系统的所有用户
```

## 2、创建仓库

```c++
git init +项目名/仓库名  //创建项目仓库
 
git add [filename]  //将文件由工作目录添加到暂存区（重要）
    
git status		//查看暂存区的状态
    
git commit -m'变更理由'  //提交暂存区的文件到版本历史
git commit -am'变更理由' //直接提交工作目录的文件到版本历史 
    
git add -u(update)	//所有已经之前被git管理的（已经提交到暂存区的）文件 重新更新到暂存区，不需要add具体的文件名称。如某个文件被修改后重新提交到暂存区。
git add .	//将工作空间新增和被修改的文件添加到暂存区，包括没有被git管理的文件。
```

## 3、git log查看版本演变历史

```c++
git log		//查看当前分支git历史
    
git log	--all	//查看所有分支git历史

git log --oneline	//查看单行的简洁历史
   
git log -n2		//查看最近两次历史
    
git log --oneline --all -n4 --graph //查看所有分支最近4条单行的图形化历史。
    
git branch -v	//查看本地有多少分支
    
git checkout [dev]	//切换到其他分支
    
git checkout -b	[dev]	//新建分支并切换到分支
    
git checkout -b	[dev1] [dev2]	//基于dev2分支建立分支dev1并切换到分支dev1
    
git log --all --graph	//查看图形化的log地址
    
git help --web log		//跳转到git log的帮助文档网页
```

## 4、给文件重命名的简便方法：git mv

```c++
mv [filename] [filename1]	//将filename重命名为filename1

git rm [filename]		//从git中删除文件
    
git add [filename]		//添加文件到暂存区
    
以上三条等同于一条：git mv [filename] [filename1]
```

```c++
git reset --hard （HEAD）	//清除暂存区和工作区上的所有变更.
    
git reset --hard + commit的哈希值	//消除最近几次的提交，并清除对应的暂存区和工作区的内容
```

## 5、通过图形界面工具来查看版本历史

```c++
gitk --all		//查看所有分支的图形化界面
```

## 6、探秘.git目录

**几个重要的文件夹：**

HEAD：存放的是当前处于哪个分支；

config：存放的是本地仓库（local）的配置信息；

refs/heads：存放分支；

refs/heads/master/:指向master分支最后一次commit；

refs/tags:存放tag。当这次提交是具有标志性的，一般会打一个tag；

**objects**：核心文件、存储文件。commit、tree、blob三种类型。

```c++
cat + [filename]	//查看文件内容
    
git cat-file		//显示版本库对象的内容、类型及大小信息。
    
git cat-file -t [f70b420e59b哈希值]//获取版本库对象的类型
    
git cat-file -s b44dd71d62a5a8ed3 //显示版本库对象的大小
    
git cat-file -p b44dd71d62a5a8ed3 //显示版本库对象的内容
    
find [路径] 				//查看当前目录下文件列表
```

git里几个核心的对象类型有commit、tree、blob三种。

```c++
commit:提交时的快照
tree:文件夹
blob:文件			//不管文件名是什么，只要内容相同的文件都会存在一个blob里，节省存储空间。
```

当向暂存区里添加文件/文件夹时，git就会在object里创建blob文件。

## 7、删除分支

```c++
git branch -D [分支名]
```

## 8、修改最新commit的message

```c++
git commit --amend
```

## 9、修改老旧commit的message

```c++
git commit rebase -i [要更改的commit的上一级commit]
```

接下来是交互的过程。。。

## 10、如何将连续的多个commit整理成1个

```c++
git rebase -i [最初的commit]		//变基

pick  7735d66 update #合并到该commit上

squash bbe6d53 update

squash 9eb3188 update

squash 7d33868 update
```

## 11、比较暂存区和HEAD所含文件的差异

```c++
git diff --cached
```

## 12、比较工作目录和暂存区里文件的差异

```c++
git diff			  //对当前分支所有文件比较
git diff -- [文件名]	//对某个文件进行比较
    
git diff [分支1] [分支2]	//比较两个分支的差异
git diff [commit1的哈希值] [commit2的哈希值] --[w文件名]		//比较两次commit的差异
```

## 13、将暂存区的内容（reset）恢复成和HEAD的一样

```c++
git reset HEAD		//将暂存区所有的内容恢复
    
git reset HEAD -- [文件1] [文件2]	//将暂存区指定的文件恢复
```

## 14、将工作目录里的内容(checkout)恢复为和暂存区的一致

```c++
git checkout -- [文件名]
```

## 15、删除暂存区的内容

```c++
git rm [文件名]	//删除暂存区的内容
```

## 16、开发中临时加塞了紧急任务如何处理？

```c++
工作区内容正在修改，但是此时线上bug需要修复，则：
git stash		//临时栈区存储工作区正在编辑的内容
    
git stash pop与git stash apply的区别：
git stash pop：线上任务修复完后，用此指令恢复到正在编辑的内容，从栈区恢复到工作区。stash列表不保留。
git stash apply：stash列表里保留。
```

## 17、指定不需要git管理的文件

```c++
rm -rf [文件夹名]		//删除文件夹
    
①建立一个.gitignore文件
②在.gitignore文件里填写文件名或者文件夹名称，表明该文件名或者文件夹不被git管理
```

## 18、将git仓库备份到本地

哑协议与智能协议：

```c++
哑协议：git clone --bare /c/Users/JJJ/git_learning/.git ya.git		
智能协议：
git clone --bare file:///c/Users/JJJ/git_learning/.git zhineng.git
--bare表示不复制工作区的内容；
智能协议多加了file://，传输速度更快，且有传输进度。

git clone [远端仓库] [本地目录]
```

**在不同仓库之间发生关系，必须使用remote作为桥梁：**

```c++
git remote -v		//查看有多少个远程仓库
    
git remote add [自己起的远程仓库代号A] [远程仓库地址]		//在本地仓库添加一个远程仓库
```

## 19、github配置公私钥

```c++
查看目录列表是否已有SSH密钥：
ls -al cd ~/.ssh
    
配置公私钥：
①ssh-keygen -t rsa -b 4096 -C "your email"	//生成公私钥 	
    
②cat id_rsa.pub		//查看公钥
id_rsa	//私钥名称
    
③在github的设置里添加SSH keys,将公钥粘贴到这儿。
```

## 20、把本地仓库同步到github

```c++
//第一步，建立本地和远端仓库的连接
git remote add [自己起的远端仓库代号A] [github仓库的SSH地址]

git fetch:把远端分支拉到本地，不会与本地分支产生关联。
git pull:把远端分支拉到本地，并且与本地分支merge。
    
git fetch [关联的远端仓库代号A] [分支名]

//merge远端分支与本地分支：
git checkout [分支名]		//本地切换到需要merge的分支
git merge [要merge的远端仓库代号A]/[远端仓库的分支]

若两个要merge的分支没有公共祖先时：
git merge --allow-unrelated-histories [要merge的远端仓库代号A]/[远端仓库的分支]
 
//将本地的分支版本上传到远程并合并
git push [远端仓库代号A] [本地分支名]:[远程分支名]
当本地分支名与远程分支名相同，则可以省略冒号：
git push [远端仓库代号A] [本地分支名]
```

## 21、变更了文件名git仍然可以智能识别。


参考文档：https://www.runoob.com/git/git-basic-operations.html
