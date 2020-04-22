---
title: HTML媒体捕获实例
date: 2018-11-06 23:06:21
tags: input
categories: html5
---
## accept属性
&emsp;&emsp;规定了可通过文件上传提交的服务器接受的文件类型，仅适用于` <input type="file">`。
1. ` audio/*` ：接受所有的声音文件。
```
<input type="file" accept="audio/*">
```
2. ` video/*` ：接受所有的视频文件。
```
<input type="file" accept="video/*">
```
2. ` image/*` ：接受所有的图像文件。
```
<input type="file" accept="image/*">
```

## capture属性
&emsp;&emsp;可以捕获到系统默认的设备。
1. ` camera` ：照相机。
```
<input type="file" accept="image/*" capture="camera">
```
2. ` camcorder` ：摄像机。
```
<input type="file" accept="video/*" capture="camcorder">
```
3. ` microphone` ：录音。
```
<input type="file" accept="audio/*" capture="microphone">
```
&emsp;&emsp;注：`input:file`标签有个`multiple`属性，表示可以支持多选。加了这个属性，`capture`就没效果了。

## 常见例子
1. 调用拍照：微信环境及浏览器环境下，苹果手机、安卓手机均可以拍照。
```
<input type =”file”  accept =”image / *”  capture>
```
2. 调用录像：微信环境及浏览器环境下，苹果手机、安卓手机均可以录像。
```
<input type =”file”  accept =”video / *”  capture>
```
3. 调用音频：微信环境，苹果手机、安卓手机均可以调起录音机和文件；浏览器环境，苹果手机、安卓手机均只能调起录音机。
```
<input type =”file”  accept =”audio / *”  capture>
```
4. 高级用例：在标记中指定capture属性，并通过XMLHttpRequest在脚本中处理文件上载。微信环境及浏览器环境下，苹果手机、安卓手机均可以拍照。
```
<input type =”file”  accept =”image / *”  capture>
```
5. 无capture：微信环境，苹果手机、安卓手机均可以调起拍照和文件；浏览器环境，苹果手机可以调起拍照和相册，安卓手机可以调起拍照和文件。
```
<input type =”file”  accept =”image / *”>
```
6. 无capture：微信环境，苹果手机、安卓手机均可以调起文件；浏览器环境，苹果手机可以调起拍照、相册和文件，安卓手机可以调起拍照和文件。
```
<input type =”file”>
```

## 浏览器环境判断
```
    var ua = navigator.userAgent.toLowerCase();//获取浏览器的userAgent
    if(ua.match(/MicroMessenger/i) == "micromessenger") {
        // true为微信环境
        alert("微信环境");
    }
    else{
       // false为浏览器环境
        alert("浏览器环境");
    }
```

## 苹果、安卓设备判断
```
    var ua = navigator.userAgent.toLowerCase();//获取浏览器的userAgent
	var isIos = (ua.indexOf('iphone') != -1) || (ua.indexOf('ipad') != -1);
    if (isIos) {
        //true为苹果
    alert("苹果手机");
    }
    else{
        // false为安卓
        alert("安卓手机");
    }
```

## 参考地址
- [参考网址1](http://anssiko.github.io/html-media-capture/)
- [参考网址2](https://blog.csdn.net/bbt_yyc/article/details/78337501)

