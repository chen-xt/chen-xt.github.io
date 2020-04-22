---
title: JS之JSON
date: 2016-08-19 23:43:27
tags: JSON
categories: javascript
---
**JSON**(JavaScript Object Notation，JavaScript对象表示法)，是一个轻量级的数据格式，可以简化表示复杂数据结构的工作量。
## 语法
JSON的语法可以有3种类型的值：简单值、对象和数组。
### 简单值
简单值是最简单的JavaScript数据形式。在JSON中可以表示字符串、数值、布尔值和null，比如可以表示字符串：”good night”，但是JSON不支持JavaScript中的特殊值undefined。
注意：JSON字符串必须使用双引号(单引号会导致语法错误)，这是JSON字符串与JavaScript字符串的最大区别。
### 对象
JSON对象同JavaScript对象字面量相似，但是也有两点不同之处：(1)JSON对象没有声明变量，因此JSON中没有变量的概念。(2)JSON没有末尾的分号。JSON对象允许对象中嵌入对象。
注意：对象的属性必须加双引号；同一个对象中绝不能出现两个同名属性。
``` javascript
{
    “name”:”rose”,
    “age”:20,
    “school”:{ //对象中嵌套对象
        “name”:”WuYi College”,
        “location”:”NanPing”  //没有末尾的分号
    }
}
```
### 数组
JSON数组采用的是JavaScript中的数组字面量形式。比如：[23,”hello”,false]。
注意，JSON数组也没有变量和分号。把数组和对象结合起来，可以构成更复杂的数据集合。
``` javascript
[{
    “name”:”rose”,
    “age”:20,
    “school”:[
    ”WuYi College”
    ]
}]
```
## 解析与序列化
JSON可以把JSON数据结构解析为有用的JavaScript对象，这是其一大优势。通过两个方法JSON对象可以对JavaScript对象进行序列化和解析：
- stringify()方法：把JavaScript对象序列化为JSON字符串。在序列化JavaScript对象时，所有函数及原型成员都会被忽略，值为undefined的任何属性也会被跳过。
- parse()方法：把JSON字符串解析原生JavaScript值。

### stringify()方法
JSON. stringify()方法接收3个参数：①要序列化的对象；②过滤器，可以是一个数组或一个函数(替换(过滤)函数)；③一个选项，表示是否在 JSON字符串中保留缩进。

#### 过滤结果
(1) 过滤器参数为数组：
JSON. stringify()的结果中将只包含数组中列出的属性。看下面一个例子：
``` html
var person={
    "name":"rose",
    "age":20,
    "school":[
        "WuYi College"
    ]
    }
    var jsonText = JSON.stringify(person,["name","age"]);
```
如上面的例子，JSON. stringify()的第二个参数是一个数组，包含name和age两个属性，所以在返回的结果字符串中，就只会包含这两个属性，序列化的结果如下：{“name”:”rose”,“age”:20}。
(2) 过滤器参数为函数：
传入的函数接收2个参数：属性(键)名和属性值。函数返回的值就是相应键的值，当函数返回了undefined，那么相应的属性会被忽略。
``` html
var person={
    "name":"rose",
    "age":20,
    "school":[
        "WuYi College"
    ]
}
var jsonText = JSON.stringify(person,function(key,value){
 switch(key){
                case "name":
                  return value.join(",")
                case "age":
                  return 25;
                case "school":
                  return undefined;
                default:
                  return value;
        }
});
```
上面的例子中，如果键为"name"，就将数组连接为一个字符串；如果键为 "age"，则将其值设置为25；如果键为"school"，通过返回undefined删除该属性。最后，提供一个default项，返回传入的值。序列化的结果如下：{"name":"rose","age":25,}。

#### 字符串缩进
JSON. stringify()的第3个参数用于控制结果中缩进和空白符，它的类型有2种：数值和字符串。
(1) 参数是数值
数值表示的是每个级别缩进的空格数。但是缩进的空格数是有限制的，最大缩进空格数为10，所以大于10的值都会自动转换为10。
``` javascript
var jsonText = JSON.stringify(person, null, 3);//每个级别缩进3个空格
```
(2) 参数是字符串
字符串将在JSON字符串中被用作缩进字符，不再使用空格。同样的，字符串的长度也是有限制的，最长不能超过10，一旦超过，只会显示前10个字符。
``` javascript
var jsonText = JSON.stringify(person, null, "--");//每个级别缩进两个短划线
```

### parse()方法
 JSON. parse()方法接收2个参数：①要序列化的对象；②一个函数(还原函数)。
传入的函数接收2个参数：属性(键)名和属性值。函数返回的值就是相应键的值。当函数返回了undefined，那么相应的属性会被删除；当函数返回其他值，则将该值插入到结果中。
``` html
var person={
    "name":"rose",
    "age":20,
    "school":[
        "WuYi College"
    ],
    releaseDate: new Date(2016,8,18) 
   }
var jsonText = JSON.stringify(person);

var personCopy = JSON.parse(jsonText,function(key,value){
      if (key == "releaseDate"){
                return new Date(value);
            } else {
                return value;
            }
});

    alert(personCopy.releaseDate.getFullYear());//2016
```
如上面的例子，先为person对象新增一个releaseDate属性，该属性保存一个Date对象，这个对象经过序列化后变成有效的JSON字符串，然后经过解析又在 personCopy中还原为一个Date对象。还原对象在遇到”releaseDate”键时，会基于相应的值创建一个新的Date对象。结果就是personCopy.releaseDate属性中会保存一个Date对象，因此才能调用getFullYear()方法。