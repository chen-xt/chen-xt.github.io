---
title: JS之事件
date: 2016-08-07 10:30:18
tags: 事件
categories: javascript
---
## 事件
什么是事件呢？用户在某些内容上的点击、鼠标经过某个特定元素或按下键盘上的某些按键等等这些都算是事件。所以说白点，事件就是文档或浏览器窗口中发生的一些特定的交互瞬间。
## 事件流
事件流是从页面中接收事件的顺序。主要有IE提出的事件冒泡流和Netscape开发团队提出的事件捕获流。
### 事件冒泡
事件冒泡，即事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点(文档)。
``` html
<!DOCTYPE html>
<html>
<head>
<title>事件冒泡</title>
<body>
<div id=”div1”>传播顺序为：<div>,<body>,<html>,document </div>
</body>
</html>
```
如上的例子，click事件首先在div元素(我们单击的元素)上发生，然后，click事件沿DOM树向上传播，在每一级节点都会发生，直至传播到document对象。
### 事件捕获
事件捕获，即不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。
``` html
<!DOCTYPE html>
<html>
<head>
<title>事件冒泡</title>
<body>
<div id=”div1”>传播顺序为：document,<html>,<body>,<div> </div>
</body>
</html>
```
如上的例子，document对象首先接收到clcik事件，然后事件沿DOM树依次向下，一直传播到事件的实际目标，即div元素。
### DOM事件流
DOM事件流包括三阶段：事件捕获阶段，处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会；然后是实际的目标接收到事件；最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
## 事件处理程序
产生了事件，我们就要去处理他，因此需要事件处理程序，即响应某个事件的函数。主要有以下几种方式：
### HTML事件处理程序
HTML事件处理程序：直接在HTML代码中添加事件处理程序。
``` html
<script type="text/javascript">
        function showMessage(){
            alert("Hello world!");
        }
 </script>
<input type="button" value="Click Me" onclick="showMessage()" />
```
从上面的代码中，我们可以看出，事件处理是直接嵌套在元素里头的，这样有一个缺点就是html和js代码紧密耦合，如果要更换事件处理程序，不但要修改js，还需要修改html。一两处修改尚可接受，当你的代码达到万行级别时，修改起来就将会是件工程量浩大的事情了。因此并不推荐使用。
### DOM0级事件处理程序
DOM0级事件处理程序：为指定对象添加事件处理。
``` javascript
var btn = document.getElementById("myBtn");
 btn.onclick = function(){
            alert(this.id);
        };
```
从上面的代码中，我们能看出，相对于HTML事件处理程序，DOM0级事件处理程序中html代码和js代码的耦合性已经大大降低。
### DOM2级事件处理程序
DOM2级事件处理程序：也是对特定的对象添加事件处理程序。定义了两个方法addEventListener()和removeEventListener()，用于处理指定和删除事件处理程序的操作。它们都接收三个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值（是否在捕获阶段处理事件）。
1. addEventListener()方法：添加事件处理程序。使用该方法的一个好处就是可以添加多个事件处理程序。
``` javascript
var btn = document.getElementById("myBtn");
  btn.addEventListener("click", function(){
            alert(this.id);
        }, false); //false，表示在冒泡阶段调用事件处理程序
 btn.addEventListener("click", function(){
            alert("Hello world!");
        }, false);
```
上面的代码为一个按钮添加了两个事件处理程序，这两个事件处理程序会按照添加它们的**顺序触发**，因此首先会显示元素的ID，其次会显示“Hello world!”消息。
2. removeEventListener()方法：删除通过addEventListener()方法添加的事件处理程序，删除时传入的参数必须与添加处理程序时使用的参数相同，也就是说，无法删除匿名函数。我们来看一个例子：
``` javascript
var btn = document.getElementById("myBtn");
  btn.addEventListener("click", function(){
            alert(this.id);
        }, false); 
btn.removeEventListener("click", function(){
            alert(this.id);
        }, false);  //无法删除
```
上面的代码，我们使用addEventListener()添加了一个事件处理程序，虽然调用removeEventListener()时看似使用了相同的参数，但实际上，第二个参数与传入addEventListener()中那个是完全不同的函数，所以没办法删除。那么应该如何进行改进，才能使removeEventListener()发挥其真正的作用呢？
``` javascript
var btn = document.getElementById("myBtn");
        var handler = function(){
            alert(this.id);
        };
        btn.addEventListener("click", handler, false); 
        
        var removeBtn = document.getElementById("myRemoveBtn");
        removeBtn.onclick = function(){
            btn.removeEventListener("click", handler, false);  //成功删除        
        };
```
上述重写后的例子，在addEventListener()和removeEventListener()中使用了相同的函数，因此，成功地删除所添加的事件处理程序。
### IE事件处理程序
IE实现了与DOM类似的两个方法：attachEvent()和detachEvent()方法，它们都接受两个参数，事件处理程序名称与事件处理程序函数。
1. attachEvent()方法：添加事件处理程序。
``` javascript
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
            alert("Clicked");
        });
```
上述例子，使用attachEvent()方法添加了一个事件处理程序，需要注意的一点是，attachEvent()第一个参数是“onclick”，而addEventListener()方法中是“click”。同样的，attachEvent()方法也可以用来添加多个事件处理程序，只是触发顺序是**逆序**的，与addEventListener()多事件处理程序的触发顺序相反。
2. detachEvent()方法：删除通过attachEvent()方法添加的事件处理程序，条件是必须提供相同的参数，同样的，detachEvent()方法无法删除匿名函数。
``` javascript
var btn = document.getElementById("myBtn");
 var handler = function(){
            alert("Clicked");
        };
  btn.attachEvent("onclick", handler);     
 var removeBtn = document.getElementById("myRemoveBtn");
  removeBtn.onclick = function(){
    btn.detachEvent("onclick", handler); 
        };
```
上述例子，首先利用attachEvent()方法成功添加一个事件处理程序，然后利用detachEvent()删除所添加的事件处理程序。
### 跨浏览器的事件处理程序
综合前面所提到的几种事件处理程序方法，我们封装一个EventUtil对象，实现跨浏览器处理事件。如下：
``` javascript
var EventUtil = {
    addHandler: function(element, type, handler){  //添加事件
        if (element.addEventListener){
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent){
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    }
 removeHandler: function(element, type, handler){  //删除事件
        if (element.removeEventListener){
            element.removeEventListener(type, handler, false);
        } else if (element.detachEvent){
            element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
        }
    }}
```
上述两个方法首先都会检测传入的元素中是否存在DOM2级方法。如果存在DOM2级方法，则使用该方法，传入三个参数；如果存在的是IE的方法，则采取第二种方案。如果前面两者都不存在的话，那就是采取最后一张方案，使用DOM0级方法。
对EventUtil对象进行使用：
``` javascript
EventUtil.addHandler(btn, "click", handler); 
EventUtil.removeHandler(btn, "click", handler); 
```
