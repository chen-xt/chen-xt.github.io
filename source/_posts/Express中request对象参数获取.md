---
title: ' Express中request对象参数获取'
date: 2016-11-19 14:17:22
tags: express
categories: Node.js
---
## 前言
&emsp;&emsp;在表单类的操作中，客户端的HTTP请求通常会通过GET或POST提交一些数据到服务端，所以需要在服务端识别这些数据并进行处理。常利用request对象的三个方法来进行操作。

## req.params
&emsp;&emsp;获取请求路径。
- 例1
       浏览器访问：localhost:3000/login
       服务端侦听：app.use('/:key',function(req,res){});
       则：req.params.key为login。
- 例2
       浏览器访问：localhost:3000/user/tom
       服务端侦听：app.use('/user/:name',function(req,res){});
       则：req.params.name为tom。

## req.query
&emsp;&emsp;获取GET请求的参数（即地址栏传递的参数）。
- 例
      浏览器访问：http://localhost:3000/login?username=Tom&&password=123
      服务端侦听：app.use('/:key',function(req,res){});
      则：req.query.usename和req.query.password分别为Jack和123。

## req.body
&emsp;&emsp;获取POST请求的参数。
- 例
      浏览器访问：http://localhost:3000/login，通过表单POST参数username=Tom && password=123
      服务端侦听：app.use('/:key',function(req,res){});
      则：req.body.usename和req.body.password分别为Tom和123。

## 注意点
1. req.params和req.query的主要区别：
     req.params包含路由参数（在URL的路径部分），而req.query包含URL的查询参数（在URL的？后的参数）。
2. req.param()方法已经被弃用。