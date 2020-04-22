---
title: 初识Vue.js
date: 2017-03-04 09:55:52
tags: Vue
categories: Vue.js
---
## Vue.js是什么？
&emsp;&emsp;**Vue.js**是一套构建用户界面的渐进式框架。**Vue.js**的目标是通过尽可能简单的API实现响应的数据绑定和组合的视图组件。**Vue.js** 的核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统。

## 从“Hello, world!”例子开启Vue之旅
```
<div id="example">
	 {{ message }} 
</div>

<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script type="text/javascript">
    //通过构造函数 Vue 创建一个 Vue 的根实例
	  var example = new Vue({
		    el: '#example',
		    data:{
		        message:'hello world!'
    }
})
</script>
```

## Vue常用指令
### v-model
&emsp;&emsp;在表单控件或者组件上创建双向绑定。
```
	<div id="example">
		<select v-model="selected">
			<option>A</option>
			<option>B</option>
			<option>C</option>
		</select>
		<span>Selected: {{ selected }}</span>
	</div>

	var example = new Vue({
		el: '#example',
		data: {
			selected: ''
		}
	})
```

### v-if
&emsp;&emsp;根据其后表达式的`bool值`进行判断是否渲染该元素。常与`v-else-if/v-else`一起使用。
```
<div id="example">
	<h1 v-if="score === '100'">优秀</h1>
	<h1 v-else-if="score === '100'">良好</h1>
	<h1 v-else="score === '100'">及格</h1>
</div>

var example = new Vue({
    el: '#example',
    data:{
        score:'100'
}
})
```

### v-show
&emsp;&emsp;跟v-if的用法大致一样，不同的是有 `v-show` 的元素会始终渲染并保持在 DOM 中。`v-show` 是简单的切换元素的 CSS 属性 display。
```
<div id="example">
	<h1 v-show="ok">Hello!</h1>
</div>

var app = new Vue({
	el: '#example',
	data: {
		ok: true
	}
})
```

### v-bind
&emsp;&emsp;用于响应地更新 HTML 特性。可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性，如style，class等。
```
<div id="example" v-bind:style="styleObject">
	哈哈哈
</div>

var example = new Vue({
	el: '#example',
	data: {
		styleObject:{
			color: 'blue',
			fontSize: '10px',
			background: 'yellow'
		}
	}
})
```
&emsp;&emsp;注意，v-bind可以缩写成`：` 。
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

### v-on
&emsp;&emsp;用于监听指定元素的DOM事件，如点击事件。
```
<div id="example">
  <button v-on:click="greet">greet</button>
</div>

var example = new Vue({
	el: '#example',
	methods:{
		greet: function(event){
			alert('hello vue.js!');
		}
	}
})
```
&emsp;&emsp;注意，v-on可以缩写成`@` 。
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

### v-for
&emsp;&emsp;根据一组数组的选项列表进行渲染。`v-for` 指令需要以 `item in items`形式的特殊语法，其中`items` 是源数据数组，并且 `item` 是数组元素迭代的别名。
```
<ul id="example">
  	<li v-for="item in items">
  		{{item.message}}
  	</li>
</ul>

var example = new Vue({
	el: '#example',
	data: {
		items: [
			{
				message: '红楼梦'
			},
			{
				message: '水浒传'
			},
			{
				message: '西游记'
			},
			{
				message: '三国演义'
			}
		]
	}
})
```
&emsp;&emsp;`v-for` 还支持一个可选的第二个参数为当前项的索引。
```
<div id="example">
	<ul>
		<li v-for="(item, index) in items">
			{{ index }} - {{ item.message }}
		</li>
	</ul>
</div>

var example = new Vue({
		el: '#example',
		data: {
		items: [
			{ message: '陈' },
			{ message: '婷' }
		]
	  }
})
```

## 计算属性
&emsp;&emsp;模板中的表达式一般只用于简单的操作。如果在模板中放入太多的逻辑会让模板过重且难以维护。这就是为什么 `Vue.js` 将绑定表达式限制为一个表达式，所以如果需要多于一个表达式的逻辑，应当使用计算属性。
### 基本例子
```
<div id="example">
	{{ fullName }}
</div>
var example = new Vue({
	  el: '#example',
	  data: {
	    firstName: 'Foo',
	    lastName: 'Bar'
	  },
	computed: {
	    fullName: function () {
	      return this.firstName + ' ' + this.lastName
	    }
	  }
	})
```

### 加入setter
&emsp;&emsp;计算属性默认只有 `getter` ，可以根据需要提供一个 `setter`。如下例子，在控制台调用`vm.fullName = 'John Doe'`时，`setter` 就会被调用。`vm.firstName`和`vm.lastName`也会有相应更新。
```
var example = new Vue({
	el: '#example',
	data: {
		firstName: '陈',
		lastName: '婷'
	},
	computed: {
		fullName: {
		// getter
		get: function(){
			return this.firstName + ' ' + this.lastName
		},
		// setter
		set: function(newValue){
			var names = newValue.split(' ')
			this.firstName = names[0]
			this.lastName = names[names.length - 1]
		}
	}
  }
})
```

### 计算属性 vs  method
&emsp;&emsp;**计算属性**与 **method** 的区别在于，计算属性根据它的依赖被缓存，即如果 `message` 没有被修改，下次 `get` 不会进行重复计算，而 `method` 则每次调用都会重新计算。这也意味着如 `Date.now()` 这样返回的计算属性会永远得不到更新。

## 组件
&emsp;&emsp;组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素;通过`Vue.component()`接口将大型应用拆分为各个组件，从而使代码更好具有维护性、复用性以及可读性。

### 注册
&emsp;&emsp;要注册一个组件，使用` Vue.component(tagName, options)`形式。组件在注册之后，便可以在父实例的模块中以自定义元素 `<my-component></my-component>` 的形式使用。
```
<div id="example">
	<my-component></my-component>
</div>

//注册
Vue.component('my-component',{
	template: '<div>hello world!</div>'
})
//创建根实例
var example = new Vue({
	el: '#example'
})
```

### 组件中的data必须是函数
```
// Vue 会在控制台发出警告，告诉你在组件中 data 必须是一个函数
Vue.component('my-component',{
	template: '<div>{{ message }}</div>',
	data: {
		message:'hello world!'
	}
})
//data为一个函数，程序正常执行
Vue.component('my-component',{
	template: '<div>{{ message }}</div>',
	data: function(){
		return {
			message: 'hello world!'
		}
	}
})
```

### Prop
#### 父组件向子组件传递数据
&emsp;&emsp;每个组件都是相互隔离的，所以无法在子组件的` template `中引用父组件的数据。数据只能通过 `prop `传递。`prop` 是父组件用来传递数据的一个自定义属性。子组件需要显式地用 `props `选项声明 `“prop”`。
```
<div id="example">
<!-- 传递props -->
	<child message="chen-xt"></child>
</div>

Vue.component('child',{
//声明props
		props: ['message'],
		template: '<div>{{ message }}</div>'
	})
	var example = new Vue({
		el: '#example'
	})
```
#### 子组件向父组件传递数据
&emsp;&emsp;子组件要把数据传递回去可以使用自定义事件。使用 $on(eventName) 监听事件，使用 $emit(eventName) 触发事件。
```
//子组件通过$emit触发父组件的事件，$emit后面的参数是向父组件传参
<div id="example">
	<p>{{ total }}</p>
	<button-counter v-on:increment="incrementTotal"></button-counter>
	<button-counter v-on:increment="incrementTotal"></button-counter>
</div>

Vue.component('button-counter',{
		template: '<button v-on:click="increment">{{ counter }}</button>',
		data: function(){
		return{
			counter: 0
		}
	},
	methods: {
		increment: function(){
			this.counter += 1
			this.$emit('increment')
		}
	},
})
new Vue({
	el: '#example',
	data:{
		total: 0
	},
	methods:{
		incrementTotal:function(){
			this.total += 1
		}
	}
})
```

### 使用Slots分发内容
&emsp;&emsp;内容分发指的是混合父组件的内容与子组件自己的模板。
#### 单个slot
```
<div>
  <h2>我是子组件的标题</h2>
  <slot>
    只有在没有要分发的内容时才会显示。
  </slot>
</div>
<div>
  <h1>我是父组件的标题</h1>
  <my-component>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </my-component>
</div>

<!-- 渲染结果：-->
<div>
  <h1>我是父组件的标题</h1>
  <div>
    <h2>我是子组件的标题</h2>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </div>
</div>
```

#### 具名slot
```
<div class="container">
  <header>
      <slot name="header"></slot>
  </header>
  <main>
      <slot></slot>
  </main>
  <footer>
      <slot name="footer"></slot>
  </footer>
</div>

<app-layout>
    <h1 slot="header">这里可能是一个页面标题</h1>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
    <p slot="footer">这里有一些联系信息</p>
</app-layout>
<!-- 渲染结果：-->
<div class="container">
  <header>
      <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
      <p>主要内容的一个段落。</p>
      <p>另一个主要段落。</p>
  </main>
  <footer>
      <p>这里有一些联系信息</p>
  </footer>
</div>
```

## 事件处理器
### 简单监听事件
&emsp;&emsp;用 v-on 指令监听 DOM 事件来触发一些 JavaScript 代码。
```
<div id="example">
	<button v-on:click="counter += 1">减少 1</button>
	<p>这个按钮被点击了 {{ counter-10 }} 次。</p>
</div>

var example = new Vue({
    el: '#example',
    data: {
       counter: 10
   }
})
```

### 方法事件处理器
&emsp;&emsp;当事件处理的逻辑很复杂的时候，可以将`v-on` 指令绑定到一个方法上。
```
<div id="example">
     <!-- `show` 是在下面定义的方法名 -->
	 <button v-on:click="show">show</button>
</div>

 var example = new Vue({
	  el: '#example',
	  data: {
	    name: 'chen-xt',
	    message: ' '
	  },
	  // 在 `methods` 对象中定义方法
	  methods: {
	    show: function (event) {
	      // `this` 在方法里指当前 Vue 实例
	      alert('Hello ' + this.name + '!')
	      
	    }
	  }
	})
```

### 内联处理器
&emsp;&emsp;当事件处理的逻辑很复杂的时候，除了将`v-on`指令绑定到一个方法上，还可以使用内联 JavaScript 语句。
```
<div id="example">
	  <button v-on:click="show('hi')">Say hi</button>
	  <button v-on:click="show('hello')">Say hello</button>
</div>

var example =new Vue({
	  el: '#example',
	  methods: {
	    show: function (message) {
	      alert(message)
	    }
	  }
	})
```
&emsp;&emsp;需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 `$event` 把它传入方法。
```
<div id="example">
	 <button v-on:click="warn('Form cannot be submitted yet.', $event)">
		 Submit
	 </button>
</div>

var example =new Vue({
   el: '#example',
   methods: {
   warn: function (message, event) {
      // 访问原生事件对象
      if (event) event.preventDefault()
      alert(message)
   }
 }
})
```

### 事件修饰符
&emsp;&emsp;`Vue.js` 为` v-on` 提供了事件修饰符来处理 DOM 事件细节，。通过由点(.)表示的指令后缀来调用修饰符。
- .stop
- .prevent
- .capture
- .self
- .once
```
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
<!-- click 事件至少触发一次 -->
<a v-on:click.once="doThis"></a>
```

### 按键修饰符
&emsp;&emsp;`Vue` 允许为` v-on `在监听键盘事件时添加按键修饰符。
```
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```
&emsp;&emsp;想记住所有的 keyCode 是很有难度的，因此Vue 为最常用的按键提供了别名，如下：
- .tab
- .delete (捕获 "删除" 和 "退格" 键)
- .esc
- .space
- .up
- .down
- .left
- .right
- .ctrl
- .alt
- .shift
- .meta
```
<!-- Alt + C -->
<input @keyup.alt.67="clear">
```

## 过渡效果
### 过渡的CSS类名
1. **v-enter**: &emsp;定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
2. **v-enter-active**:&emsp; 定义进入过渡的结束状态。在元素被插入时生效，在 `transition/animation` 完成之后移除。
3. **v-leave**: &emsp;定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
4. **v-leave-active**:&emsp; 定义离开过渡的结束状态。在离开过渡被触发时生效，在 `transition/animation` 完成之后移除。

### 使用
&emsp;&emsp;对于这些在 `enter/leave `过渡中切换的类名，`v-` 是这些类名的前缀。使用` <transition name="my-transition"> `可以重置前缀，比如 `v-enter` 替换为 `my-transition-enter`。
```
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-active {
  transform: translateX(10px);
  opacity: 0;
}

<div id="example">
  <button @click="show = !show">
      Toggle 
  </button>
  <transition name="slide-fade">
      <p v-if="show">hello</p>
  </transition>
</div>

<script type="text/javascript">
	var example = new Vue({
		el: '#example',
		data: {
		show: true
	   }
	})
```

## 路由
&emsp;&emsp; Vue.js 路由允许我们通过不同的 URL 访问不同的内容。Vue.js 路由需要载入 vue-router 库。
&emsp;&emsp; 下面是一个使用 Vue.js + vue-router 实现简单单页应用的例子：
```
<div id="app">
  <h1>Hello 路由!</h1>
  <p>
	    <!-- 使用 router-link 组件来导航. -->
	    <!-- 通过传入 `to` 属性指定链接. -->
	    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
	    <router-link to="/foo">Go to Foo</router-link>
	    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口：路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>

//  定义（路由）组件。
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
 
// 定义路由：每个路由应该映射一个组件。
const routes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
]
 
//创建 router 实例，然后传 `routes` 配置
const router = new VueRouter({
   routes // （缩写）相当于 routes: routes
})
 
// 创建和挂载根实例：要通过 router 配置参数注入路由，从而让整个应用都有路由功能
const app = new Vue({
     router
}).$mount('#app')
```

## 响应式原理
### 如何追踪变化
&emsp;&emsp;把一个普通 Javascript 对象传给 Vue 实例的 `data` 选项，Vue 将遍历此对象所有的属性，并使用 `Object.defineProperty`把这些属性全部转为 `getter/setter`。

### 变化检测问题
&emsp;&emsp; Vue 不能检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 `getter/setter` 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的。所以可以使用 `Vue.set(object, key, value)` 方法将响应属性添加到嵌套的对象上。
```
var vm = new Vue({
  data:{
  a:1
  }
})
// vm.a 是响应的
vm.b = 2
// vm.b 是非响应的
```

### 声明响应式属性
&emsp;&emsp; 由于 Vue 不允许动态添加根级响应式属性，所以你必须在初始化实例前声明根级响应式属性，即使是一个空值。
```
var vm = new Vue({
  data: {
    // 声明 message 为一个空值字符串
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// 之后设置 message 
vm.message = 'Hello!'
```

### 异步更新队列
&emsp;&emsp;Vue.js 默认异步执行DOM更新 。每当观察到数据变化时，Vue 就开始一个队列，将同一事件循环内所有的数据变化缓存起来。如果一个 `watcher` 被多次触发，只会推入一次到队列中。等到下一次事件循环，Vue 将清空队列，只进行必要的 DOM 更新。
&emsp;&emsp;注意，当设置 `vm.someData = 'new value'` 时，该组件不会立即重新渲染。为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即使用 `Vue.nextTick(callback)` 。这样回调函数在 DOM 更新完成后就会调用。
```
<div id="example">{{message}}</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```
## 参考资料
- [Vue.js官方教程](http://cn.vuejs.org/v2/guide/)
- [Vue.js API](http://cn.vuejs.org/v2/api/)
