---
title: Nodejs之Url模块&QueryString模块
date: 2016-09-13 09:38:41
tags: URL模块&queryString模块
categories: Node.js
---
## Url模块
### url.parse(urlStr, [parseQueryString], [slashesDenoteHost])
将URL字符串转换为对象。有3个参数：
* urlStr： url字符串。
* parseQueryString：可选。设置为true时，会使用querystring模块来解析URL中德查询字符串部分，默认为 false。
* slashesDenoteHost：可选。默认为false，//foo/bar 形式的字符串将被解释成 { pathname: ‘//foo/bar' }；
如果设置成true，//foo/bar 形式的字符串将被解释成  { host: ‘foo', pathname: ‘/bar' }。
``` html
url.parse('http://www.jb51.net/article/61637.htm')
//结果
url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.jb51.net',
  port: null,
  hostname: 'www.jb51.net',
  hash: null,
  search: null,
  query: null,
  pathname: '/article/61637.htm',
  path: '/article/61637.htm',
  href: 'http://www.jb51.net/article/61637.htm' }
```
### url.format(urlObj)
将对象格式化为URL字符串，与url.parse()方法刚好相反。只有1个参数——urlObj，表示 URL对象。
下面代码会将一串对象格式化为URL字符串：
``` html
url.format({protocol: 'http:',slashes: true,auth: null,host: 'www.jb51.net',port: null,hostname: 'www.jb51.net',hash: null,search: null,query: null, pathname: '/article/61637.htm',path: '/article/61637.htm',href:'http://www.jb51.net/article/61637.htm'})
//结果
'http://www.jb51.net/article/61637.htm'
```
### url.resolve(from, to)
为URL或 href 插入或替换原有的标签。有2个参数:
* from：源地址。
* to：需要添加或替换的标签。
``` html
url.resolve('/one/two','three')
//结果
'/one/three'

url.resolve('http://example.com','two')
//结果
'http://example.com/two'

url.resolve('http://example.com/two','three')
//结果
'http://example.com/three'
```

## QueryString模块
### querystring.stringify(obj, [sep], [eq])
将对象转换成字符串，字符串里多个参数将用 ‘&' 分隔，将用 ‘=' 赋值。有3个参数：
* obj： 欲转换的对象。
* sep：可选。设置分隔符，默认为 ‘&'。
* eq：可选。设置赋值符，默认为 ‘='。
``` html
querystring.stringify({name:'rose',sex:'girl'})
//结果
'name=rose&sex=girl'

querystring.stringify({name:'rose',sex:'girl'},';',':')//用分号分隔，用冒 号赋值
//结果
'name:rose;sex:girl'
```
### querystring.parse(str, [sep], [eq], [options])
与querystring.stringify()刚好相反，将字符串转换成对象。有4个参数：
* str：欲转换的字符串。
* sep：可选。设置分隔符，默认为 ‘&'。
* eq：可选。设置赋值符，默认为 ‘='。
* options：maxKeys可接受字符串的最大长度，默认为1000，如果将其设置为0则表示没这个限制。
``` html
querystring.parse('name=rose&sex=girl')
//结果
{ name: 'rose', sex: 'girl' }
```
### querystring.escape()
转义。将传入的字符串进行编码返回相应的编码串。
``` html
querystring.escape('<哈哈>')
//结果
'%3C%E5%93%88%E5%93%88%3E'
```
### querystring.unescape()
反转义。将传入的编码字符串反转回原始字符串。
``` html
querystring.unescape('%3C%E5%93%88%E5%93%88%3E')
//结果
'<哈哈>'
```
