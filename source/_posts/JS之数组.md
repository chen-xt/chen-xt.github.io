---
title: JS之数组
date: 2016-08-14 17:06:15
tags: 数组
categories: javascript
---
## 创建数组
创建数组的方式有2种，如下：
#### 使用Array构造函数
1. 不传入参数
``` javascript
var students= new Array();//创建一个空数组
```
2. 传入一个数值作为参数，指定数组的长度
``` javascript
var students= new Array(10); //创建一个length值为10的数组
```
3. 传入一个或者多个数组元素作为参数
``` javascript
var students= new Array(“red”,”blue”); //创建包含2个字符串值的数组
```

#### 使用数组字面量表示法
数组字面量由一对包含数组项的方括号表示，多个数组项之间以逗号隔开。
``` javascript
var names = [];//创建一个空数组
var colors = ["red", "blue"];//创建包含2个字符串值的数组
```

## 检测数组
对于一个网页或者一个全局作用域而言，使用**instanceof操作符**即可确定某个对象是不是数组。但是在当存在多个全局作用域(如一个页面包含多个frame)的情况下，使用instanceof操作符就会出现各种问题。因此可以使用**Array.isArray()**方法，该方法的目的是最终确定某个值是不是数组，而不管它是在哪个全局执行环境中创建的。
``` javascript
if(value instanceof Array){  //instanceof操作符
//对数组执行某些操作
}
if(Array.isArray(value)){  //Array.isArray()方法
//对数组执行某些操作
}
```
## 转换方法
- **toLocaleString()：**返回对象的字符串表示，该字符串与执行环境的地区对应。
- **toString()：**返回对象的字符串表示。
- **valueOf()：**返回对象的字符串、数组或布尔值。通常与toString()方法的返回值相同。
- **join()：**返回对象用符号连接的的字符串表示。join()方法只接收一个参数，即作为分隔符的字符串，如果没有传入参数，则默认以逗号作为分隔符。
``` javascript
var colors = ["red", "blue", "green"]; //创建一个包含3个字符串的数组  
alert(colors.toString());    //red,blue,green
alert(colors.valueOf());    //red,blue,green
alert(colors.toLocaleString());    //red,blue,green
alert(colors.join('||'));    //red||blue||green
```

## 栈方法
栈是一种后进先出的数据结构，最新添加的项最早被移除。栈中项的插入和移除都只发生在一个位置——栈的顶部。数组可以表现的像栈一样，通过push()方法和pop()方法实现类似栈的行为。
- **push()：**可以接收任何数量的参数，把它们逐个添加到数组的末尾，并**返回数组的长度**。
- **pop()：**从数组末尾移除最后一项，减少数组的length值，然后**返回移除的项**。
``` javascript
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green");  //推入两项
alert(count);  //2  

var item = colors.pop();         //取得最后一项
alert(item);   //"green"
alert(colors.length);  //1
```
## 队列方法
队列是一种先进先出的数据结构，队列在列表的末端添加项，从列表的前端移除项。数组可以表现的像队列一样，通过push()方法和shift()方法实现类似队列的行为。
- **push()：**向数组末端添加项。
- **shift()：**移除数组中的第一个想并返回该项，同时将数组长度减1。
``` javascript
var colors = new Array();     //创建一个数组
var count = colors.push("red", "green","black");   //从末端推入三项
alert(count);  //3
        
var item = colors.shift();    //取得第一项
alert(item);   //"red"
alert(colors.length);  //2
```
数组还有一个**unshift()方法**，与shift()方法的用途恰恰相反，在数组的前端添加任意个项并返回新数组的长度，同时使用unshift()方法和pop()方法，可以实现模拟队列的效果，即在数组的前端添加项，从数组的末端移除项。
``` javascript
var colors = new Array();   //创建一个数组
var count = colors.unshift("red", "green","black");    //从前端推入三项
alert(count);  //3
       
var item = colors.pop();   //取得最后一项
alert(item);   //"black"
alert(colors.length);  //2
```
## 重排序方法
数组中有两种可以直接用来重排序的方法：reverse()和sort()。
- **reverse()：**反转数组项的顺序。
``` javascript
var values = [1, 2, 3, 4, 5];
values.reverse();
alert(values);       //5,4,3,2,1
```
- **sort()：**对数组进行排序。sort()方法比较的是字符串，即使数组中的每一项都是数值，可以看下面的例子：
``` javascript
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values);    //0,1,10,15,5
```
上面的例子中，虽然数值5小于10，但在进行字符串比较时，“10”则位于“5”前面，所以并非像我们所想象的那样，按数值的大小进行排序。
因此sort()方法可以接收一个比较函数compare(a,b)作为参数，这个比较函数接收两个参数a和b，a,b表示数组中的任意两个元素。如果a应该位于b之前，则返回一个负数；如果a等于b，则返回0；如果a应该位于b之后，则返回一个正数。这里产生的是升序排列的结果。
``` javascript
        function compare(a, b) {
            if a< b) {
                return -1;
            } else if (a > b) {
                return 1;
            } else {
                return 0;
            }
        }
        
        var values = [0, 1, 5, 10, 15];
        values.sort(compare);
        alert(values);    //0,1,5,10,15
```
如果想产生降序排列的结果，只需交换比较函数的返回值即可。

## 操作方法
- **concat()：**基于当前数组中所有项创建一个新数组。这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。
(1) 没有参数：只是复制当前数组并返回副本。
(2)有参数：传递值或者数组，则将这些值或者或者数组中每一项都添加到结果数组的末尾。
``` javascript
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);
alert(colors);     //red,green,blue        
alert(colors2);    //red,green,blue,yellow,black,brown
```
- **slice()：**基于当前数组中的一或多个项创建一个新数组。该方法接收一或
两个参数，即要返回项的起始和结束位置。
(1) 一个参数：返回从参数指定位置开始到当前数组末尾的所有项。
(2) 两个参数：返回起始和结束位置之间的项，但不包括结束位置的项。
(3) 参数为负数：使用数组长度加上该数来确定相应的位置。
``` javascript
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);//不包括结束位置的项
var colors4 = colors.slice(-2,-1);//与slice(3,-4)结果相同

alert(colors2);   //green,blue,yellow,purple
alert(colors3);   //green,blue,yellow
alert(colors4);  //yellow
```
- **splice()：**向数组的中部插入项。可以实现三种效果：
(1) **删除：**可以删除任意数量的项。指定2个参数：要删除的第一项的位置和要删除的项数。例如：splice(0,1)会删除数组中的第一项。
(2) **插入：**可以向指定位置插入任意数量的项。指定3个参数：起始位置、
0（要删除的项数）和要插入的项。例如：splice(1,0,3)会从位置1插入数字3。
(3) **替换：**可以向指定位置插入任意数量的项，且同时删除任意数量的项，指定3个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的数量不必与删除的数量相等。例如，splice(1,1,3,2)会删除当前数组位置1的项，容纳后再从位置1开始插入数字3和2。

## 位置方法
**indexOf()和lastIndexOf()方法**是数组的两个位置方法，这两个方法接收两个参数：要查找的项和表示查找起点位置的索引(可选的)，都是返回要查找的项在数组中的位置，如果没找到，则返回-1.
两个方法主要的区别：indexOf()方法从数组的**开头**(位置为0)开始向后查找，而lastIndexOf()方法则是从数组的**末尾**开始向前查找。
``` javascript
var numbers = [1,2,3,4,5,4,3];   
alert(numbers.indexOf(4));        //3
alert(numbers.lastIndexOf(4));    //5
        
alert(numbers.indexOf(4, 3));     //3
alert(numbers.lastIndexOf(4, 5)); //5
alert(numbers.indexOf(6));  //-1  
```
## 迭代方法
数组具有5个迭代方法，每个方法都接收2个参数：①要在每一项运行的函数；②(可选的)运行在该函数的作用域对象——影响this的值。对于参数一传入的函数会接收3个参数：数组项的值、该项在数组中的位置和数组对象本身。
- **every()和some()方法：**用于查询数组中的项是否满足某个条件，返回true或false。二者的区别在于，every()方法，传入的函数必须对**每一项**都返回true，这个方法才返回true；而some()方法则只有传入的函数对数组的**某一项**返回true，就会返回true。
``` javascript
var numbers = [1,2,3,4,5,4];   
var everyResult = numbers.every(function(item, index, array){
    return (item > 2);
  });
alert(everyResult);       //false
        
var someResult = numbers.some(function(item, index, array){
    return (item > 2);
  });
alert(someResult);       //true
```
- **filter()：**对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。这个方法对查询某些符合条件的所有数组项非常有用。
``` javascript
var numbers = [1,2,3,4,5,4];
var filterResult = numbers.filter(function(item, index, array){
     return (item > 2);
    });
alert(filterResult);   //返回所有数值大于2的数组：[3,4,5,4]
```
- **map()：**对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。这个方法适合创建包含的项与另一个数组一一对应的数组。下面例子返回数组中每一项乘于2所得乘积组成的数组。
``` javascript
var numbers = [1,2,3,4,5];    
var mapResult = numbers.map(function(item, index, array){
      return item * 2;
   });
alert(mapResult);   //[2,4,6,8,10]

```
- **forEach()：**对数组中的每一项运行给定函数，没有返回值。本质上与使用for迭代数组一样。
``` javascript
var numbers = [1,2,3,4,5];  
numbers.forEach(function(item, index, arry){  //对数组的每个元素累加
  arry[index] = item+1;
})
alert(numbers);  //[ 2, 3, 4, 5, 6]
```

## 归并方法
**reduce()和reduceRight()方法**： 都会迭代数组的所有项，然后构建一个最终返回的值。两个方法都接收2个参数：①一个在每一项调用的函数；②作为归并基础的初始值（可选的），这个参数如果没有指定，那么将使用数组的第一个元素作为初始值。对于传入到两种方法的函数也接收4个参数：前一个值、当前值、项的索引和数组对象。 
那么两种方法区别在哪呢？跟indexOf()和lastIndexOf()方法很相似，二者的区别在于，reduce()方法从数组的**第一项**开始，逐个遍历到最后；而reduceRight()方法则从数组的**最后一项**开始，向前遍历到第一项。下面例子为对数组中所有值进行求和的结果。
``` javascript
var values = [1,2,3,4,5];
var sum1 = values.reduce(function(prev, cur, index, array){
      return prev + cur;
    });
alert(sum1);//15

var sum2 = values.reduceRight(function(prev, cur, index, array){
       return prev + cur;
    });
alert(sum2);//15
```
可以看到，除了遍历数组的方向相反外，reduce()和reduceRight()方法完全相同。