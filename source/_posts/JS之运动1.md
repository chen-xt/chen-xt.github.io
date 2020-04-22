---
title: JS之运动1
date: 2016-07-19 
tags: 运动框架
categories: javascript
---
今天学习了JS的运动，从最简单的一个div运动到多个div运动开始，到后面通过div的运动实现常见的侧边栏的隐藏与显示，最后还实现图片或者div块的淡入淡出。最终总结起来其实都是一个运动函数startMove()的套用，涉及定时器的关与开。仔细观察下每个例子的startMove()运动函数，会发现都是大同小异的，所以将startMove()运动函数封装起来，当需要的时候导入，再修改下相应的参数即可，这样可以大大实现代码的简洁性。下面就是封装该函数的一个过程。

 首先，获取样式表指定对象的属性值，其中涉及浏览器兼容性问题，利用if-else语句来解决兼容性问题。

``` javascript
function getStyle(obj,attr)
    {
        if(obj.currentStyle)
        {
          return obj.currentStyle[attr]; //只支持IE浏览器
        }
        else
        {
            return getComputedStyle(obj,false)[attr]; //支持火狐，谷歌等浏览器，IE浏览器不支持
        }
    }
```

然后，就是设置startMove()函数里面包含的真正内容了，
1. 设置三个参数值，分别为obj（对象），attr（属性值），iTarget（目标值）
``` javascript
function startMove(obj, attr, iTarget){}
```
2. 设置定时器。开启一个定时器前应该先关闭定时器，因为如果没有先关闭定时器，那么当重复点击运动按钮时，会重复执行startMove()运动函数，即一起开启多个定时器，导致运动的重复和混乱。
``` javascript
clearInterval(obj.timer);
        obj.timer=setInterval(function()
        {  ... },30);      }
```
3. 设置定时器内部的内容。这里要注意一点，如果设置的是透明度的值，因为有可能会是小数点，所以需要用parseFloat；反之，如果设置的是非透明度值，则用parseInt。（iCur的初值为0，为所指定属性的值）
``` javascript
if(attr=='opacity')
            {
             //iCur=parseFloat(getStyle(obj,attr))*100;//结果是小数，会出现闪烁
             iCur=parseInt(parseFloat(getStyle(obj,attr))*100);//将小数部分去掉，解决闪烁
            }
            else
            {    //传进来的参数不是透明度，采用parseInt方法
                iCur=parseInt(getStyle(obj,attr));
            }
```
     
然后设置运动速度，这里设置速度值iSpeed为目标值与当前属性值的差再除以8，当速度值大于0时，将其向上取整，否则向下取整。
``` javascript
var iSpeed=(json[attr]-iCur)/8;
iSpeed=iSpeed>0?Math.ceil(iSpeed):Math.floor(iSpeed);
```

最后就是将指定属性的当前值与目标值做判断，等于目标值时关闭定时器，否则将属性的当前值加上速度值形成最新的值。
``` javascript
if(iCur==iTarget)
            {
                clearInterval(obj.timer);         
            }
            else
            {
                if(attr=='opacity')
                {
                    obj.style.filter='alpha(opacity:'+(iCur+iSpeed)+')';
                    obj.style.opacity=(iCur+iSpeed)/100; 
                }
                else
                {
                    obj.style[attr]=iCur+iSpeed+'px';
                }
            }
```

 到此一个运动函数搞定了，当需要导入到html文档时，只要添加如下命令，再修改下参数即可。
 ``` html
 <script src="move2.js"></script>
 ```

如下面例子中，改变oDiv1中的opacity属性，使其达到100。
``` 
 startMove(oDiv1,'opacity',100);
```

总结下，今天主要收获是学习并一定程度掌握了JS的运动框架。