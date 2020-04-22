---
title: PHP实训总结
date: 2016-12-08 16:49:11
tags: 实训总结
categories: PHP
---
## 前言
&emsp;&emsp;结课考试后，学校安排了一次为期10天的实训，实训方向有安卓和PHP，我选择了PHP，因为PHP跟前端还是有关系的。实训虽然讲的内容不多，但还是从中得到一些收获，并最终做出一个包含增删改查功能的简历投递系统，特此写个总结。

## PHP环境配置
&emsp;&emsp;主要用到三个软件：PhpStorm----开发工具，phpStudy----开发环境，Navicat for MySQL----MySQL数据库。
### phpStudy的安装
1. 点击安装包，一直next直至安装完成。
2. 打开phpStudy—>其他选项菜单—>打开配置文件—>找到vhosts.conf文件—>粘贴配置代码—>保存—>重启phpStudy。配置代码如下(其中路径为phpstudy安装目录下www文件所在的路径，如`D:\phpstudy\WWW`，域名可以自己取，如`www.cxt.com`)：
```
    <VirtualHost *:80>
        DocumentRoot "路径"
        ServerName 域名
      <Directory "路径">
          Options +Indexes +FollowSymLinks +ExecCGI
          AllowOverride All
          Order allow,deny
          Allow from all
          Require all granted
      </Directory>
    </VirtualHost>
```
3. 找到路径 `C:\Windows\System32\drivers\etc\hosts`，对hosts文件进行配置：在最后一行加上`127.0.0.1 域名`。
4. 打开phpStudy—>其他选项菜单—>PHP扩展及配置—>PHP扩展—>将php_openssl打钩。
5. 通过域名或者路径“127.0.1.1”或者路径“localhost”打开浏览器，会有欢迎方面，表示安装成功。
6. 如果想创建多个域名，只要参照(1)(2)步下来即可。

### PhpStorm的安装
1. 点击安装包，一直next直至安装完成。
2. 打开PhpStorm—>输入以下License进行破解：
```
     User name:
        EMBRACE

     License key:
        ===== LICENSE BEGIN =====
        43136-12042010随机加上一串数字，防止验证码冲突(数字随便写几个)
        00002UsvSON704l"dILe1PVx3y4"B3
        49AU6oSDJrsjE8nMOQh"8HTDJHIUUh
        gd1BebYc5U"6OxDbVsALB4Eb10PW8"
        ===== LICENSE END =====
```
3. 将软件关闭进行汉化：找到phpstorm安装位置，找到里面的`\lib\messages`,把汉化包`resources_cn.jar`放入进去，重启phpstrom，即可发现汉化成功。

### Navicat for MySQL的安装
1. 点击安装包，一直next直至安装完成。
2. 打开数据库—>连接—>设置连接名和密码都为`root`—>连接测试—>确定测试成功。
3. 根据需要创建数据库和表。

## MySQL常用语句
### 插入一条数据
格式：insert  into  表名（字段1，字段2，字段3……）values(值1，值2，值3……)
如：`insert into type(name) values ('生物')`
### 查看数据
格式：select 字段1，字段2，……from  表名
&emsp;&emsp;字段之间用`,`隔开，如果查询所有的字段用`*`来代替。
如：`select * from books`
### 修改数据
格式：update  表名  set 字段名1=‘新值1’，字段名2=‘新值2’ where='条件'
&emsp;&emsp;如果不加where条件那么将会把所有的记录的值都修改掉。
如：update type set name='生物' where name='地理'
### 删除数据
格式：delete  from  表名 where 条件
&emsp;&emsp;如果不加where条件那么将会把所有的记录都删除掉。
如：delete from type where name='生物'

## PHP知识点
&emsp;&emsp;这里只列举一些本次实训所用到的PHP知识点。
1. PHP中所有的变量必须用`$`来修饰
2. php定界符是`<?php ?>`，也就是说PHP代码中以`<?php`开始，以`?>`结束。
3. 隐藏域：不会显示在页面上，但是会被提交过去。
```
    <input type=’hidden’ name=’hide’value=“eeee”>  //php接收:$_POST[‘hide’]
```
4. `$_POST[]`接收所有以`post`方式传递的值，`$_GET[]`接收所有以`get`方式传递的值。
5. mysql_query() 函数：执行一条 MySQL 查询。
```
    $data=mysql_query($sql);
```
6. mysql_fetch_assoc() 函数：从结果集中取得一行作为关联数组,其实就是将取得的结果以数组形式返回。
```
    $data=mysql_fetch_assoc($data);
```
7. include文件：将 PHP 文件的内容插入到另一个 PHP 文件中。
```
    include("mysql.php");
```
8. die(status) 函数：输出一条消息，并退出当前脚本。如果 status 是字符串，则该函数会在退出前输出字符串；如果 status 是整数，这个值会被用作退出状态。
```
$link=mysql_connect("127.0.0.1","root","root") or die("数据库连接失败");//连接数据库
```
9. isset()函数： 一般用来检测变量是否设置。若变量已存在则返回 true 值。其它情形返回 false 值。
```
 $tel=isset($_POST["tel"])?$_POST["tel"]:"";//三元运算符，如果变量值未设置就返回空值
```
10. explode()：将字符串分割成为数组.有两个参数，第一个是分割符号，第二个是字符串名，返回值为一个分割后的新数组。
```
 $arr=explode(".",$_FILES["myfile"]["name"]);
```

## 总结
&emsp;&emsp;通过完成一个简单的小项目--[简历投递系统](https://github.com/chen-xt/resume)，我发现用PHP做增删改查功能的原理大致是：处理页面去获取用户提交的值或者路径的参数值，然后执行sql语句，再转入指定的结果显示的页面。而且经常需要手动去添加路径的参数值，以便能被获取到。
&emsp;&emsp;明白了原理之后再去做项目其实并不会特别难，但是常常因为变量未获取，sql语句语法出现错误等等各种原因导致错误，所以在敲代码的时候认真细心还是非常重要的，这点还需要后面慢慢去培养。
