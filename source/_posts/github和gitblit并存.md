---
title: github 和 gitblit 并存
date: 2019-09-17 15:45:33
tags: Git
categories: Git
---

## 前言

&emsp;&emsp;由于工作需要，一台电脑需要同时存在 github 和 gitblit 账号，因为需要配置多个 ssh key，以满足个人和工作上的需要。

## 步骤

### 分别创建 github 和 gitblit 账号的公钥

1. 创建 github 账号公钥
   输入命令`ssh-keygen -t rsa -C "GitEmail@example.com"`，然后 enter 键，如下图
   ![图1](/images/github1.jpg)这里先不要回车，输入`/Users/cxiaoting/.ssh/id_rsa_github`（id_rsa_github 为自定义的名字），然后 enter 键，会提示输入 2 次密码，根据实际情况输入密码即可。
2. 创建 gitblit 账号公钥
   输入命令`ssh-keygen -t rsa -C "GitEmail@example.com"`，然后 enter 键，如下图
   ![图2](/images/github1.jpg)这里先不要回车，输入`/Users/cxiaoting/.ssh/id_rsa_gitblit`（id_rsa_gitblit 为自定义的名字），然后 enter 键，会提示输入 2 次密码，根据实际情况输入密码即可。

### 创建并配置 config 文件

1. 打开.ssh 目录
   输入命令`open ~/.ssh`，进入.ssh 目录。
2. 创建 config 文件
   输入命令`touch config`，将以下内容复制进去到 config 文件中，其中 username 为账户名。

```
Host github
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
User username1

Host gitblit
HostName gitblit.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitblit
User username2

```

### 将公钥添加到系统

1. 把 2 个账号公钥添加到系统
   输入命令`ssh-add id_rsa_gitblit id_rsa_github`，然后会提示你分别输入两个账号的密码，按照提示输入。
2. 查看 2 个公钥是否成功添加到系统
   输入命令`ssh-add -l`，添加成功则会显示 2 条记录。

### 将公钥添加到线上的 sshkey

&emsp;&emsp;这里举例 github 账号,新建一个 ssh key，然后将 id_rsa_github.pub 的内容（即公钥）复制一份放到对应的 key 里面，保存。gitblit 账号也是同样的做法。
![图3](/images/github2.jpg)

### 测试连接是否成功

&emsp;&emsp;测试 github 账号，输入命令`ssh -T git@github`；测试 gitblit 账号，输入命令`ssh -T git@gitblit`,出现如下图即表示成功。
![图4](/images/github3.jpg)

## 总结

&emsp;&emsp;成功完成以上步骤，就可以无需账号密码，尽情操作 github 和 gitblit 了。
