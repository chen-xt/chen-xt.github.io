---
title: JS之绑定事件
date: 2016-08-01 21:45:30
tags: 事件
categories: javascript
---
当我们给一个元素绑定一个方法后，再绑定一个方法时，第一个方法往往会被覆盖掉。如下例子，结果只会执行第二个方法，也就是说只弹出“b”。
``` javascript
window.onload=function(){
    alert('a');
}
window.onload=function(){
    alert('b');
}
```
因此为了保证两个及以上的方法能够正常执行，可以对元素进行事件绑定。
绑定事件有两个函数，attachEvent()和addEventListener()。
1. attachEvent(事件名称, 函数)，IE浏览器兼容，这个函数的执行顺序是相反的，即先绑定的方法后面执行。
``` javascript
oBtn.attachEvent('onclick',bb);
oBtn.attachEvent('onclick',aa);//先执行方法aa,再执行方法bb
```
2. addEventListener(事件名称,函数, 捕获)，ff和chrome浏览器兼容，这个函数的执行顺序是按正常的顺序执行的，即先绑定的方法先执行。
``` javascript
oBtn.addEventListener('click',aa,false);
oBtn.addEventListener('click',bb,false);//先执行方法aa,再执行方法bb
```
为了解决浏览器兼容性问题，使用if-else语句来处理兼容性，使其兼容所有的浏览器。这里要注意一点，attachEvent()的事件名称要比addEventListener()的事件名称前面多了个‘on’。
``` javascript
    if(oBtn.attachEvent)//IE
    {
        oBtn.attachEvent('onclick',bb);
        oBtn.attachEvent('onclick',aa);
    }
   else//ff、chrome
    {
        oBtn.addEventListener('click',aa,false);
        oBtn.addEventListener('click',bb,false);
    }
```
到此一个事件绑定就完成了，但是为了使代码更加简洁明了，且方便调用，可以将此解决兼容性的if-else语句封装成一个函数。
``` javascript
function myAddEvent(obj,sEvent,fn)
        {
            if(obj.attachEvent)
            {
                obj.attachEvent('on'+sEvent,fn);
            }
            else
            {
                obj.addEventListener(sEvent,fn,false);
            }
        }
```
之后就可以根据需要调用该封装函数了。
``` javascript
      myAddEvent(oBtn,'click',aa)
      myAddEvent(oBtn,'click',bb);
```
既然可以对元素进行事件绑定，当然也可以解除绑定。解除绑定同样有两个函数detachEvent(事件名称, 函数)和removeEventListener(事件名称, 函数, 捕获)，分别对应attachEvent()和addEventListener()的解绑。由于二者的用法同attachEvent()和addEventListener()的用法类似，所以这里就不再进行详细的说明了。