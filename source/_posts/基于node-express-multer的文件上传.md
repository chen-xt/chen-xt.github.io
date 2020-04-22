---
title: 基于node-express-multer的文件上传
date: 2017-04-26 22:03:07
tags: multer
categories: Node.js
---
## 简介
&emsp;&emsp;**multer**是一个用以处理 **multipart/form-data** 请求的node.js中间件，主要用于上传文件。它基于 busboy 开发以提供最高的性能。本篇文章是基于 multer的1.3.0版本，express的4.13.4版本进行操作的，其中**index.js**是路由文件，用来处理文件上传请求，**form.ejs**是前端页面，用来上传文件。

## 安装
```
npm install --save multer
```

## 修改index.js
```
var multer  = require('multer')
var upload = multer({ dest: 'upload/' });//dest 是上传的文件所在的目录,可以自定义
```

## 上传单个文件
&emsp;&emsp;**.single(fieldname)：**接受一个名称的文件fieldname。单个文件将存储在req.file。
### form.ejs
```
<form action="/upload" method="post" enctype="multipart/form-data">
    <h2>单个文件上传</h2>
    <input type="file"  name="logo">
    <input type="submit" value="提交">
</form>
```
### index.js
```
router.post('/upload', upload.single('logo'), function(req, res, next){
//logo为图片的name属性
      res.redirect('/form');
      console.log(req.file.filename); //获取文件的filename值，其他属性类似
      console.log('单个文件上传');
});
```

## 上传多个相同name值的文件
&emsp;&emsp;**.array(fieldname[, maxCount])：**接受一系列的文件，所有的名称fieldname。如果多于maxCount上传文件，可以选择是错误输出。文件数组将存储在 req.files。
### form.ejs
```
<form action="/upload" method="post" enctype="multipart/form-data">
    <h2>多个文件上传</h2>
    <input type="file" name="logo">
    <input type="file" name="logo">
    <input type="submit" value="提交">
</form>
```
### index.js
```
router.post('/upload', upload.array('logo', 2), function(req, res, next){
    res.redirect('/form');
    console.log(rreq.files[0].filename); //获取文件的filename值，其他属性类似
    console.log('多个文件上传');
});
```

## 上传多个不同name值的文件
&emsp;&emsp;**.fields(fields)：**接受指定的文件混合fields。具有文件数组的对象将被存储req.files。fields应该是一个数组的对象name和可选的maxCount。
### form.ejs
```
<form action="/upload" method="post" enctype="multipart/form-data">
    <h2>多个不同name值文件上传</h2>
    <input type="file" name="file1">
    <input type="file" name="file2">
    <input type="file" name="file3">
    <input type="file" name="file4">
    <input type="file" name="file5">
    <input type="submit" value="提交">
</form>
```
### index.js
```
router.post('/upload', upload.fields([
    {name: 'file1'},
    {name: 'file2'},
    {name: 'file3'},
    {name: 'file4'},
    {name: 'file5'}
]), function(req, res, next){
     for(var i in req.files){
        console.log(req.files[i]);//输出每个文件的信息
        var a=req.files[i];
        console.log(a[0].filename);//获取文件的filename值，其他属性类似
    }
    res.redirect('/form');
    // console.log('多个不同name值文件');
});
```
&emsp;&emsp;每个文件对象包括以下信息:
* fieldname: 'file5',//在form表单中指定的name属性值
* originalname: 'good5.jpg',//原始文件名
* encoding: '7bit',//文件的编码类型
* mimetype: 'image/jpeg', //多媒体类型
* destination: 'upload/', //文件已被保存到的文件夹(ben)
* filename: '7df920656f2aea54d6f62f51f58db037', //保存在本地的文件名(保存后的名称)
* path: 'upload\\7df920656f2aea54d6f62f51f58db037',//文件保存的完整路径
* size: 88400 //文件大小，单位b

## 自定义文件名（filename）和文件保存路径
&emsp;&emsp;multer 提供了 **storage** 这个参数来对资源保存的路径、文件名进行自定义的设置。
将**index.js**文件中的`var upload = multer({ dest: 'upload/' });`
替换成如下：
```
var storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, './files');  //保存的路径，备注：需要自己创建
  },
  filename: function (req, file, cb) {
    cb(null, file.originalname);//文件名，这里设置成和原始名字一样
  }
});
var upload = multer({ storage: storage });
```
&emsp;&emsp;上述代码的执行结果：所有上传的文件都会保存到files文件夹中，且名字都和原始名字一样。

## 参考文档
&emsp;&emsp;multer官方文档：[https://github.com/expressjs/multer](https://github.com/expressjs/multer)