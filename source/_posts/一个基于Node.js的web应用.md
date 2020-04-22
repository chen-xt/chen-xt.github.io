---
title: 一个基于Node.js的web 应用
date: 2016-09-08 13:12:51
tags: Node.js
categories: Node.js
---
## 前言
Node.js是一个JavaScript运行环境，允许在后端（脱离浏览器环境）运行JavaScript代码。那么在后台中又是如何运行JavaScript代码的呢？Node.js使用了 Google 的V8 虚拟机（Google 的Chrome 浏览器使用的JavaScript 执行环境）来解释和执行 JavaScript代码。
初识Node.js，通过完成一个完整的基于 Node.js 的 web 应用来让自己对Node.js有个初步的认识，下面就是完成该应用的具体过程。

## 应用目标
1. 用户可以通过浏览器使用我们的应用。

2. 当用户请求 http://domain/start 时，可以看到一个欢迎页面，
页面上有一个文本框和提交按钮。

3. 用户可以在文本框输入相应的内容并提交表单，随后文本框的内容将被上传到
http://domain/upload， 该页面完成上传后会把文本框内容显示在页面上。

## 应用不同模块分析
1. 需要一个 **HTTP 服务器**，用于提供 Web 页面。

2. 需要一个 **路由**，用于把请求对应到请求处理程序（对于不同的请求，服务器需要给予不同的响应）。

3. 需要一个最终的 **请求处理程序**，用于处理被服务器接收并通过路由传递之后的请求。

4. 需要 **请求数据处理功能**。路由应该能处理 POST 数据，并且把数据封装成更友好的格式传递给请求处理入程序。

5. 需要一些 **视图逻辑**供请求处理程序使用，用于将内容发送给用户的浏览器，显示给用户。

## 构建应用的模块
### 构建HTTP 服务器
创建一个叫server.js 的文件，并写入以下代码：
``` javascript
var http=require("http");
//请求Node.js 自带的 http 模块，并且把它赋值给 http 变量

http.createServer(function (request,response){
//createServer为http 模块提供的函数
    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
 }).listen(8888); 
// listen方法里的数值参数，指定这个 HTTP 服务器监听的端口号(这里为8888)
```
输入node.js命令“node server.js”执行文件,打开浏览器http://localhost:8888/， 可以看到一个写着“Hello World”的网页。
#### 函数传递是如何让 HTTP 服务器工作的
通过函数传递的方法让HTTP 服务器工作，我们可以将上述的代码修改成如下的，当然二者的实现结果是相同的。
``` javascript
var http=require("http");
   function onRequest(request,response){
    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
}
http.createServer(onRequest).listen(8888);
```
将修改后的代码与之前未修改的代码进行比较，可以发现修改前我们向 createServer 函数传递了的是一个匿名函数；而经过修改后，我们首先定义一个“onRequest”函数，然后再将该函数以参数的形式传递给createServer 函数。
#### 服务器处理请求
函数 onRequest() 里面的内容就是服务器处理请求的内容。当onRequest() 函数被触发的时候，有两个参数被传入：request 和response。这2个都是对象，通过使用它们的方法可以处理 HTTP 请求的细节，并且响应请求。
``` javascript
function onRequest(request,response){
    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
}
```
上述代码表示，当收到请求时，使用 response.writeHead() 函数发送一个 HTTP 状态 200 和 HTTP 头的内容类型；使用 response.write() 函数在 HTTP 相应主体中发送文本 “HelloWorld"；最后调用 response.end() 完成响应。
#### 调用HTTP 服务器模块
通常我们会有一个叫 index.js 的文件去调用应用的其他模块来引导和启动应用。所以要让server.js能被index.js 主文件(还没创建)使用，应该先把 server.js 变成一个真正的 Node.js 模块。把某段代码变成模块只要将希望提供其功能的部分导出到请求这个模块的脚本即可。

 （1） 修改server.js的代码，将server.js中的服务器脚本放到一个叫做 start 的函数里，再导出这个函数，代码如下：
``` javascript
var http=require("http");
function start(){
   function onRequest(request,response){
    console.log("Request received.");
    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
   }

http.createServer(onRequest).listen(8888);
console.log("Server has started");
}

exports.start=start;  //导出
```
 
 （2） 创建index.js主文件，然后通过index.js来启动HTTP服务器。index.js主文件的代码如下：
``` javascript
var server=require("./server");
server.start();
```
#### 执行应用
输入node.js命令“node index.js”执行文件，在命令编辑框中会显示“Server has started”；打开浏览器http://localhost:8888/ ，同样可以看到一个写着“Hello World”的网页，此时命令编辑框中会再显示“Request received.”。
### 构建路由模块
编写一个路由模块——即创建一个叫server.js 的文件，并写入以下代码：
``` javascript
function route(pathname){
    console.log("About to route a request for"+pathname);
}
exports.route=route;
```
#### 修改srever.js文件
为了获取浏览器请求的URL路径，需要给 onRequest()函数加上一些逻辑。然后我们就可以通过获取的URL路径区别POST请求和GET请求了。同时，我们也需要将路由函数作为参数传递过去。
``` javascript
var http=require("http");
var url=require("url");

function start(route){ //将路由参数作为参数
  function onRequest(request,response){
    var pathname=url.parse(request.url).pathname;  //获取URL路径
    console.log("Request for"+pathname+"received");

    route(pathname);

    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
}

http.createServer(onRequest).listen(8888);
console.log("Server has started");
}

exports.start=start;
```
#### 修改index.js文件
通过修改index.js，使得路由函数可以被注入到服务器中：
``` javascript
var server=require("./server");
var router=require("./router");

server.start(router.route);
```
#### 执行应用
输入node.js命令“node index.js”执行文件，然后打开浏览器 http://localhost:8888/   ， 同样可以看到一个写着“Hello World”的网页，此时命令编辑框中会显示：
``` javascript
Server has started
Request for/received
About to route a request for/
Request for/favicon.icoreceived
About to route a request for/favicon.ico
```
以上信息表明HTTP 服务器已经在使用路由模块了，并且会将请求的路径传递给路由。

### 构建请求处理程序
针对被服务器接收并通过路由传递过来的不同请求，我们需要有不同的处理方式，因此需要请求处理程序来分别进行处理。创建一个“requestHandlers.js”文件，添加2个请求处理程序，并分别为其添加一个函数，然后再将这些函数作为模块的方法导出。requestHandlers.js中代码如下：
``` javascript
function start(){
    console.log("Request handle 'start' was called.");
}

function upload(){
    console.log("Request handle 'upload' was called.");
}

exports.start=start;
exports.upload=upload;
```
#### 修改index.js文件
将一系列请求处理程序通过对象来传递。先将对象引入到index.js中，对index.js进行修改，如下：
``` javascript
var server=require("./server");
var router=require("./router");
var requestHandlers=require("./requestHandlers");

var handle={}
handle["/"]=requestHandlers.start;  //"/"的请求由start处理程序处理
handle["/start"]=requestHandlers.start;  //"/start"的请求由start处理程序处理
handle["/upload"]=requestHandlers.upload;  //"/upload"的请求由upload处理程序处理

server.start(router.route,handle);
```
#### 修改server.js文件
将对象传递给服务器，对server.js进行修改，如下:
``` javascript
var http=require("http");
var url=require("url");

function start(route,handle){ //将handle 参数传给start()函数
  function onRequest(request,response){
    var pathname=url.parse(request.url).pathname; //获取URL路径
    console.log("Request for"+pathname+"received");

    route(handle,pathname); //将handle 参数传给route()函数

    response.writeHead(200,{"Content-Type":"text/plain"});
    response.write("hello world");
    response.end();
}

http.createServer(onRequest).listen(8888);
console.log("Server has started");
}

exports.start=start;
```
#### 修改route.js文件
修改route.js文件，将handle 参数传给route()函数，修改如下：
``` javascript
function route(handle,pathname){//将handle 参数传给route()函数
    console.log("About to route a request for"+pathname);

    if(typeof handle[pathname]=='function'){
        handle[pathname]();  //从传递的对象中获取请求处理函数
    }
    else{
        console.log("No request handle found for"+pathname);
    }
}
exports.route=route;
```
通过以上代码，我们首先检查给定的路径对应的请求处理程序是否存在，如果存在的话直接调用相应的函数。
#### 执行文件
经过以上的处理，就可以把服务器、 路由和请求处理程序在一起了。启动应用程序并在浏览器中访问  http://localhost:8888/start   ，请求会由start处理程序处理，命令编辑框显示结果如下：
``` javascript
Server has started
Request for/startreceived
About to route a request for/start
Request handle 'start' was called.
Request for/favicon.icoreceived
About to route a request for/favicon.ico
No request handle found for/favicon.ico
```
在浏览器中访问 http://localhost:8888， 请求同样由start处理程序处理，命令编辑框显示结果如下：
``` javascript
Server has started
Request for/received
About to route a request for/
Request handle 'start' was called.
```
在浏览器中访问http://localhost:8888/upload， 请求会由upload处理程序处理，命令编辑框显示结果如下：
``` javascript
Server has started
Request for/uploadreceived
About to route a request for/upload
Request handle 'upload' was called.
```
### 让请求处理程序作出响应
采用将服务器“传递”给内容的方式，就是将 response 对象（从服务器的回调函数 onRequest()获取）通过请求路由传递给请求处理程序。 随后，处理程序就可以采用该对象上的函数来对请求作出响应。
#### 修改server.js文件
不再从 route()函数获取返回值，而是将 response 对象作为第三个参数传递给 route()函数；然后将 onRequest()处理程序中所有有关 response 的函数调用都移除，这部分工作会让route()函数来完成。对server.js进行修改，如下：
``` javascript
var http=require("http");
var url=require("url");

function start(route,handle){ //将handle 参数传给start()函数
  function onRequest(request,response){
    var pathname=url.parse(request.url).pathname; //获取URL路径
    console.log("Request for"+pathname+"received");

    route(handle,pathname,response); 

}

http.createServer(onRequest).listen(8888);
console.log("Server has started");
}

exports.start=start;
```
#### 修改router.js文件
不再从请求处理程序中获取返回值，而是直接传递 response 对象。如果没有对应的请求处理器处理，我们就直接返回“404”错误。对router.js进行修改，如下：
``` javascript
 function route(handle,pathname,response){
    console.log("About to route a request for"+pathname);

    if(typeof handle[pathname]=='function'){
        handle[pathname](response);
    }
    else{
        console.log("No request handle found for"+pathname);
        response.writeHead(404, {"Content-Type": "text/plain"});
        response.write("404 Not found");
        response.end();
     }
  }
 exports.route=route;
```
#### 修改 requestHandler.js文件
处理程序函数需要接收 response 参数，为了对请求作出直接的响应。start 处理程序在 exec()的匿名函数中做请求响应的操作，而upload 处理程序仍然是回复“Hello World”，只是这次是使用response 对象而已。对requestHandler.js进行修改，如下：
``` javascript
var exec = require("child_process").exec;
function start(response){
    console.log("Request handle 'start' was called.");
    exec("ls -lah", function (error, stdout, stderr) {
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write(stdout);
    response.end();
});
}

function upload(response){
    console.log("Request handle 'upload' was called.");
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello Upload");
    response.end();
}

exports.start=start;
exports.upload=upload;
```
#### 执行文件
通过node.js命令执行文件，然后通过浏览器访问页面，可以发现一切正常工作，对于不同的请求，会由对应的处理程序去进行处理。
### 上传内容功能
通过显示一个文本区供用户输入内容，然后通过 POST 请求提交给服务器。最后，服务器接受到请求，通过处理程序将输入的内容展示到浏览器中。
#### 构建一个表单
用/start 请求处理程序生成带文本区的表单，在requestHandlers.js添加一些html代码。对requestHandlers.js 进行修改，如下：
``` 
var exec = require("child_process").exec;

function start(response){
    console.log("Request handle 'start' was called.");
    
var body = '<html>'+
'<head>'+
'<meta http-equiv="Content-Type" content="text/html; '+
'charset=UTF-8" />'+
'</head>'+
'<body>'+
'<form action="/upload" method="post">'+
'<textarea name="text" rows="20" cols="60"></textarea>'+
'<input type="submit" value="Submit text" />'+
'</form>'+
'</body>'+
'</html>';
    
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write(body);
    response.end();

}

function upload(response){
    console.log("Request handle 'upload' was called.");
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello Upload");
    response.end();
   }

exports.start=start;
exports.upload=upload;
```
启动应用，即可看到简单的表单了，提交表单，会触发/upload 请求处理程序处理POST请求。
#### 处理 POST 请求
一个简单的表单已经完成了，接下来就是需要实现当用户提交表单时，触发/upload 请求处理程序处理 POST 请求了。
这里，还需要注意一点，由于对于POST请求，用户可能会输入大量的内容，所以Node.js 会将 POST 数据拆分成很多小的数据块， 然后通过触发特定的事件，将这些小数据块传递给回调函数。当这些事件触发的时候，具体回调哪些函数需要通过在 request 对象上注册监听器（ listener） 来实现。

 （1） 修改server.js文件：
``` javascript
var http = require("http");
var url = require("url");

function start(route, handle) {
    function onRequest(request, response) {
        var postData = "";
        var pathname = url.parse(request.url).pathname;
        console.log("Request for " + pathname + " received.");
        
        request.setEncoding("utf8");
        
        request.addListener("data", function(postDataChunk) {
            postData += postDataChunk;
            console.log("Received POST data chunk '"+
            postDataChunk + "'.");
        });

        request.addListener("end", function() {
            route(handle, pathname, response, postData);
    });
}

    http.createServer(onRequest).listen(8888);
    console.log("Server has started.");
}
    exports.start = start;
```
上述代码完成了三件事情：
①设置了接收数据的编码格式为UTF-8；
②注册了“data”事件的监听器，用于收集每次接收到的新数据块，并将其赋值给 postData 变量；
③我们将请求路由的调用移到 end 事件处理程序中，以确保它只会当所有数据接收完毕后才触发，并且只触发一次。同时还把 POST 数据传递给请求路由，因为这些数据，请求处理程序会用到。
 
 （2） 修改 router.js文件：
要在/upload 页面，展示用户输入的内容，需要将 postData 传递给请求处理程序，因此对router.js 进行修改，如下：
``` javascript
function route(handle,pathname,response,postData){
    console.log("About to route a request for"+pathname);

    if(typeof handle[pathname]=='function'){
        handle[pathname](response,postData);
    }
    else{
        console.log("No request handle found for"+pathname);
        response.writeHead(404, {"Content-Type": "text/plain"});
        response.write("404 Not found");
        response.end();
    }
}
exports.route=route;
```
 
 （3） 修改 requestHandlers.js：
为了实现将数据包含在对 upload 请求的响应中，对 requestHandlers.js进行修改，如下：
``` 
var querystring = require("querystring");

function start(response, postData) {
    console.log("Request handler 'start' was called.");
    var body = '<html>'+
        '<head>'+
        '<meta http-equiv="Content-Type" content="text/html; '+
        'charset=UTF-8" />'+
        '</head>'+
        '<body>'+
        '<form action="/upload" method="post">'+
        '<textarea name="text" rows="20" cols="60"></textarea>'+
        '<input type="submit" value="Submit text" />'+
        '</form>'+
        '</body>'+
        '</html>';
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write(body);
    response.end();
}

function upload(response, postData) {
    console.log("Request handler 'upload' was called.");
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("You've sent the text: "+
    querystring.parse(postData).text);
    response.end();
}

exports.start = start;
exports.upload = upload;
```
到此，一个简单的web应用已经完成了。启动应用，然后通过浏览器访问
http://localhost:8888/start， 在页面的文本框输入文本“hello world!”，点击提交按钮后，请求会被upload处理程序处理,然后在新的浏览器页面显示提交的内容：“You've sent the text: hello world!”。
### 出现的问题
#### 问题
Node.js中的html代码以源码出现，而不能被解析。
#### 解决方法
响应的 Content-Type属性 如果设置成 “text/plain”，表示以 **文本形式** 输出。只有将其设置成“text/html ”，才能让浏览器 **解析** html文档。








