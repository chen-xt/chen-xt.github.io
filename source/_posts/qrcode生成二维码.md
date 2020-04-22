---
title: qrcode生成二维码
date: 2019-01-16 00:32:49
tags: 二维码
categories: javascript
---
## 前言
&emsp;&emsp;js生成二维码有两种方法：分别是用qrcode.js和jquery.qrcode.js。

## qrcode
&emsp;&emsp;qrcode.js 是一个用于生成二维码图片的插件，主要是通过获取DOM的标签，再通过 HTML5 Canvas 绘制而成，不依赖任何库，**支持中文内容**。

### 使用方法
1. 载入js文件
```
    <script src="qrcode.js"></script>
```
2. DOM 结构
```
    <div id="code"></div>
```
3. 调用
``` 
    // 简单方式
    new QRCode(document.getElementById('code'), 'https://www.baidu.com/');

    // 设置参数方式
    var qrcode = new QRCode('code', {
        text: 'hello world!',
        width: 200,
        height: 200,
        colorDark : '#000',
        colorLight : '#fff',
        correctLevel : QRCode.CorrectLevel.H
    });

    // 使用 API
    qrcode.clear();
    qrcode.makeCode('哈哈哈');
```

### 参数说明
&emsp;&emsp;格式：**new QRCode(element, option)**
1. element：显示二维码的元素或该元素的id
2. option：参数配置，有以下几个参数：
（1）text：设置二维码的内容；
（2）width：设置二维码宽度，默认值是256；		
（3）height：设置二维码高度，默认值是256；		
（4）colorDark：设置前景色，默认值是"#000"	  
（5）colorLight：设置背景色，默认值是"#fff"
（6）typeNumber: 设置计算模式，默认值是4
（7）correctLevel : 设置容错级别，默认值是QRErrorCorrectLevel.H，可设置为QRCode.CorrectLevel.L(容错率7%)、QRCode.CorrectLevel.M(容错率15%)、QRCode.CorrectLevel.Q(容错率25%)、QRCode.CorrectLevel.H(容错率30%)；
注意：二维码容错率即是指二维码图标被遮挡多少后，仍可以被扫描出来的能力。容错率越高，则二维码图片能被遮挡的部分越多。容错率越高，越容易被快速扫描。因此推荐使用QRErrorCorrectLevel.H。

### API接口
1. **makeCode(text)**：设置二维码内容
2. **clear()**：清除二维码（仅在不支持 Canvas 的浏览器下有效）

### 参考网址
- [参考网址](http://code.ciaoca.com/javascript/qrcode/)


## jquery.qrcode.js
&emsp;&emsp;jquery.qrcode.js 是把qrcode.js用jquery方式封装起来的，用它来实现图形渲染，其实就是画图（支持canvas和table两种方式）默认是canvas，canvas方式还支持右键图片下载，**不支持中文内容**。

### 使用方法
1. 载入js文件
```
    <script src="jquery.qrcode.min.js"></script>
```
2. DOM 结构
```
    <div id="code"></div>
```
3. 调用
``` 
    // 简单方式
    // $("#code").qrcode("hi,chen!");

    // 直接生成二维码
    $("#code").qrcode({
        render: "canvas", //渲染方式
        width: 200,  //宽度
        height: 200, //高度
        text: "https://www.baidu.com/", //内容
        background: "#fff",  //背景色  
        foreground: "#000",   //前景色(二维码的颜色)
        correctLevel: 1,  //容错级别
    });
```

### 参数说明
（1）render:  设置渲染方式，支持canvas和table两种方式，默认是canvas；
（2）text：设置二维码的内容
（3）width：设置二维码宽度，默认值是256；		
（4）height：设置二维码高度，默认值是256；		
（5）foreground: 设置前景色，默认值是"#000"	  
（6）background: 设置背景色，默认值是"#fff"
（7）correctLevel : 设置容错级别
（8）typeNumber  : 设置计算模式

### 生成二维码图片
1. 载入js文件
```
    <script src="jquery.qrcode.min.js"></script>
```
2. DOM 结构
```
    <div id="code"></div>
    <img id="imgOne" src="">
```
3. 调用
```
    //生成二维码图片
    var qrcode = $('#code').qrcode("https://www.baidu.com/").hide();
    var canvas = qrcode.find('canvas').get(0);
    $('#imgOne').attr('src',canvas.toDataURL('image/jpg'));
```

### 解决无法生成中文二维码的问题
&emsp;&emsp;jquery-qrcode这个库是采用 charCodeAt() 这个方式进行编码转换的，这个方法默认会获取它的Unicode编码，一般的解码器都是采用UTF-8, ISO-8859-1等方式。英文是没有问题，如果是中文，一般情况下Unicode是UTF-16实现，长度2位，而UTF-8编码是3位，这样二维码的编解码就不匹配了。 所以可以在二维码编码前把字符串转换成UTF-8，就可以解决这个问题了。
&emsp;&emsp;相关代码如下：
```
 	$("#code").qrcode({
        render: "canvas", //渲染方式
        width: 200,  //宽度
        height: 200, //高度
        text: utf16to8("你好啊"), //内容
        background: "#fff",  //背景色  
        foreground: "#000",   //前景色(二维码的颜色)
        correctLevel: 1,  //容错级别
  	});

 	// 编码转换函数
    function utf16to8(str) {  
        var out, i, len, c;  
        out = "";  
        len = str.length;  
        for(i = 0; i < len; i++) {  
        c = str.charCodeAt(i);  
        if ((c >= 0x0001) && (c <= 0x007F)) {  
            out += str.charAt(i);  
        } else if (c > 0x07FF) {  
            out += String.fromCharCode(0xE0 | ((c >> 12) & 0x0F));  
            out += String.fromCharCode(0x80 | ((c >>  6) & 0x3F));  
            out += String.fromCharCode(0x80 | ((c >>  0) & 0x3F));  
        } else {  
            out += String.fromCharCode(0xC0 | ((c >>  6) & 0x1F));  
            out += String.fromCharCode(0x80 | ((c >>  0) & 0x3F));  
        }  
        }  
        return out;  
    }
```

### 参考网址
- [参考网址](https://github.com/jeromeetienne/jquery-qrcode)