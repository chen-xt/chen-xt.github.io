---
title: 解决Next主题主页空白
date: 2016-11-19 16:39:35
tags: hexo
categories: hexo
---
## 前言
&emsp;&emsp;最近一个月都没有发布文章到hexo博客上，今天发布一篇学习笔记到博客，本地测试正常，但是部署到github上，再进行测试却发现博客全部空白了。网上找了好多资料，终于找到解决方法了。

## 问题出现原因
&emsp;&emsp;由于 GitHub 升级 gitpage，Next主题下面下`source/vendors`目录的访问受限。

## 解决方法
- 将`source/vendors`目录修改成`source/lib` （不一定是lib，也可以是其他的名称）。
- 修改下Next主题配置文件`_config.yml`， 将`_internal: vendors` 改成所修改的名字`_internal: lib`（其他名称的只需将lib替换即可）。
- 修改 next 底下所有引用`source/vendors`路径为`source/lib`：
   1.Hexo\themes\next.bowerrc
   2.Hexo\themes\next.gitignore
   3.Hexo\themes\next.javascript_ignore
   4.Hexo\themes\next\bower.json 
- 最后刷新下，然后`hexo g`，再`hexo d`，打开博客，发现内容正常显示。
