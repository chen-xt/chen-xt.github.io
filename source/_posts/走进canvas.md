---
title: 走进canvas
date: 2017-04-04 18:36:51
tags: canvas
categories: html5
---
## canvas是什么？
&emsp;&emsp;`<canvas>`是一个可以使用脚本在其中绘制图形的HTML 元素。什么意思呢？就是canvas元素本身并不绘制图形，它只是相当于一张空画布。如果想在canvas上绘制图形，则必须使用 JavaScript脚本来进行绘制。
## canvas的基本用法
### 定义 canvas 元素
```
	<canvas id="canvas" width="1920" height="1200"></canvas>
```
&emsp;&emsp;&emsp;&emsp;`<canvas>`标签只有两个属性—— `width`和`height`，这两个属性都是可选的，当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。要注意一点，因为`<canvas>`是**双标签**，所以结束标签`</canvas>`一定**不可以省略**。
### 获取canvas 元素对应的 DOM 对象
```
	//得到DOM对象
	var canvas = document.getElementById('canvas');
```
### 调用 canvas 对象的 getContext()方法
&emsp;&emsp;&emsp;&emsp;这个方法是用来获得渲染上下文和绘画功能。该方法会返回一个 `canvasRenderingContext2D`对象，该对象可以绘制图形。
```
	var ctx = canvas.getContext('2d');
```
### 调用canvas的方法进行绘图
&emsp;&emsp;&emsp;&emsp;这些后面会提到。

## 绘制形状
### 绘制矩形
- **rect(x, y, width, height)**

&emsp;&emsp;&emsp;&emsp;绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。没有填充的话，不会显示。
- **fillRect(x, y, width, height)**

&emsp;&emsp;&emsp;&emsp;绘制一个左上角坐标为（x,y），宽高为width以及height并填充的矩形，默认填充颜色为黑色。
- **strokeRect(x, y, width, height)**

&emsp;&emsp;&emsp;&emsp;绘制一个左上角坐标为（x,y），宽高为width以及height的矩形边框，默认边框颜色为黑色。
- **clearRect(x, y, width, height)**
&emsp;&emsp;清除指定矩形区域，让清除部分完全透明。
```
	//边长为100px的黑色正方形
	ctx.fillRect(25,25,100,100); 

	//从正方形的中心开始擦除了一个60*60px的正方形
	ctx.clearRect(45,45,60,60);

	//在清除区域内生成一个50*50的正方形边框  
	ctx.strokeRect(50,50,50,50);  
```

### 绘制路径
&emsp;&emsp;图形的基本元素是路径。一个路径，甚至一个子路径，都是闭合的。以下为常用的几个方法，很多图形其实是通过绘制路径而绘制出来的。
- **beginPath()**
&emsp;&emsp;新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
- **closePath()**
&emsp;&emsp;闭合路径之后图形绘制命令又重新指向到上下文中。闭合路径closePath(),不是必需的，比如路径使用填充（filled）时，路径会自动闭合，可以不用closePath()；但是如果使用描边（stroked）则不会闭合路径。
- **stroke()**
&emsp;&emsp;通过线条来绘制图形轮廓。
- **fill()**
&emsp;&emsp;通过填充路径的内容区域生成实心的图形。
- **moveTo(x, y)**
&emsp;&emsp;把 canvas 的当前路径的结束点移动到 x, y 对应的点。通常会使用moveTo()函数设置起点，也可以绘制一些不连续的路径。

#### 绘制直线
&emsp;&emsp;**lineTo(x, y)**：绘制一条从当前位置到指定x以及y位置的直线。
```
ctx.beginPath();
ctx.moveTo(125,125);
ctx.lineTo(125,45);
ctx.stroke();
```
#### 绘制三角形
&emsp;&emsp;绘制三角形其实就是绘制好闭合路径，然后对其进行填充或者描边的效果。
```
// 填充三角形
ctx.beginPath();
ctx.moveTo(25,25);
ctx.lineTo(105,25);
ctx.lineTo(25,105);
ctx.fill();

// 描边三角形
ctx.beginPath();
ctx.moveTo(125,125);
ctx.lineTo(125,45);
ctx.lineTo(45,125);
ctx.closePath();
ctx.stroke();
```
#### 绘制圆弧
- **arc(x, y, radius, startAngle, endAngle, anticlockwise)**
&emsp;&emsp;画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为false，顺时针）来生成。
- **arcTo(x1, y1, x2, y2, radius)**
&emsp;&emsp;根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点(x1, y1)和(x2, y2)。
```
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
ctx.beginPath();
ctx.arc(75, 75, 50, 0, 2 * Math.PI);
ctx.stroke();
```
#### 绘制贝塞尔曲线
- **quadraticCurveTo(cp1x, cp1y, x, y)**
&emsp;&emsp;绘制贝塞尔曲线，cp1x,cp1y为控制点，x,y为结束点。
- **bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)**
&emsp;&emsp;绘制二次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x，y为结束点。
```
//绘制一个对话气泡
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
ctx.beginPath();
ctx.moveTo(75,25);
ctx.quadraticCurveTo(25,25,25,62.5);
ctx.quadraticCurveTo(25,100,50,100);
ctx.quadraticCurveTo(50,120,30,125);
ctx.quadraticCurveTo(60,120,65,100);
ctx.quadraticCurveTo(125,100,125,62.5);
ctx.quadraticCurveTo(125,25,75,25);
ctx.stroke();

//绘制一颗爱心
ctx.beginPath();
ctx.moveTo(75,40);
ctx.bezierCurveTo(75,37,70,25,50,25);
ctx.bezierCurveTo(20,25,20,62.5,20,62.5);
ctx.bezierCurveTo(20,80,40,102,75,120);
ctx.bezierCurveTo(110,102,130,80,130,62.5);
ctx.bezierCurveTo(130,62.5,130,25,100,25);
ctx.bezierCurveTo(85,25,75,37,75,40);
ctx.fill();
```

## 添加颜色
- **fillStyle = color**：设置图形的填充颜色。
- **strokeStyle = color**：设置图形轮廓的颜色。
```
ctx.fillStyle = "blue";
ctx.fillRect(10, 10, 100, 100);
ctx.strokeStyle = "red";
ctx.strokeRect(10, 10, 100, 100);
```

## 绘制文本
- **fillText(text, x, y [, maxWidth])**
&emsp;&emsp;在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的。
- **strokeText(text, x, y [, maxWidth])**
&emsp;&emsp;在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的。
```
//文字是填充的
ctx.fillText("Hello world", 10, 50); 
//文字是边框的
ctx.strokeText("Hello world", 10, 50); 
```

## 使用图片
**drawImage(image, x, y)**
&emsp;&emsp;其中 image 是 image 或者 canvas 对象，x和y是其在目标canvas里的起始坐标。
```
var img = new Image();   // 创建一个img元素
img.onload = function(){
// 执行drawImage语句
 ctx.drawImage(img, 0, 0);
}
img.src = 'a.png'; // 设置图片源地址
```
&emsp;&emsp;若调用 drawImage 方法时，图片还没装载完，那么什么都不会发生。所以应该用load时间来保证不会在加载完毕之前使用这个图片。

## 状态的保存和恢复
&emsp;&emsp;**save()**和**restore()**方法是用来保存和恢复 canvas 状态的，都没有参数，这两个是相互匹配出现的。当save()方法被调用后，当前的状态就会被推送到栈中保存；当 restore 方法被调用后，上一个保存的状态就从栈中弹出，所有设定都会恢复。那么**为什么需要保存canvas状态呢**？
&emsp;&emsp;当我们对画布进行旋转，缩放，平移等操作的时候，我们只想对特定的元素进行操作，比如图片，一个矩形等，但是当你用canvas的方法来进行这些操作的时候，其实是对整个画布进行了操作，那么之后在画布上的元素都会受到影响，所以在操作之前调用canvas.save()来保存画布当前的状态，当操作之后取出之前保存过的状态，这样就不会对其他的元素进行影响。
```
ctx.save(); // 保存默认的状态
ctx.fillStyle = "green";
ctx.fillRect(10, 10, 100, 100);
ctx.restore(); // 还原到上次保存的默认状态
ctx.fillRect(150, 75, 100, 100);
```

## 变形
### 移动
**translate(x, y)**
&emsp;&emsp;translate方法接受两个参数。x 是左右偏移量，y 是上下偏移量。对于这个方法，很多人会有个误解，认为translate（x，y）是把点（x，y）作为新的坐标原点。其实并不是这样的。Translate(x,y)表示原来的原点分别在x轴和y轴偏移多远的距离，然后以偏移后的位置作为坐标原点。假如原点落在（1，1），那么translate（10，10）就是在原点（1，1）基础上分别在x轴、y轴移动10，则原点变为（11,11）。

### 旋转
**rotate(angle)**
&emsp;&emsp;这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。要注意的是这里指的是弧度，弧度和度的概念是不一样的，它们之间的关系可以用一个计算公式来表示，`弧度＝度 × π/180`。

### 缩放
**scale(x, y)**
&emsp;&emsp;scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。比如设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。

## 动画
画一帧动画的步骤：
（1）**清空 canvas**
&emsp;&emsp;除非接下来要画的内容会完全充满 canvas（如背景图），否则需要清空所有，可以用 clearRect()方法来清空。
（2）**保存 canvas 状态**
&emsp;&emsp;如果要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，需要先保存一下。
（3）**绘制动画图形**
&emsp;&emsp;这一步才是重绘动画帧。
（4）**恢复 canvas 状态**
&emsp;&emsp;如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。
```
var car = new Image();
function init(){
  car.src = 'car.png';
  window.requestAnimationFrame(draw);
}
function draw(){
      var ctx = document.getElementById('canvas').getContext('2d');
      ctx.save();
      ctx.clearRect(0, 0, 1920, 1200); 
      ctx.scale(0.5, 0.5); 
      ctx.rotate(90);
      ctx.drawImage(car, Math.floor(-car.width/2), Math.floor(-car.height/2));
      ctx.restore();
      window.requestAnimationFrame(draw);
  }
$(document).ready(function() {
      init();      
  });
```
&emsp;&emsp;**requestAnimationFrame函数**，传递一个callback参数，在下一个动画帧时，会调用callback。这个函数可以告诉浏览器希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。

## 使用多层画布
&emsp;&emsp;在画一个复杂的场景时，有些元素不断地改变或者移动，而其它的元素，如背景，永远不变。这个时候可以考虑用多个画布元素去创建不同层次来进行优化，将需要一直变动的元素和不变的元素分开放在不同层次的画布中，便于操作。这里要注意的是canvas**不能嵌套，只能层叠**。
&emsp;&emsp;多层的canvas可以通过设置定位来实现每一层都可独立移动而相互之间不影响。
```
//使用3个canvas
<div id="container">
  <canvas id="canvas1" width="480" height="320"></canvas>
  <canvas id="canvas2" width="480" height="320"></canvas>
  <canvas id="canvas3" width="480" height="320"></canvas>
</div>
 
<style>
  #container {
    width: 480px;
    height: 320px;
    position: relative;
   }
  canvas { position: absolute; }
  #canvas1 { z-index: 3 }
  #canvas2 { z-index: 2 }
  #canvas3 { z-index: 1 }
</style>
```
