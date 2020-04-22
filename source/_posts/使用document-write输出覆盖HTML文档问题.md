---
title: 使用document.write输出覆盖HTML文档问题
date: 2016-08-04 23:16:23
tags: 覆盖文档
categories: javascript
---
今天在W3C学习时，看到一行提示：“您只能在 HTML 输出中使用 document.write。如果您在文档加载后使用该方法，会覆盖整个文档。”感到很困惑，什么是文档加载后？为什么会覆盖整个文档？为此上网去查找了些资料。下面做下总结。
### 什么情况是文档加载后？
HTML文档加载，是自上而下的加载HTML表示的内容，当整个页面内容都加载完毕之后，再调用document.write()这方法会导致文档上所有内容被清空，只留下 document.write 中的内容。如下代码：
``` 
<body>
<button onclick="after()">点击这里</button> 
   <script type="text/javascript">
    document.write("hello");
    function after()
    {
     document.write("点击按钮后，会清除所有 ");
    }
    </script>
</body>
```
文档加载完成后，页面显示“hello”，当点击按钮后会清除并覆盖当前内容，显示“点击按钮后，会清除所有”。
### 为什么将该方法嵌套在函数中，就相当于文档加载后？
代码都是写在body标签中的，不是应该在加载时就执行了所有的内容，包括函数中的内容吗？其实不是这样的，如果将document.write直接写在script标签中，那么文档加载完成前会运行该代码；但是如果把document.write嵌套在函数中，则在文档加载完成前，只会声明该函数，而不会执行函数内的代码，具体函数内代码的执行，是等到文档加载完成后的。
### 为什么文档加载后使用该方法，会覆盖整个文档？
文档加载时，相当于打开一个document对象，此时如果遇到document.wirte，就会把内容加入到document中。但是当文档加载完成后，document对象就会被关闭，此时若再使用该方法，相当于重新打开一个document对象，这次打开，就会覆盖上一次打开所写的内容，对document对象进行重写。