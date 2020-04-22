---
title: 简单的PHP+MySql实现动态页面
date: 2016-07-27 20:20:51
tags: PHP
categories: PHP
---
虽说自己学的是前端，但是关于前后端的一些交互还是有必要做简单的了解的。这两天学会了简单的PHP加上MySql数据库来实现动态页面，在WAMP的集成环境下设计了一个简单的注册页面，并将注册信息通过后端传送到数据库。
1. 首先创建好数据库，在WAMP的环境下，建立数据库“register”，并创建具有两个字段的“message”表，存放用户名(username)及密码(password)。当有用户注册填写相关信息时，所填信息会被保存到该数据库中。

2. 首先创建"register.php"文件，在里面编写相关的前端代码。创建两个文本框分别为用户的姓名输入框和密码输入框，再加一个注册按钮，为注册按钮添加调用ajax的相关语句。
``` javascript
oBtn.onclick=function()
 {
var url='register_post.php?act=add&username='+oTxt1.value+'&password='+oTxt2.value;
ajax(url, function (str){          
        });
 }
```

3. 创建"register_post.php"文件，在里面编写与数据库连接的代码以及数据库的执行操作语句。
``` 
<?php
switch($_GET['act'])
{
 case 'add':
  $username=$_GET['username'];//接收前端上传的数据
  $password=$_GET['password'];
  $sql="INSERT INTO message (username, password) VALUES('{$username}', '{$password}')";
  mysql_connect('localhost', 'root', '');
  mysql_select_db('register');
  mysql_query($sql);
  break;
}
?>
```

4. 执行"register.php"文件，输入用户名和密码，点击注册按钮，即可发现数据库多了所填写的用户名和密码字段，到此，一个小的动态注册页面完成。这里只是通过简单的做个小例子来让自己明白前后端的一些交互方式，并不做深入的学习。
