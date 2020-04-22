---
title: Node.js 编程规范
date: 2016-10-14 16:03:26
tags: node规范
categories: Node.js
---
## 前言
&emsp;&emsp;其实并没有一个官方的文档规定Node.js应用程序代码的风格，但Node.js代码分割有着一些“事实上的约定”，大多数项目的代码都一定程度上遵循了这一标准。 所以遵守这些约定还是有一定的必要的，因为它可能会让你的程序避免很多意外的错误和性能损失。
## 缩进
&emsp;&emsp;选择**两空格**作为Node.js代码的缩进标记。为什么不用常见的四空格缩进呢？因为Node.js代码中很容易写出深层的函数嵌套，过多的空格会给阅读带来不便。
``` html
//正确的缩进
function func(boolVar) {
   if (boolVar) {
     console.log('True');
   } else {
     console.log('False');
   }
};

//错误的缩进
function func(boolVar)
{
     if (boolVar)
    {
       console.log('True');
     }
    else
    {
       console.log('False');
     }
};
```
## 行宽
&emsp;&emsp;建议把行宽限制为**80个字符**，可以保证在任何设备上都可以方便地阅读。
## 语句分隔符
&emsp;&emsp; JavaScript不仅支持像C语言一样的分号`; `作为语句之间的分隔符，还支持像Python语言那样的换行作为语句之间的界限。但是建议一律使用**分号**，哪怕一行只有一个语句，也不要省略分号。
``` html
//正确的语句分隔
var a = 1;
var b = 'world';
var c = function(x) {
    console.log('hello ' + x + a);
};
c(b);

//错误的语句分隔
var a = 1
var b = 'world'
var c = function(x) {
    console.log('hello ' + x + a)
}
c(b)
```
## 变量定义
（1）不要通过赋值隐式定义变量，因为通过赋值隐式定义的变量总是全局变量，会造成命名空间污染。所以要通过 `var` 把所有变量定义为局部变量。
（2）使用` var `定义多个变量时，不要通过逗号`, `把多个变量隔开。
``` html
//正确的变量定义格式
var foo;
var bar;
var arr = [40, 'foo'];
var obj = {};

//错误的变量定义格式
var foo, bar;
var arr = [40, 'foo'],
obj = {}; 
```
## 变量名和属性名
&emsp;&emsp;使用**小驼峰式命名法**作为所有变量和属性的命名规则。小驼峰式命名法，即第一个单字以小写字母开始，第二个单字的首字母大写，如firstName。
``` html
//正确的命名
var yourName = 'BYVoid';

//错误的命名
var YourName = 'BYVoid';
//或者
var your_name = 'BYVoid';
```
## 函数
&emsp;&emsp;对于**一般的函数**我们使用小驼峰式命名法。但对于**对象的构造函数**名称（或者不严格地说“类”的名称），使用大驼峰式命名法。大驼峰式命名法，即每一个单字的首字母都采用大写字母，如FirstName。
``` html
//正确
var someFunction = function() {
   return 'something';
};
function anotherFunction() {
   return 'anything';
}
function DataStructure() {
   this.someProperty = 'initialized';
}

//错误
var SomeFunction = function()
{
   return 'something';
};
function another_function () {
   return 'anything';
}
function dataStructure() {
this.someProperty = 'initialized';}
```
## 引号
&emsp;&emsp;在JavaScript中单引号`' `和双引号`" `没有任何语义区别，两者都是可用的。建议用**单引号**，因为JSON、 XML都规定了必须是双引号，这样便于无转义地直接引用。
``` html
//正确的引号用法
console.log('Hello world.');

//错误的引号用法
console.log("Hello world."); 
```
## 关联数组的初始化
&emsp;&emsp;按`var = { 放在一行，下面每行一对键值，保持两空格的缩进，以分号结尾， }; `的形式，最后单独另起一行。对于每对键值，除非键名之中有空格或者有非法字符，否则一律不用引号。
``` html
//正确
var anObject = {
    name: 'BYVoid',
    website: 'http://www.byvoid.com/',
    'is good': true,
};
//错误
    var anObject = {'name': 'BYVoid',
    website: 'http://www.byvoid.com/'
    , "is good": true};
```
## 等号 
&emsp;&emsp;尽量使用 `=== `而不是` == `来判断相等，因为` ==` 包含了隐式类型转换，很多时候可能会出错。
## 命名函数 
&emsp;&emsp;尽量给构造函数和回调函数命名。对于回调函数，Node.js的API和各个第三方的模块通常约定回调函数的第一个参数是错误对象`err`，如果没有错误发生，其值为 `undefined`。
``` html
//正确
req.on('end', function onEnd(err, message) {
   if (err) {
    console.log('Error.');
   }
});
function FooObj() {
    this.foo = 'bar';
}

//错误
req.on('end', function (message, err) {
    if (err === false) {
      console.log('Error.');
   }
});
var FooObj = function() {
this.foo = 'bar';
}
```
## 对象定义 
&emsp;&emsp;尽量将所有的成员函数通过原型定义，将属性在构造函数内定义，然后对构造函数使用`new `关键字创建对象。避免把成员函数定义在构造函数内部，否则会有运行时的闭包开销。
``` html
//正确:
function FooObj(bar) {
//在构造函数中初始化属性
    this.bar = bar;
    this.arr = [1, 2, 3];
}
//使用原型定义成员函数
FooObj.prototype.func = function() {
    console.log(this.arr);
};
var obj1 = new FooObj('obj1');
var obj2 = new FooObj('obj2');
obj1.arr.push(4);
obj1.func(); // [1, 2, 3, 4]
obj2.func(); // [1, 2, 3]

//错误：
function FooObj(bar) {
    this.bar = bar;
this.func = function() {
    console.log(this.arr);
};
}
FooObj.prototype.arr = [1, 2, 3];
var obj1 = new FooObj('obj1');
var obj2 = new FooObj('obj2');
obj1.arr.push(4);
obj1.func(); // [1, 2, 3, 4]
obj2.func(); // [1, 2, 3, 4]
```
## 继承 
&emsp;&emsp;避免使用复杂的**继承**，如果的确需要继承，那么尽量使用Node.js的util模块中提供的`inherits`函数。
``` html
var util = require('util');
var events = require('events');
function Foo() {
   }
util.inherits(Foo, events.EventEmitter); //让Foo继承EventEmitter
```
