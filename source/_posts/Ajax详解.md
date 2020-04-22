---
title: Ajax详解
date: 2016-08-21 20:47:28
tags: Ajax
categories: javascript
---
## 什么是Ajax?
**Ajax** (Asynchronous JavaScript and XML)，是一种使网页实现异步更新的技术。Ajax的核心对象是**XMLHttpRequest对象**，XMLHttpRequest通过在后台与服务器进行少量数据交换，可以实现在不重新加载整个网页的情况下，对网页的某部分进行更新。
传统的网页（不用Ajax技术的网页），想要更新内容或者提交一个表单，就要重新载入页面。而如果让网页使用Ajax技术，通过后台跟服务器进行少量的数据交换，网页就可以实现异步局部更新。我们身边有很多Ajax的应用案例，如新浪微博、开心网等等。

## Ajax的属性
**1. responseText属性：**作为响应主体被返回的文本。

**2. responseXML属性：**如果响应的内容类型是“text/xml”或“applocation/xml”，这个属性就将保存相应数据的XML DOM文档。

**3. status属性：**响应的HTTP状态。只有当HTTP状态代码为200，才表示响应成功。此时可以访问responseText或者responseXML的内容。

**4. statusText属性：**HTTP状态的说明。

**5. readyState属性：**表示请求/响应过程的当前活动阶段。
- 0：未初始化。还没有调用open()方法。
- 1：启动。已调用open()方法，还没有调用send()方法
- 2：发送。已调用send()方法，还没有接收到响应。
- 3：接收。已接收到部分响应数据。
- 4：（完成）已接收到全部响应数据，可以在客户端调用了。(此时仅代表完成（结束），是否成功还需要看status属性)

## Ajax的使用
#### 创建XMLHttpRequest对象
创建XMLHttpRequest对象时需要考虑一个浏览器兼容性的问题，因为IE6及以下版本的浏览器不支持XMLHttpRequest对象，所以需要先检查浏览器是否支持 XMLHttpRequest 对象。如果支持，则创建 XMLHttpRequest 对象。如果不支持，则创建 ActiveXObject对象(IE6及以下版本的浏览器支持)。
``` javascript
          var oAjax=null;
            if(window.XMLHttpRequest)         
            {
                oAjax=new XMLHttpRequest();
            }
            else
            {
                oAjax=new ActiveXObject("Microsoft.XMLHTTP");
            }
```
#### 连接服务器
XMLHttpRequest对象连接服务器使用的是open()方法，它接受3个参数：要发送的请求的类型（“GET”或者“POST”）、请求的URL和表示是否异步发送请求的布尔值（“false”表示同步，“true”表示异步）。
``` javascript
    oAjax.open('GET',”login.php”,true); 
```
这行代码会启动一个针对login.php的GET请求，且支持异步发送请求。使用open()方法并不会真正发送请求，而只是启动一个请求以备发送。
#### 发送请求
要真正发送特定的请求，需要在添加open()方法的基础上再添加send()方法。send()方法接受1个可选的参数，即要作为请求主体发送的数据。当发送的请求为POST请求时，必须使用 setRequestHeader() 来添加 HTTP 头，然后在 send() 方法中设置发送的数据。特别要注意的是，setRequestHeader() 方法必须在调用open(0方法之后且调用send()方法之前调用才可以成功发送头部信息。下面为一个POST请求的例子。
``` javascript
    oAjax.open("POST","login.php",true);
    oAjax.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    oAjax.send("fname=Bill&lname=Gates");
```
#### 接收返回
当请求被发送到服务器时，我们需要执行一些基于响应的任务。每当 readyState改变时，就会触发 onreadystatechange 事件。readyState 属性存有 XMLHttpRequest 的状态信息。在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。
只有当 readyState 等于 4 且状态为 200 时，才表示响应已就绪，而我们通常也只关注这2种状态。
``` javascript
   oAjax.onreadystatechange=function()
      {
        if(oAjax.readyState==4)//4表示（完成）响应内容解析完成，可以在客户端调用了
                {
                    if(oAjax.status==200)//结果为200，代表成功
                    {
                        //成功，服务器返回的内容
                    }
                    else
                    {
                        //失败返回的内容                       
                        
                    }
                }
        }
```


## Ajax应用举例
这里通过一个读取文件信息的Ajax小例子来加深对Ajax的理解。这个例子通过Ajax技术去读取文件名为“abc.txt”的文档里面的内容，并返回。
``` javascript
            //1.创建ajax对象
            var oAjax=null;
            if(window.XMLHttpRequest)
            {
                oAjax=new XMLHttpRequest();
            }
            else
            {
                oAjax=new ActiveXObject("Microsoft.XMLHTTP");
            }

            //2.连接服务器  open(方法, url, 是否异步)
            oAjax.open('GET',"abc.txt",true);  //异步

            //3.发送请求
            oAjax.send();

            //4.接收返回
            oAjax.onreadystatechange=function()
            {
                if(oAjax.readyState==4)//4表示（完成）响应内容解析完成，可以在客户端调用了
                {   
                    if(oAjax.status==200)
                    {
                        alert('成功：'+oAjax.responseText);//服务器返回的内容  
                    }
                    else
                    {
                        alert('失败');
                    }
                }
            }
```

