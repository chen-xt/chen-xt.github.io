---
title: JS之闭包
date: 2016-08-10 
tags: 闭包
categories: javascript
---
## 什么是闭包？
闭包是指有权访问另外一个函数作用域中的变量的函数。
简单的说，在Javascript中，函数内部作用域可以访问外部作用域的变量，而外部作用域却无权访问内部作用域的局部变量。但是有时候我们需要在函数作用域外访问到函数内部的变量，这个时候就需要闭包的出场了。通过在函数的内部在创建一个函数，将内部函数作为返回值，就可以在函数外去访问到函数的内部变量。
## 闭包的特点
先看下面的一段关于闭包的代码，通过这段代码来逐一分析闭包的特点：
``` javascript
function aaa(a){
    var b=5;
    function bbb(){
        alert(a);
        alert(b);
}}
```
1. **函数嵌套函数**：
   aaa()函数里再嵌套了一个bbb()函数。
2. **内部函数可以引用外部函数的参数和变量**：
    bbb()函数里可以引用aaa()函数中的a参数和b变量。
3. **参数和变量不会被垃圾回收机制所收回**：
    a和b的值不会被垃圾回收机制回收，因为它们都要在内部函数bbb()中被引用。

## 使用闭包的好处 
1. **希望一个变量长期驻扎在内存当中（不会被垃圾回收机制回收）。**
2. **避免全局变量的污染(为了提高性能，尽量避免出现全局变量)。**
    (1) 全局变量的累加：
``` javascript
 var a=1;//全局变量
    function aaa(){
    a++;
    alert(a);
}
    aaa();//2
    aaa();//3
```
    (2) 将全局变量a变成局部变量(但是不能累加)：
``` javascript
    function aaa(){
    var a=1;
    a++;
    alert(a);
}
    aaa();//2
    aaa();//2
```
    (3) 不仅将全局变量a变成局部变量，还可以实现累加(采用闭包)：
``` javascript
function aaa(){
    var a=1;
    return function(){
    a++;
    alert(a);}}
    var b=aaa();
    b();//2
    b();//3
```
3. **私有成员的存在**

## 使用闭包的注意事项
### 内存泄露
由于闭包会携带私有成员的存在包含它的函数的作用域，因此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过多。所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。
解决方法是，在退出函数之前，将不使用的局部变量全部删除。
### 闭包和变量
闭包只能取得包含函数中的任何变量的最后一个值。闭包保存的是整个变量对象，而不是某个特殊的变量。看下面的例子：
``` javascript
var result=[];
function aa(){
    for (var i= 0;i<3;i=i+1){
        result[i]=function(){
            alert(i)
        }   }   };
aa();
result[0](); // 3
result[1](); // 3
result[2](); // 3
```
这段代码中,表面上看，aa函数中的变量i被内部循环的函数使用,并且能分别获得他们的索引值,而实际上,只能获得该变量最后保留的值。因为闭包中所记录的自由变量,只是对这个变量的一个引用,而非变量的值,当这个变量被改变了,闭包里获取到的变量值,也会被改变。
解决的方法：是让内部函数在循环创建的时候立即执行,并且捕捉当前的索引值,然后记录在自己的一个本地变量里.再利用返回函数的方法,重写内部函数,让下一次调用的时候,返回本地变量的值,改进后的代码:
``` javascript
var result=[];
function aa(){
    for ( var i= 0;i<3;i=i+1){
        result[i]=(function(j){
            return function(){
                alert(j);
            };
        })(i);
    }   };
aa();
result[0](); // 0
result[1](); // 1
result[2](); // 2
```
### 关于this对象
我们都知道，this对象是在运行时基于函数的执行环境绑定的，在全局函数中，this等于window。而当函数被作为某个对象的方法调用时，this等于那个对象。不过，在匿名函数中，其执行环境具有全局性，因此其this对象通常指的是window。下面看下一个例子：
``` javascript
var name = “The window”;
var object = {
    name: “My Object”,
    getNameFunc: function() {
        return function() {
            return this.name;
        }  }      }
console.log(object.getNameFunc()()); //The window
```
以上代码先创建了一个全局变量name，又创建了一个包含name属性的对象，这个对象还包含一个getNameFunc()方法，它返回一个匿名函数，而匿名函数又返回this.name。因此调用object.getNameFunc()()会立即调用它返回的函数，结果就是返回一个字符串，这个字符中就是全局name的值，即“The window”。如果想访问匿名函数外部作用域的this对象，可以做如下的改进：
``` javascript
var name = “The window”;
var object = {
    name: “My Object”,
    getNameFunc: function() {
        var that= this;
        return function() {
            return that.name;
        }    }  }
console.log(object.getNameFunc()()); //My Object
```
上述改进后的代码，在定义匿名函数之前，把this对象赋值给了一个名叫that的变量，而定义了闭包之后，闭包也可以访问这个变量，因为他是在包含函数中特意声明的一个变量，即使在函数返回后，that依旧引用着object，所以调用object.getNameFunc()()就返回了“My Object”。
## 闭包的用途
### 匿名自执行函数
函数内部声明变量的时候，如果不加上var关键字，就会默认声明一个全局变量，这样的临时变量加入全局对象有很多坏处，可能造成全局对象过于庞大，影响访问速度等等。除了每次使用变量都是用var关键字外，也遇到这样一种情况，就是有的函数只需要执行一次，其内部变量无需维护，这个时候就可以使用闭包。
``` javascript
var data= {    
    table : [],    
    tree : {}    
};      
(function(dm){    
    for(var i = 0; i < dm.table.rows; i++){    
       var row = dm.table.rows[i];    
       for(var j = 0; j < row.cells; i++){    
           drawCell(i, j);    
       }    
    }         
})(data); 
```
我们创建了一个匿名的函数，并立即执行它，由于外部无法引用它内部的变量，因此在执行完后很快就会被释放，这种机制不会污染全局对象。
### 结果缓存
在开发过程中，设想我们有一个处理过程很耗时的函数对象，每次调用都会花费很长时间，那么我们就需要将计算出来的值存储起来，当调用这个函数的时候，首先在缓存中查找，如果找不到，则进行计算，然后更新缓存并返回值，如果找到了，直接返回查找到的值即可。闭包正是可以做到这一点，因为它不会释放外部的引用，从而函数内部的值可以得以保留。
``` javascript
var CachedSearchBox = (function(){    
    var cache = {},    
       count = [];    
    return {    
       attachSearchBox : function(dsid){    
           if(dsid in cache){//如果结果在缓存中    
              return cache[dsid];//直接返回缓存中的对象    
           }    
           var fsb = new uikit.webctrl.SearchBox(dsid);//新建    
           cache[dsid] = fsb;//更新缓存    
           if(count.length > 100){//保正缓存的大小<=100    
              delete cache[count.shift()];    
           }    
           return fsb;          
       },    
       clearSearchBox : function(dsid){    
           if(dsid in cache){    
              cache[dsid].clearSelection();      
           }     }     };    
})();      
CachedSearchBox.attachSearchBox("input1"); 
```
当我们第二次调用CachedSearchBox.attachSerachBox(“input1”)的时候，我们就可以从缓存中读取到该对象，而不用再去创建一个新的searchbox对象。
### 封装
来看下面的一个例子，在person之外的地方无法访问其内部的变量，但是可以通过提供闭包的形式来访问。
``` javascript
var person = function(){    
    //变量作用域为函数内部，外部无法访问    
    var name = "default";       
       
    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
}();    
print(person.name);//直接访问，结果为undefined    
print(person.getName());    //default  
person.setName("abruzzi");    
print(person.getName());  //abruzzi 
```
### 实现类和继承
闭包的一个重要用途是实现面向对象中的对象，传统的对象语言都提供类的模板机制，这样不同的对象(类的实例)拥有独立的成员及状态，互不干涉。虽然JavaScript中没有类这样的机制，但是通过使用闭包，我们可以模拟出这样的机制。
``` javascript
function Person(){    
    var name = "default";       
    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
};    
     
var john = Person();    
print(john.getName());    //default
john.setName("john");    
print(john.getName());    //john
     
var jack = Person();    
print(jack.getName());    //default
jack.setName("jack");    
print(jack.getName());    //jack
```
上述的代码，定义了Person，它就像一个类，可以通过new一个Person对象，来访问它的方法。john和jack都可以称为是Person这个类的实例，它们继承了Person，并添加自己的方法，并且这两个实例对name这个成员的访问是独立的，互不影响的。

