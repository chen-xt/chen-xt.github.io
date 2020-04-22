---
title: layer弹层组件之移动版
date: 2017-08-31 22:19:47
tags: layer
categories: layer
---
## 简介
&emsp;&emsp;layer弹层组件分移动版和PC版，这边主要讲的是移动版的layer，PC版的后续再进行补充。移动版是为移动设备量身定做的弹层UI，并且完全独立于PC版的layer。与PC版的layer不同的是，移动版的layer只提供一个核心调用方法，即`layer.open(options)`，通过对参数`options`进行设置来实现各种不同的弹层。

## 快速上手
&emsp;&emsp;第一步，下载。要去[官网](http://layer.layui.com/mobile/)下载layer的相关文件，主要是`layer.js`文件，`layer.css`文件可以根据需要引入，但是`layer.js`文件是必需的。
&emsp;&emsp;第二步，引入。将layer的相关文件引入项目中，这里需要注意的是，不能去挪动layer里面的文件结构，因为它们是不可拆散的组合，直接整个引入就可以。由于layer是基于jQuery的，在引入`layer.js`文件之前，必须先引入**jQuery1.8**或以上版本。
&emsp;&emsp;第三步，使用。下面是一个简单的layer弹层例子。
```
<!doctype html>
<html>
	<head>
	  <title>开始使用layer</title>
	</head>
	<body>
	  <button id="test">小小提示层</button>

	  <script src="jQuery的路径"></script>
	  <script src="layer.js的路径"></script>
	  <script>
		$('#test').on('click', function(){
		  layer.open({
			type: 1,
			area: ['600px', '360px'],
			shadeClose: true, //点击遮罩关闭
			content: '\<\div style="padding:20px;">自定义内容\<\/div>'
		  });
		});
	  </script>
	</body>
</html> 
```

## 参数
&emsp;&emsp;这里所谈到的参数指的就是核心接口`layer.open(options)`中的**options**。

### type
&emsp;&emsp;type参数用于设置弹层的类型。其类型为`Number`型，默认值为`0`（**0**表示信息框，**1**表示页面层，**2**表示加载层）。
```
	type: 1
```

### content
&emsp;&emsp;content参数用于设置弹层内容。其类型为`String`型，这个参数为必选参数。
``` 
	content:  'layer弹层组件'  
```

### title
&emsp;&emsp;title参数用于设置弹层标题。其类型为`String`型或`Array`型，默认值为`空`，值可以为字符串或者数组，为空则不显示。
``` 
	//格式
		title: '标题'
		title: ['标题', 'background-color: #eee;'] //第二个参数可以自定义标题的样式 

	//举例
		title:  'layer'
		title:  ['layer', 'color: #eee;']  
```

### time
&emsp;&emsp;time参数用于控制自动关闭层所需秒数。其类型为`Number`型，默认不开启，支持整数和浮点数。
``` 
	time:  5  //5秒后关闭  
```

### style
&emsp;&emsp;style参数用于设置自定义层的样式。其类型为`String`型。
``` 
	style:  'border:none; background-color:#78BA32; color:#fff;'  
```

### skin
&emsp;&emsp;skin参数用于设定弹层显示风格。其类型为`String`型，目前支持配置`footer`(即底部对话框风格)、`msg`(普通提示)两种风格。
``` 
	skin: 'footer'  
```

### className
&emsp;&emsp;className参数用于自定义一个css类。其类型为`String`型，用于自定义样式。自定义一个css类后，就可以在css里面通过设置这个类的样式来控制该弹层的风格。
``` 
	className:  'modal'  
```

### btn
&emsp;&emsp;btn参数用于设置按钮。其类型为`String`型或`Array`型，不设置则不显示按钮。如果只需要一个按钮，则`btn: '按钮'`，如果有两个，则`btn: ['按钮一', '按钮二']`。
``` 
	//设置两个按钮
	btn:  ['马上开启','稍后再说' ]  
```

### anim 
&emsp;&emsp;anim参数用于设置动画类型。其类型为`String`型或`Boolean`型，可支持的支持两种动画配置`scale`(默认)和`up`(从下往上弹出)，如果不开启动画，设置`false`即可。
``` 
	anim:  'up'
```

### shade 
&emsp;&emsp;shade参数用于控制遮罩展现。其类型为`String`型或`Boolean`型，默认为`true`，显示遮罩，并且可以自定义遮罩风格。
``` 
	shade:  'background-color: rgba(0,0,0,.6) '  //遮罩的颜色以及透明度
```

### shadeClose
&emsp;&emsp;shadeClose参数用于设置点击遮罩时是否关闭层。其类型为`Boolean`型，默认为`true`，即点击遮罩时关闭层。
``` 
	shadeClose:  false  //点击遮罩时不关闭层  
```

### fixed &&  top
&emsp;&emsp;fixed参数用于设置是否固定层的位置，需要与top参数一起使用。其类型为`Boolean`型，默认为`true`，不固定层的位置。而top参数用于控制层的纵坐标，其类型为`Number`型，默认为`无`，一般情况下不需要设置，因为层会始终垂直水平居中，只有当`fixed: false`时top才有效。
``` 
	fixed : false,
	top: 10
```

### success &&  end
&emsp;&emsp;success参数用于设置层成功弹出层的回调，该回调参数返回一个参数为当前层元素对象。其类型为`Function`型。end参数与success参数用法类似，只是它刚好与success参数相反，是用于设置层彻底销毁后的回调函数。
``` 
	//格式
		success: function(elem){
			console.log(elem);   //弹层成功弹出时候提示弹出	
		}
		end: function(elem){
			console.log(elem);   //弹层销毁时提示销毁
		}
	//举例
		success: function(){
			alert('弹出');   
		},
		end:function(index){
			  alert("销毁"); 
		}
```

### yes &&  no  
&emsp;&emsp;yes参数用于设置点击确定按钮触发的回调函数，返回一个参数为当前层的索引。其类型为`Function`型。no参数与yes参数用法类似，只是它刚好与yes参数相反，用于设置点击取消按钮触发的回调函数。
``` 
	//格式：
		yes: function(index){
		  alert('点击了是')
		  layer.close(index)
		}
		no: function(index){
		  alert('点击了否')
		  layer.close(index)
		}
	//举例
		yes: function(){  //点击确定时候弹出“好的”提示
		    layer.open({
		      content: '好的'
		      ,time: 2
		      ,skin: 'msg'
		    });
		},
		no: function(index){			  
			window.location.href="https://www.baidu.com/"; //点击取消跳转到必应
		}
```

## 其它内置方法/属性 
### layer.v
&emsp;&emsp;layer.v用于返回当前使用的layer mobile版本号。
``` 
	yes: function(index){
	    alert(layer.v);   //弹出“2.0”，当前的版本为2.0
    }
```

### layer.close(index)
&emsp;&emsp;layer.close(index)用于关闭特定层，index为该特定层的索引。
``` 
	yes: function(index){
		layer.close(index);
	}
```

### layer.closeAll()
&emsp;&emsp;layer.closeAll()用于关闭页面所有layer的层。
``` 
	yes: function(index){
	    layer.closeAll();
    }
```

## 实例
1. 信息框
```
	layer.open({
	    content: '我是layer移动版',
	    btn: '知道了'
	});
```

2. 提示
```
	layer.open({
	    content: '你好',
	    skin: 'msg',
	    time: 3 //3秒后自动关闭
	});
```

3. 询问框
```
	layer.open({
	    content: '您确定要刷新一下本页面吗？',
	    btn: ['刷新', '不要'],
	    yes: function(index){
	      location.reload();
	      layer.close(index);
	    }
	});
```

4. 底部对话框
```
	layer.open({
	    content: '这是一个底部弹出的询问提示',
	    btn: ['删除', '取消'],
	    skin: 'footer',
	    yes: function(index){
	      layer.open({content: '执行删除操作'})
	    }
	});
```

5. 页面层
```
	layer.open({
	    type: 1,
	    content: '可传入任何内容，支持html。一般用于手机页面中',
	    anim: 'up',
	    style: 'position:fixed; bottom:0; left:0; width: 100%; height: 200px; padding:10px 0; border:none;'
	});
```

6. loading带文字
```
	layer.open({
	    type: 2,
	    content: '加载中' //如果没有content的话，则只有几个加载的小圆点
	});
```

## 参考地址
- [参考网址](http://layer.layui.com/mobile/api.html)