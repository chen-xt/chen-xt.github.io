---
title: JS之运动2
date: 2016-07-20
tags: 图片轮播
categories: javascript
---
今天继续了JS运动的学习，本来以为昨天总结的运动函数startMove()是最完整的了，才发现昨天那个还是存在欠缺部分，只能实现单个div单个运动。今天将这个运动函数进行改进，实现了多个div运动以及同一个div的多值运动。改进版运动函数跟之前的差别不大，首先是参数的改变，由原来的2个变成3个，参数类型也变了,"function startMove(obj,json,fn)"。其次是多了个布尔变量bStop，该变量用来判断运动是否结束；最后是添加一个if语句，判断是否传入fn参数，以此来决定执行不执行fn,"if(fn){fn();}"。

然后学到的就是图片轮换播放，这是今天最大的收获了。下面就是实现图片轮换播放的过程。
1. 用class名称选取元素，需要注意，js中的class是关键字，所以必须写成className。
``` javascript
function getByClass(oParent, sClass)
{
    var aEle=document.getElementsByTagName('*'); //选出父节点下的所有元素
    var i=0;
    var aResult=[];   //定义一个空的数组，用来装所有的选出元素
    for(i=0;i<aEle.length;i++)
    {
        if(aEle[i].className==sClass)
        {
            aResult.push(aEle[i]);  //将选出的所有元素装入数组中
        }
    }
    return aResult;  //  循环完成后，返回数组里的值
}
```
2. 点击实现上下张图片的左右按钮，通过改变其透明度的大小，实现鼠标移入移出渐隐渐现功能。
``` javascript
oBtnPrev.onmouseover=oMarkLeft.onmouseover=function ()
    {
        startMove(oBtnPrev, 'opacity', 100);
    }
    oBtnPrev.onmouseout=oMarkLeft.onmouseout=function ()
    {
        startMove(oBtnPrev, 'opacity', 0);
    }   
    oBtnNext.onmouseover=oMarkRight.onmouseover=function ()
    {
        startMove(oBtnNext, 'opacity', 100);
    }   
    oBtnNext.onmouseout=oMarkRight.onmouseout=function ()
    {
        startMove(oBtnNext, 'opacity', 0);
    }
```
3. 实现小图移入渐隐渐现效果，配合点击时的变化功能，当重复点击同一张图片时，不做任何处理。
``` javascript
       aSmallLi[i].onmouseover=function ()
        {
            startMove(this, 'opacity', 100);
        }
        aSmallLi[i].onmouseout=function ()
        {
            if(this.index!=iNow)//在非当前图片移入移出
            {
                startMove(this, 'opacity', 60);
            }
        }   
        aSmallLi[i].onclick=function ()
        {
            if(this.index==iNow)return;  //重复点击同一张图片，不做处理
            iNow=this.index; //更新   
            tab();
        }
```
4. 当点击小图时，对应大图进行切换。这里要注意的是需要给每张图片加层级，点击即将要显示的图片必须让它的Zindex值比当前显示的图片大，这样才能覆盖当前图片显示。还有利用运动函数让大图的高度由0开始增大，实现大图下拉显示的效果。
``` javascript
for(i=0;i<aSmallLi.length;i++)
            {
                startMove(aSmallLi[i], 'opacity', 60);
            }
            startMove(aSmallLi[iNow], 'opacity', 100);
            aBigLi[iNow].style.zIndex=iMinZindex++; //加层级，否则只会显示第一张图片，其他都会被挡住
            aBigLi[iNow].style.height=0; //使图片从上往下显示
            startMove(aBigLi[iNow], 'height', oBigUl.offsetHeight);
```
5. 随着点击下一张图片时，发生居中切换的效果。首尾图片的位置需要做特殊处理，避免出现前后的空白。
``` javascript
    if(iNow==0) 
            { //第0张，移动到0，否则会前面有空白
                startMove(oSmallUl, 'left', 0);
            }
            else if(iNow==1) //第二张
            {
                startMove(oSmallUl, 'left', 1);
            }
            else if(iNow==aSmallLi.length-1) //最后一张
            {
                startMove(oSmallUl, 'left', -(iNow-3)*aSmallLi[0].offsetWidth);
            }
            else
            {
                startMove(oSmallUl, 'left', -(iNow-2)*aSmallLi[0].offsetWidth);
            }
```
6. 给左右按钮添加点击动作，点击第一张图片时，跳到最后一张，点击最后一张时，跳到第一张。
``` javascript
      oBtnPrev.onclick=function ()
        {
            iNow--;
            if(iNow==-1)  //到0张，直接到最后一张
            {
                iNow=aSmallLi.length-1;
            }       
            tab();
        }
        oBtnNext.onclick=function ()
        {
            iNow++;
            if(iNow==aSmallLi.length) //到最后一张，直接到第0张
            {
                iNow=0;
            }           
            tab();
        }
```
7. 执行完以上六个步骤，得到如下效果图:
![效果图1](/images/1.png)
以上代码为参考课程所给案例进行编写测试的，对其进行修改，将底下小图片显示的个数增加到四张，首先是通过样式改变小图的宽度，此时第一张及最后一张都会出现空白，所以还需要对图片的位置进行上述步骤五的处理，如下，这样一个图片轮播的效果就出来了。
![效果图2](/images/2.png)
运动函数以及图片轮播经常在一些网站上出现，所以个人感觉学会它们是很有必要的，目前学到也只是初步的一些东西，后面还是需要进行一些深入学习与练习才能真正掌握它们。