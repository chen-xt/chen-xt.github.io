---
title: Javascript的异常处理和事件处理
date: 2016-07-29 23:05:19
tags: 异常处理
categories: javascript
---
#### Javascript的异常处理
Javascript的异常就是当javascript引擎执行javascript代码时，发生了错误，导致程序停止运行的一种现象。当异常产生时，会创建一个异常类对象，该异常类对象将被提交给浏览器，这个过程称为“抛出异常”。当浏览器接收到异常对象时，会寻找能处理这一异常的代码并把当前异常对象提交给其处理，这一过程被称为“捕获异常”。
Javascript使用try..catch..finally语句来进行异常的捕获，try..catch..finally语句的基本语法格式为： 
``` javascript
try{
    //可能抛出异常的代码 
    }catch(error){
     //如果发生异常会执行的代码，error为发生的异常类对象 
    }finally{
    //无条件执行的代码 
    } 
```
try块中的语句首先被执行。如果运行中发生了错误，控制就会转移到位于catch块中语句，其中括号中的error参数被作为例外变量传递。否则，catch块的语句被跳过不执行。无论是发生错误时catch块中的语句执行完毕，或者没有发生错误try块中的语句执行完毕，最后将执行 finally块中的语句。在try..catch..finally语句中，try是必需的，catch和finally均为可选语句，但catch和finally中必须至少出现一个。
下面例子为捕获到的是变量未定义的异常：
``` javascript
function demo()
        {   var str="你好"; //没有声明str的话会有异常
            try{
                alert(str);
            }catch(er){
                alert(er);
            }
        }
        demo();
```
Javascript抛出异常使用的是throw语句，可创建一个自定义异常。将throw语句于try..catch..finally语句结合使用，能够控制程序流，并生成自定义的错误消息。
``` javascript
function demo()
        {   
            try{
                var e=document.getElementById('txt').value;
                if(e=='')
                {
                    throw "第一个用户输入异常==空";
                }
            }catch(er){
                alert(er);
            }
        }
        demo();
```

#### Javascript的事件处理
事件指的是文档或者浏览器窗口中发生的一些特定交互瞬间。与浏览器进行交互的时候浏览器就会触发各种事件。比如当我们打开某一个网页的时候，浏览器加载完成了这个网页，就会触发一个load事件；当我们点击页面中的某一个“地方”，浏览器就会在那个“地方”触发一个 click 事件。这里列举了Javascript几个常用的事件：    
 - onClick：鼠标点击触发的事件，多用在某个对象控制的范围内的鼠标点击
 - onMouseOver：鼠标移入元素时触发的事件。
 - onMouseOut：鼠标移开元素时触发的事件。
 - onChnange：当文本内容改变时触发的事件。
 - onSelect：当文本框被选中时触发的事件。
 - onFocus：光标聚集事件，即在对象获得焦点时触发的事件。
 - onBlur：移开光标事件，即在对象失去焦点时触发的事件。
 - onLoad：网页加载事件，即在网页加载完成触发的事件。
 - onUnload：关闭网页事件，即退出网页时触发的事件