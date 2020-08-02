## 安装

### 直接 CDN 引入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 或者 -->
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### [下载](https://cn.vuejs.org/v2/guide/installation.html) 及 引入

```shell
开发环境：https://vuejs.org/js/vue.js
生产环境：https://vuejs.org/js/vue.min.js
```

### NPM 安装

```shell
# 最新稳定版
$ npm install vue
```

### webstrom 模板设置

settings 	-> 	editor -> 	live templates -> 	vue -> 	+ ->

```html
<div id ="app">
    {{msg}}
</div>

<script src="../js/vue.js"></script>
<script>
    var app = new Vue({
        el:'#app',
        data:{
            msg:"hello vuejs!",
        },
    })
</script>
```

设置在哪些脚本中使用 	-> 	define -> 	html, javascript and typescript

设置快捷键	->	expand with Tab

## 生命周期

![vue_life_cycle.png](https://i.loli.net/2020/08/01/NMWF7ar4qKeDAOG.png)



## 基本语法

### mustache

即{{ msg + "_" + msg }} ，花括号可以写简单的表达式

### v-once

只赋值一次

```html
<h1 v-once>{{msg}}</h1>
```

### v-html

将string按html的方式渲染出来

### v-text

 不灵活，会覆盖掉尖括号内的字符串，了解即可

```html
<h2 v-test="msg"></h2>
...
<script>
	const app = new Vue({
		el:'#app',
		data:{
			msg:'this is a msg.'
		}
	})
```

### v-pre

把内容直接显示出来，不做任何解析

```html
<h2 v-pre>{{msg}}</h2>
```

### v-cloak

在某些情况下，浏览器可能会直接显示未编译的 mustache 标签。可以用它结合css延迟渲染。

不常用，了解即可

```html
...
<style>
	[v-cloak]{
		display: none;
	}
</style>
...
<div id="app" v-cloak>{{msg}}</div>
...
<script>
...
	setTimeout(function(){
		const app = new Vue({
			el: '#app',
			data:{
				msg: 'this is a msg.'
			},
		})
	}, 1000)
...
```

### v-bind

动态绑定元素的属性，简写 ‘:’

```html
...
<div id="app">
<!-- 这里不可以使用 mustache 语法 -->
<!-- <img src="{{your_url}}" alt=""> -->
<img v-bind:src="your_url" alt="">
<!-- 或者这样写 -->
<img :src="your_url" alt="">
...
	data:{
		your_url: 'www.baidu.com'
	}
...
```

### v-on

事件监听，简写 '@'

```html
<button v-on:click="do">button</button>
<!-- or -->
<button @click="do">button</button>
```

.stop								 解决点击穿透的问题，调用 event.stopProgation()

```html
<div @click="dicClick">
	<button @click.stop="btnClick">button</button>
</div>
```

.prevent							阻止默认事件的调用，调用 event.preventDefault() 

```
<form action="sm">
	<input type="submit" value="提交" @click="commit">
```

.{keyCode | keyAlias}	只从特定键触发时回调

```html
<!-- 所有按键弹起的时候触发 -->
<input type="text" @keyup="keyUp"> 
<!-- 回车按键弹起的时候触发 -->
<input type="text" @keyup.enter="keyUp"> 
...
methods:{
	keyUp(){
		console.log('keyUp');
	}
}
```

.native							  监听组件根元素的原生事件

.once 							   只触发一次

```html
<div @click="dicClick">
	<button @click.once="canClickOnce">button</button>
</div>
```

