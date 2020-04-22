---
title: Git的基本使用
date: 2016-09-28 15:33:52
tags: Git
categories: Git
---
## 前言
Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。在使用Git之前，你需要去[github官网](https://github.com/)去注册个账号。

## Git安装
Git的安装很简单，直接[下载](https://git-for-windows.github.io/)一个git安装包，然后安装的时候一直next到finish就OK了。
Git安装完成后需要做要一下设置，设置`username`和`email`：
``` 
$ git config --global user.name "your name"  
  //your name为github注册的名字
$ git  config --global user.email "your_email@youremail.com" 
  //your_email@youremail.com为github注册的邮箱
```

## 创建版本库
版本库又名仓库，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
1. 选择一个合适的地方，创建一个空目录`source`(版本库的名字可以随意取)，在我电脑上，我的仓库位于`E:\source`。
2. 通过`git init`命令把这个目录变成Git可以管理的仓库。执行完命令后可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，所以没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
```
$ git init
```
3. 添加文件到Git仓库，需要两步(这里以test.md文件为例)：
- 第一步，使用命令`git add <file>`，将文件test.md添加到仓库：
```
$ git add test.md
```
   这个命令可反复多次使用，添加多个文件。
- 第二步，使用命令`git commit -m “xxx”`，其中`-m`后面输入的是本次提交的说明，可以输入任意内容，不添加说明也是可以的。通过这个命令将文件提交到仓库：
```
 $ git commit -m "test1"
 [master 2140772] test1
 1 file changed, 1 insertion(+), 1 deletion(-)
```

这个时候，问题来了：为什么Git添加文件需要`add`、`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：
```
$ git add file1.txt
$ git add file2.txt 
$ git commit -m "add 3 files."  //2个文件都被提交 
         
```

## 管理文件
### 查看文件
`Cat`命令可以查看文件的内容
```
$ cat test.md
hello！
```
上面的命令告诉我们test.md文件的内容为`hello！`。
### 删除文件
`git rm`命令可以删除文件，它有2种删除方式。先新建一个文件，命名为delete.md，然后通过`git rm`命令的2种方式来删除该文件。
（1） `git rm <file>` 从暂存区和仓库中都删除某文件：
```
$ git rm delete.md
  rm 'delete.md'

$ git commit -m "del1"
 [master 25d0ea2] del1
 1 file changed, 1 deletion(-)
 delete mode 100644 delete.md
```
执行上面的命令后，会删除文件跟踪并且删除仓库中的文件delete.md，当你提交删除动作后，git不再管理该文件。去查看本地仓库，发现delete.md文件已经不存在了。

（2） `git rm --cached <file>`从暂存区删除某文件：
```
$ git rm --cached delete.md
   rm 'delete.md'
     
$ git commit -m "del2"
[master 5deede4] del2
 1 file changed, 1 deletion(-)
 delete mode 100644 delete.md
```
执行上面的命令后，删除文件跟踪但不删除仓库的文件delete.md，当你提交删除动作后，git不再管理该文件，但是文件系统中还是有delete.md文件。查看本地仓库，发现delete.md文件还是存在的。

## 查看仓库状态
`git status`命令可以让我们时刻掌握仓库当前的状态，修改test.md的内容，执行该命令如下：
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   test.md

no changes added to commit (use "git add" and/or "git commit -a")
```
上面的命令告诉我们，test.md文件被修改过了，但还没有准备提交修改。接下来我们可以使用`git diff`命令来查看具体修改的内容：
```
$ git diff
diff --git a/test.md b/test.md
index bc7774a..0000000
--- a/test.md
+++ b/test.md
@@ -1 +1 @@
-hello！
\ No newline at end of file
+hello world！
\ No newline at end of file
```
上面的命令告诉我们，test.md文件原本内容是`hello!`,现在被修改为`hello world！`。只要再使用`git add`和`git commit`两个命令即可完成修改文件的提交。

## 版本回退
刚才我们对test.md文件进行了1次修改,为了能够有个明显的对照，再次对test.md进行一次修改(将内容`hello world！`改为`world!`)。也就是说，现在仓库里保存着test.md的3个版本，可以通过`git log`命令查看从最近到最远的提交日志。
```
$ git log
commit f11d47a998c6b66bf6ee6b2ba0ad41b3bbf1af7a
Author: chen-xt <1250548605@qq.com>
Date:   Tue Sep 27 21:25:27 2016 +0800

    test3

commit 41b8298cff37c388eaba6a36822daf39e012968f
Author: chen-xt <1250548605@qq.com>
Date:   Tue Sep 27 21:23:30 2016 +0800

    test2

commit 21407720a1f2e4cf53a1c79f943b43b057eccd2f
Author: chen-xt <1250548605@qq.com>
Date:   Tue Sep 27 21:21:03 2016 +0800

test1
```
从提交日志中我们可以看到3次提交，最近的一次是test3，上一次是test2，最早的一次是test1，而跟在commit后面的一大串类似21407720a...的是`commit id`（版本号)。
### 回退过去
现在是在版本test3，而如果我们想退回上一个版本，也就是test2，那么可以怎么做呢？只要一句命令`$ git reset --hard HEAD^`即可：
```  
$ git reset --hard HEAD^
HEAD is now at 41b8298 test2
```
此时再查看日志，可以看到test3版本已经不存在了
```
$ git log
commit 41b8298cff37c388eaba6a36822daf39e012968f
Author: chen-xt <1250548605@qq.com>
Date:   Tue Sep 27 21:23:30 2016 +0800

    test2

commit 21407720a1f2e4cf53a1c79f943b43b057eccd2f
Author: chen-xt <1250548605@qq.com>
Date:   Tue Sep 27 21:21:03 2016 +0800

    test1
```
在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

### 重返未来
（1） 命令行窗口未关掉
如上现在处于版本test2，如果想回去刚最新的版本又该如何呢？办法还是有的，只要你上面的命令行窗口还没有被关掉，就可以找到test3版本的版本号`f11d47a998c6b66bf6ee6b2ba0ad41b3bbf1af7a`，那么使用如下命令即可回去最新的版本。
```
$ git reset --hard f11d47a998
HEAD is now at f11d47a test3
```
如上命令所示，现在处于版本test3，命令中版本号不需要写全，只需要前面几位即可，Git会自动去找。

（2） 命令行窗口已关掉
Git提供了一个命令`git reflog`用来记录你的每一次命令，所以如果命令行已经关掉的话，可以通过这个命令来找回要回去的版本号，然后再执行命令`$ git reset --hard commit-id`。
```
$ git reflog
f11d47a HEAD@{0}: reset: moving to f11d47a998
41b8298 HEAD@{1}: reset: moving to HEAD^
f11d47a HEAD@{2}: commit: test3
41b8298 HEAD@{3}: commit: test2
2140772 HEAD@{4}: commit: test1
```

## 远程仓库
### 在GitHub创建一个Git仓库
登录[GitHub](https://github.com/)，然后，在右上角找到`Create a new repo`按钮，创建一个新的仓库，命名为`source`。

### 配置ssh key
（1） 在本地创建ssh key
```
$ ssh-keygen -t rsa -C "your_email@youremail.com"
```
其中`your_email@youremail.com `改为你的邮箱，也是在github上注册的那个邮箱。

（2） 接下来就是回车回车，直到出现命令完成，提示可以进行下一条命令。

（3） 打开`.ssh`文件，打开`id_rsa.pub`，复制里面的key。里面的key是一对看不懂的字符数字组合，不用管它，直接复制。

（4） 回到github网站，进入`Account Settings`，左边选择`SSH Keys`，`Add SSH Key`，title随便填，在key里粘贴刚copy的那一段看不懂的字符。

（5） 验证是否成功，在`git bash`下输入`$ ssh -T git@github.com`。出现`Are you sure you want to continue connecting (yes/no)? `填`yes`。回车就会看到：`You’ve successfully authenticated, but GitHub does not provide shell access`。这就表示已成功连上github。刷新github网站，会发现`ssh key` 的钥匙标志由灰色变成绿色。

### 把本地仓库的内容推送到GitHub仓库
使用`$ git remote add origin git@github.com:yourName/yourRepo.git`命令关联一个远程库。其中`yourName`和`yourRepo`表示你在github的用户名和刚才新建的仓库，推送完之后进入.git文件夹，打开config文件，这里会多出一个`remote “origin”`内容，这就是刚才添加的远程地址，也可以直接修改config文件来配置远程地址。

### 把本地仓库的所有内容推送到远程库上
推送的前提是文件已经被添加到本地仓库，也就是说已经`git add`和`git commit`操作了。
使用`git push origin master`命令将文件推送到远程库上。推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样。
*注意*：由于远程库是空的，我们第一次推送master分支时，加上了`-u`参数，也就是使用`git push -u origin master`Git不但会把本地的`master`分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送时就可以省去`-u`参数。
