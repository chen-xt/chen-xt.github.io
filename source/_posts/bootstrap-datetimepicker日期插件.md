---
title: bootstrap-datetimepicker日期插件
date: 2017-07-18 22:44:55
tags: datetimepicker
categories: bootstrap
---
## 简介
&emsp;&emsp;Bootstrap下的datetimepicker插件是一款日期选择器插件，使用起来方便而且界面也是很好看的。

## 属性
### format
&emsp;&emsp;时间格式，默认值为`mm/dd/yyyy`。有几种格式：
1. yyyy-mm-dd
2. yyyy-mm-dd hh:ii
3. yyyy-mm-ddThh:ii
4. yyyy-mm-dd hh:ii:ss
5. yyyy-mm-ddThh:ii:ssZ
```
format: 'yyyy-mm-dd hh:ii' //时间显示到分钟
```

### weekStart
&emsp;&emsp;默认值为0。一周从哪一天开始0（星期日）到6（星期六）。
```
weekStart:  '1'	//一周从周一开始
```

### startDate
&emsp;&emsp;开始时间。可以选择的最早日期，这个日期之前的日期都无法被选择。
```
startDate: '2017-08-15 14:45' 	//选择的日期最早只能是这个
```

### endDate
&emsp;&emsp;结束时间。可能选择的最新日期，这个日期以后的日期都无法被选择。
```
endDate: '2017-09-08 14:45'	   //选择的日期最新只能是这个
```

### daysOfWeekDisabled
&emsp;&emsp;禁用星期几。
```
daysOfWeekDisabled: '0,6'	//禁用周末，周六、周日的日期无法被选择
```

### autoclose
&emsp;&emsp;默认值为false，不关闭。当选择好一个日期之后是否立即关闭此日期时间选择器。
```
autoclose: 'true'	//选好日期后关闭日期时间选择器
```

### startView
&emsp;&emsp;默认值为2, 首先显示的视图是月份视图。0是小时视图，1是日视图，2是月份视图，3是年视图，4是十年视图。日期时间选择器打开之后首先显示的视图。0小时或小时。
```
startView:  '1'    //日视图
```

### minView
&emsp;&emsp;默认值为0，小时视图。日期时间选择器所能够提供的最精确的时间选择视图。
```
minView: '1'   //最精确是到日视图
```

### maxView
&emsp;&emsp;默认值为4，十年视图。日期时间选择器最高能展示的选择范围视图。
```
maxView: '1'   //最高能展示的是日视图
```

### todayBtn
&emsp;&emsp;默认值为false。如果此值为true 或 "linked"，则在日期时间选择器组件的底部显示一个 `Today` 按钮用以选择当前日期。如果是true的话，`Today`按钮仅仅将视图转到当天的日期，如果是`linked`，当天日期将会被选中。
```
todayBtn: 'true'
```

### keyboardNavigation
&emsp;&emsp;默认值为true。是否允许通过方向键改变日期。
```
keyboardNavigation: 'false'  //无法通过方向键改变日期
```

### minuteStep
&emsp;&emsp;默认值为5。小时视图中的可以选择的分钟数间隔为5分钟。
```
minuteStep: '10'
```

### showMeridian
&emsp;&emsp;默认值为false。用于小时视图和日视图，为true视图会显示上下午（AM，PM）。
```
showMeridian: 'true'
```

## 方法
&emsp;&emsp;.datetimepicker(options)：初始化日期时间选择器。options的值有以下几个值可以选择。

### remove
&emsp;&emsp;无参数，移除日期时间选择器。同时移除已经绑定的event、内部绑定的对象和HTML元素。
```
$(‘.form_datetime’).datetimepicker('remove');  //视图无法显示。时间无法选择
```

### show
&emsp;&emsp;无参数，显示日期时间选择器。
```
$(‘.form_datetime’).datetimepicker('show');  //页面一加载，视图就已经显示
```
     
### hide
&emsp;&emsp;无参数，隐藏日期时间选择器。 
```
$(‘.form_datetime’).datetimepicker('hide');  //页面加载完成时，视图是隐藏的
```

### update
&emsp;&emsp;无参数，使用当前输入框中的值更新日期时间选择器。  
```
$(‘.form_datetime’).datetimepicker('update'); 
```

### setStartDate
&emsp;&emsp;startDate (String)。给日期时间选择器设置一个新的起始日期。
```
$(".form_datetime").datetimepicker('setStartDate', '2012-01-01')

```

### setEndDate
&emsp;&emsp;endDate (String)。给日期时间选择器设置结束日期。
```
$(".form_datetime").datetimepicker('setEndDate', '2012-01-01')
```

### setDaysOfWeekDisabled
&emsp;&emsp;daysOfWeekDisabled (String|Array)。设置禁用日期，无法被选择的日期。
```
$(".form_datetime").datetimepicker('setDaysOfWeekDisabled', [0,6]);         
```
        
## 事件
### show
&emsp;&emsp;当选择器显示时被触发。

### hide
&emsp;&emsp;当选择器隐藏时被触发。

### changeDate
&emsp;&emsp;当日期被改变时被触发。
```
$('#date-end')
 .datetimepicker()
   .on('changeDate', function(ev){
    if (ev.date.valueOf() < date-start-display.valueOf()){
        ....
    }    
});
```

### changeYear
&emsp;&emsp;当十年视图上的年视图被改变时触发。

### changeMonth
&emsp;&emsp;当年视图上的月视图被改变时触发。

### outOfRange
&emsp;&emsp;当用户选择的日期超出`startDate`或`endDate`时，或者通过`setDate` 或`setUTCDate`方法设置日期超出范围时被触发。


## 键盘支持
### up/down/left/right方向键
&emsp;&emsp;`left/right`向后/向前 一天，`up/down`向后/向前一周。配合shift键，`up/left`向后退一个月，`down/right`向前进一个月。配置ctrl键，`up/left`向后退一年，`down/right` 向前进一年。`Shift+ctrl`和`ctrl`同等效果，即不能同时改变月和年，只能单独改变年。

### escape键
&emsp;&emsp;可以用来隐藏、重新显示日期时间选择器。

### enter键
&emsp;&emsp;当选择器处于显示状态时，enter键只是将其隐藏。当选择器处于隐藏状态时，enter键发挥通常的功能 - 提交当前表单，或者其他。


## 使用
- 绑定输入框，并设置format选项
```
<input type="text" value="2012-05-15 21:05" class="form_datetime">

$('.form_datetime').datetimepicker({
    format: 'yyyy-mm-dd hh:ii'
});
```

- 绑定输入框，并设置format标记
```
<input type="text" value="2012-05-15 21:05" class="form_datetime" data-date-format="yyyy-mm-dd hh:ii">

$('.form_datetime').datetimepicker();
```

- 作为组件使用
```
<div class="input-append date" class="form_datetime" data-date="12-02-2012" data-date-format="dd-mm-yyyy">
    <input size="16" type="text" value="12-02-2012" readonly>
    <span class="add-on"><i class="icon-th"></i></span>
</div>

$('.form_datetime').datetimepicker();
```

- 作为内联日期时间选择器
```
<div class="form_datetime"></div>

$('.form_datetime').datetimepicker();
```

## 参考资料
- [参考网址](http://www.bootcss.com/p/bootstrap-datetimepicker/index.htm)