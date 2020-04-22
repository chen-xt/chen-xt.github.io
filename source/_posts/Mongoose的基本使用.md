---
title: Mongoose的基本使用
date: 2016-12-18 13:10:21
tags: mongodb
categories: Node.js
---
## 前言
&emsp;&emsp;**MongoDB** 是一个对象数据库，它没有表、行等概念，也没有固定的模式和结构，所有的数据以文档的形式存储。在MongoDB中，多个文档(Document)可以组成集合(Collection)，多个集合又可以组成数据库。 
&emsp;&emsp;**Mongoose**是MongoDB的一个对象模型工具,也是针对MongoDB操作的一个对象模型库，封装了MongoDB对文档的的一些增删改查等常用方法，让NodeJS操作Mongodb数据库变得更加灵活简单。


## 安装及引用
### 安装mongoose
`npm install mongoose`
### 引用mongoose
`var mongoose = require("mongoose");`
### 使用"mongoose"连接数据库
`var db = mongoose.connect("mongodb://user:pass@localhost:port/database");`


## 基础
### Schema 
&emsp;&emsp;一种以文件形式存储的数据库模型骨架，不具备对数据库的操作能力，仅仅只是数据库模型在程序片段中的一种表现，可以说是数据属性模型，又或者是“集合”的模型骨架。
&emsp;&emsp;基本属性类型有：字符串、日期型、数值型、布尔型、null、数组、内嵌文档等。
```
var mongoose = require("mongoose");
var TestSchema = new mongoose.Schema({
    name : { type:String }, //属性name,类型为String
    age  : { type:Number, default:0 },      //属性age,类型为Number,默认为0
    time : { type:Date, default:Date.now },
    email: { type:String,default:''}
});
```

### Model 
&emsp;&emsp;由Schema构造生成的模型，除了Schema定义的数据库骨架以外，还具有数据库操作的行为。
```
//通过Schema来创建Model
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestModel = db.model("test1", TestSchema);   // 创建Model
//test1为数据库中的集合名称，添加数据时如果test1已经存在，则会保存到其目录下，如果未存在，则会创建test1集合，然后再保存数据。
```

### Entity 
&emsp;&emsp;由Model创建的实体，使用save方法保存数据，Model和Entity都有能影响数据库的操作，但Model比Entity更具操作性。
```
//使用Model创建Entity
var TestEntity = new TestModel({
       name : "Lenka",
       age  : 36,
       email: "lenka@qq.com"
});
console.log(TestEntity.name);   // Lenka
console.log(TestEntity.age);  // 36
```


## 保存数据
### Model保存
&emsp;&emsp;Model.create(文档数据, callback));
```
Model.create({ name:"model_create", age:26}, function(error,doc){
    if(error) {
        console.log(error);
    } else {
        console.log(doc);
    }
});
```


### Entity保存
&emsp;&emsp;Entity.save(文档数据, callback));
```
var Entity = new Model({name:"entity_save",age: 27});
Entity.save(function(error,doc) {
    if(error) {
        console.log(error);
    } else {
        console.log(doc);
    }
});
```
&emsp;&emsp;**注意：** model调用的是create方法，entity调用的是save方法。


## 更新数据
&emsp;&emsp;根据条件更新相关数据。
&emsp;&emsp;&emsp;&emsp;obj.update(查询条件,更新对象,callback);
```
var conditions = {name : 'test_update'}; 
var update = {$set : { age : 16 }};
TestModel.update(conditions, update, function(error){
    if(error) {
        console.log(error);
    } else {
        console.log('Update success!');
    }
});
```


## 删除数据
&emsp;&emsp;根据条件删除相关数据。
&emsp;&emsp;&emsp;&emsp;obj.remove(查询条件,callback);
```
var conditions = { name: 'tom' }; 
TestModel.remove(conditions, function(error){
    if(error) {
        console.log(error);
    } else {
        console.log('Delete success!');
    }
}); 
```


## 查询数据
### 简单查询
#### find查询
&emsp;&emsp;返回符合条件一个、多个或者空数组文档结果。
&emsp;&emsp;&emsp;&emsp;obj.find(查询条件,callback);
```
Model.find({},function(error,docs){
    console.log(docs);//没有向find传递参数，默认的是显示所有文档
});
 
Model.find({ "age": 28 }, function (error, docs) {
  if(error){
     console.log("error :" + error);
  }else{
     console.log(docs); //显示 age为28的所有文档
  }
}); 
```

#### find过滤查询
&emsp;&emsp;可以过滤返回结果所显示的属性个数。
&emsp;&emsp;&emsp;&emsp;find(Conditions,field,callback); 其中field省略或为Null，则返回所有属性。
```
//返回只包含一个键值name、age的所有记录
Model.find({},{name:1, age:1, _id:0}，function(err,docs){
    console.log(docs);
})
```
&emsp;&emsp;把要显示的数据设置为大于0 的数即可，通常设置为1；_id是默认返回，不显示则设置为0.但是，对于非 _id的属性不显示的话不可以设置为0，否则会抛异常或查询无果。

#### findOne查询
&emsp;&emsp;只返回第一个符合条件的文档数据。
&emsp;&emsp;&emsp;&emsp;findOne(Conditions,callback);
```
// 查询符合age等于27的第一条数据
TestModel.findOne({ age: 27}, function (err, doc){
       console.log(doc);
});
```

#### findById查询
&emsp;&emsp;只接收文档的_id作为参数，返回单个文档。
&emsp;&emsp;&emsp;&emsp;findById(_id, callback);
```
TestModel.findById('obj._id', function (err, doc){
    console.log(doc);
});
```

### 高级查询
#### $gt(>)、$lt(<)、$lte(<=)、$gte(>=)操作符
&emsp;&emsp;针对Number类型的查询具有超强的排除性。
```
//查询所有age大于18的数据
Model.find({"age":{"$gt":18}},function(error,docs){
    console.log(docs);
});
//查询所有age小于60的数据
Model.find({"age":{"$lt":60}},function(error,docs){
     console.log(docs);
});
//查询所有age大于等于18小于60的数据
Model.find({"age":{"$gte":18,"$lt":60}},function(error,docs){
    console.log(docs);
  });
```

#### $ne(!=)操作符
&emsp;&emsp;相当于不等于、不包含，查询时可根据单个或多个属性进行结果排除。$ne可以匹配单个值，也可以匹配不同类型的值。
```
//查询age不等于24的所有数据
Model.find({ age:{ $ne:24}},function(error,docs){
    console.log(docs);
});
//查询name不等于tom、age>=18的所有数据
Model.find({name:{$ne:"tom"},age:{$gte:18}},function(error,docs){
    console.log(docs);
 });
```

#### $in操作符
&emsp;&emsp;查找包含于指定字段条件的数据。和$ne操作符用法相同，但意义相反。
```
//查询age等于20的所有数据
Model.find({ age:{ $in: 20}},function(error,docs){
    console.log(docs);
});
//查询age为20和30的所有数据
Model.find({ age:{$in:[20,30]}},function(error,docs){
    console.log(docs);
}); 
```

#### $or操作符
&emsp;&emsp;可查询多个条件，只要满足其中一个就可返回结果值。
```
//查询name为yaya或age为28的全部文档
Model.find({"$or":[{"name":"yaya"},{"age":28}]},function(error,docs){
    console.log(docs);
});
```

#### $exists操作符
&emsp;&emsp;主要用于判断某些属性是否存在。 
```
//查询所有存在name属性的文档
Model.find({name: {$exists: true}},function(error,docs){
    console.log(docs);
});
//查询所有不存在telephone属性的文档
Model.find({telephone: {$exists: false}},function(error,docs){
  console.log(docs);
});
```


## 游标
&emsp;&emsp;对数据库返回的find执行结果进行控制，可以限制结果的数量，略过部分结果，根据任意键按任意顺序的组合对结果进行各种排序，或者是执行其他一些强的操作。主要包括限制返回结果的数量(limit函数)、忽略一点数量的结果(skip函数)以及排序(sort函数)。
### limit函数
&emsp;&emsp;限制返回结果的数量（limit函数指定的是上限而非下限）。
&emsp;&emsp;&emsp;&emsp;find(Conditions,fields,options,callback);
```
//以age为条件、过滤属性为null的所有文档，限制文档数量为20
Model.find({age:27},null,{limit:20},function(err,docs){
    console.log(docs);
});
//如果匹配的结果不到20个，则返回匹配数量的结果。
```

### skip函数
&emsp;&emsp;略过指定数量的匹配结果，返回余下的查询结果。
&emsp;&emsp;&emsp;&emsp;find(Conditions,fields,options,callback);
```
//跳过前4条查询结果，只返回剩下的查询结果
Model.find({},null,{skip:4},function(err,docs){
    console.log(docs);
});
//如果查询结果数量中少于4个的话，则不会返回任何结果。
```

### sort函数
&emsp;&emsp;对返回结果进行有效排序。该函数的参数是一个或多个键/值对，键代表要排序的键名，值代表排序的方向，1是升序，-1是降序。
&emsp;&emsp;&emsp;&emsp;find(Conditions,fields,options,callback);
```
//查询所有数据，并按照age降序顺序返回数据docs
Model.find({},null,{sort:{age:-1}},function(err,docs){
        console.log(docs);
 });
//查询所有文档(只显示属性name和age)且按照age升序展示结果
TestModel.find({},
               {name: 1, age: 1, _id: 0 },
               {sort:{age: 1 }},
               function(err,docs){
                     console.log(docs);
                }
);
```


## 属性方法
&emsp;&emsp;每一个文档都有一个主键“_id”，这个键在文档所属的集合中是唯一的。这个主键名称是固定的，默认是ObjectId类型，该类型的值由系统自己生成。
&emsp;&emsp;ObjectId是一个12字节的 BSON 类型字符串。按照字节顺序，依次代表：4字节：UNIX时间戳；3字节：表示运行MongoDB的机器；2字节：表示生成此_id的进程；3字节：由一个随机数开始的计数器生成的值
### 为Schema添加属性值

```
var mongoose = require('mongoose');
var TempSchema = new mongoose.Schema;
TempSchema.add({
     name: 'String', 
     email: 'String', 
     age: 'Number' 
});
```

### Schema实例方法
&emsp;&emsp;为后面的Model和Entity提供公共的方法。
```
var mongoose = require('mongoose');
var saySchema = new mongoose.Schema({name : String});
saySchema.method('say', function () {
     console.log('hello');
})
var say = mongoose.model('say', saySchema);
var h = new say();
h.say();  //hello
```

### Schema静态方法
```
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestSchema = new mongoose.Schema({
    name : { type:String },
    age  : { type:Number, default:0 },
    email: { type:String, default:"" },
    time : { type:Date, default:Date.now }
});
TestSchema.static('findByName', function (name, callback) {
    return this.find({ name: name }, callback);
});
var TestModel = db.model("test1", TestSchema );
//查询所有名字叫tom的文档结果集
TestModel.findByName('tom', function (err, docs) {
    console.log(docs);
});

```

### Schema追加方法
&emsp;&emsp;有时因为场景的需要，需要为Schema模型追加方法。
```
//为Schema模型追加speak方法
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestSchema = new mongoose.Schema({
    name : { type:String },
    age  : { type:Number, default:0 },
    email: { type:String, default:"" },
    time : { type:Date, default:Date.now }
});
TestSchema.methods.speak = function(){
    console.log('我的名字叫'+this.name);
}
var TestModel = db.model('test1',TestSchema);
var TestEntity = new TestModel({name:'Lenka'}); 
TestEntity.speak();   //我的名字叫Lenka
```