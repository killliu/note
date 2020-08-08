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
</script>
...
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

```html
<form action="sm">
	<input type="submit" value="提交" @click="commit">
</form>
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
...
```

.native							  监听组件根元素的原生事件

.once 							   只触发一次

```html
<div @click="dicClick">
	<button @click.once="canClickOnce">button</button>
</div>
```

### v-if 和 v-show 

```html
<h1 v-if="isShow">{{message}}</h1>
<h1 v-show="isShow">{{message}}</h1>
```

isShow 为 false 时 v-if 的整个元素会从DOM中移除，而 v-show 会被增加一个 style="display: none;" 的属性

频繁变更时使用 v-show，反之使用 v-if

 ### v-for

```html
...
<ul>
    <li v-for="item in names">{{item}}</li>
    <!-- 加下标 -->
    <li v-for="(v, index) in names">{{index + 1}}.{{v}}</li>
</ul>
...
data:{
	names:['a', 'bce', 'df']
}
...
```

遍历对象

```html
...
<ul>
    <!-- 这里的 item 是对象的 value -->
    <li v-for="item in info">{{item}}</li>
    
    <!-- 这里的 v 是对象的 value，k 是对象的 key -->
    <li v-for="(v,k) in info">{{k}}---{{v}}</li>
    
    <!-- 下标 i -->
    <li v-for="(v,k,i)"> index is {{i}}</li>
</ul>
...
info:{
	name:'abc',
	age:18
}
...
```

**<font color="red">注意: </font>** 使用 v-for 时，给元素加上 :key 属性，便于<font color="red">**高效地更新虚拟DOM** </font>

> <li v-for="item in info" key="item">{{item}}</li>

### 过滤器

```html
<h1>{{price | showPriceFilter}}</h1>
...
	filters:{
		showPriceFilter(value){
			return "$" + value.toFixed(2)
		}
	}
...
```

### 循环

```javascript
// 1. 普通的 for 循环
for(let i = 0; i < this.books.length;i++){
	totalPrice += this.books[i].price * this.books[i].count
}
// 2. for (let in this.books)
for (let i in this.books){
    totalPrice += this.books[i].price * this.books[i].count
}
// 3. for (let i of this.books)
let totalPrice = 0
for (let i of this.books){
    totalPrice += i.price * i.count
}
```

### 高阶函数 

#### filter

```javascript
// filter中的回调函数，必须返回一个 boolean 值
// 返回 true 时，函数内部会自动将这次回调的 n 追加到数组中
// 返回 false时，函数内部会自动过滤掉 n  
const num = [10, 20, 111, 123, 66, 65]
let newNums = nums.filter(function(n){
    return n < 100
})
```

#### map

```javascript
// 遍历 newNums ，将其每个元素 * 2 并将返回值追加到新的数组中
let newNums2 = newNums.map(function(n){
	return n*2
})
```

#### reduce

```javascript
// 对数组中的所有内容进行汇总
// 第1次：preValue 0 n 10 
// 第2次：preValue 10 n 20 
// 第3次：preValue 30 n 66 
// 第4次：preValue 96 n 65
// total: 161
let total = newNums2.reduce(function (preValue, n){
    return preValue + n
}, 0)
```

#### 示例

```javascript
let total = num.filter(function(n){
	return n < 100
}).map(function (n){
	return n * 2
}).reduce(function(preValue, n){
	return preValue + n
}, 0)
console.log(total) // 161
// 简写
let total = num.filter(n => n < 100).map(n => n * 2).reduce((pre, n)=> pre + n);
console.log(total) // 161
```

### v-model

双向绑定（ 等价于 v-bind + v-on ）

```html
...
<input type="text" v-model="msg">{{msg}}</input>
...
	data:{
		msg:"this is a message."
	}
...
```

互斥

```html
...
	<label for='male'>
    	<input type="radio" id="male" value="男" v-model="sex">男
	</label>
	<label for='female'>
		<input  type="radio" id="female" value="女" v-model="sex">女
	</label>
	<h1>您选择的性别是：{{sex}}</h1>
...
	data:{
		sex:''
	}
...
```

单选

```html
...
	<label for='agree'>
    	<input type="checkbox" id="agree" v-model="isAgree">协议
	</label>
	<h1>您选择的是：{{isAgree}}</h1>
	<button :disabled="!isAgree">下一步</button>
...
	data:{
		isAgree: false
	}
...
```

多选

```html
...
    <input type="checkbox" value="篮球" v-model="hobbies">篮球
    <input type="checkbox" value="足球" v-model="hobbies">足球
    <input type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
    <input type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
	<h1>您的爱好是：{{isAgree}}</h1>
...
	data:{
		hobbies: []
	}
...
```

进阶

```html
...
	<label v-for="item in originHobbies" :for="item">
        <input type="checkbox" :value="item" :id="item" v-model="Hobbies">{{item}}
	</label>
...
	data:{
		Hobbies:[]
		originHobbies:['篮球', '足球', '台球']
	}
...
```

#### lazy

敲回车或者失去焦点时进行绑定

```html
...
<input type="text" v-model.lazy="msg">
<h1>{{msg}}</h1>
...
	data:{
		msg:'hello'
	}
...
```

#### number

v-model 默认 string 类型

```html
...
<input type="text" v-model.number="age">
<h1>{{age}}-{{typeof age}}</h1>
...
	data:{
		age:0
	}
...
```

#### trim

```html
...
<input type="text" v-model.trim="name">
<h1>--{{name}}--</h1>
...
	data:{
		name:''
	}
...
```

### keep-alive

vue 内置组件

xxx.vue

```html
...
<!-- exclude的内容是正则表达式，逗号左右不能加空格 -->
<keep-alive exclude="your_vue1,your_vue2">
    <router-view/>
</keep-alive>
...
	actived(){},
	deactivated(){}
	// 
	beforeRouteLeave(to, from, next){
		// do something...
		next()
	}
...
```

- include 
- exclude

### 组件

Vue.extend() 创建组件构造器	---> 	Vue.component() 注册组件	--->	在 Vue 实例的作用域内使用

```vue
<div id="app">
    <!-- -------- 3.使用组件 -------- -->
    <cpn></cpn>
</div>

<script>
	<!-- -------- 1.创建组件构造器 ---- -->
    const myComponent = Vue.extend({
        template:`
        <div>
        	<h1>组件标题</h1>
        	<p>组件中的一个段落内容</p>
		</div>`
    });
    
    <!-- -------- 2.注册组件，并定义组件标签的名称 -------- -->
    Vue.component('cpn', myComponent)
    
    let app = new Vue({
        el: '#app'
    })
</script>
```

局部注册

```javascript
...
const app = new Vue({
	el:'#app',
	components:{
		cpn: myComponent
	}
})
...
```

**全局注册语法糖**

```vue
Vue.component('cpn', {
    template:`
    <div>
        <h1>组件标题</h1>
        <p>组件中的一个段落内容</p>
    </div>`
})
```

#### 父子组件

```html
...
	<div>
    	<parent></parent>
    </div>
...
<script>
	const parent = Vue.extend({
        template:`
			<div>
				<h1>i'm parent</h1>
				<p> this is parent contents</p>
				<child></child>
			</div>
		`,
        // 这里注册一个子组件
        componts:{
            child:child
        }
    })
	const child = Vue.extend({
        template:`
			<div>
				<h1>i'm child</h1>
				<p> this is child contents</p>
			</div>
		`
    })
    const app = new Vue({
        el:'#app',
        components:{
            parent: parent
        }
    })
</script>
...
```

#### 父子组件的通信

![vue_component_cnet.png](https://i.loli.net/2020/08/02/cbpQJsAGkRCiWao.png)

父传子 props

```html
...
<div id="app">
    <cpn :cmovies="movies" :cmsg="msg"></cpn>
</div>
<template id="cpn">
	<div>
        <h1>{{cmovies}}</h1>
        <p>{{cmsg}}</p>
    </div>
</template>
...
<script>
	const cpn={
        template:'#cpn',
        props:{
            // cmovies: Array,
            // cmsg: String,
            cmovies: {
                type: Array,
                default(){	// 类型是对象或者数组时，默认值必须是一个函数
                    return []
                } 	
            }
            cmsg: {
            	type: String,
                default: 'this is a default string',
                required: true // 用的时候必须传 cmsg 值
            }
        }
    }
    const app = new Vue({
        el:'#app',
        components:{
            cpn
        }
    })
</script>

```

子传父 $emit

```html
...
<div id ="app">
    <cpn @itemclick="cpnclick"></cpn>
</div>

<template id="cpn">
    <div>
        <button v-for="item in categories" 
                @click="btnClick(item)">
            {{item.id}} : {{item.name}}
        </button>
    </div>
</template>
...
<script>
 const cpn = {
        template:'#cpn',
        data(){
            return {
                categories:[
                    {id:'aaa', name:'热门推荐'},
                    {id:'bbb', name:'手机数码'},
                    {id:'ccc', name:'家具电器'},
                    {id:'ddd', name:'电脑办公'}
                ]
            }
        },
        methods:{
            btnClick(item){
                this.$emit('itemclick', item)
            }
        }
    }

    var app = new Vue({
        el:'#app',
        components:{
            cpn
        },
        methods: {
            cpnclick(item){
                console.log("btn clicked: " + item.name)
            }
        }
    })
</script>
...
```

父子访问

父组件访问子组件：<font color='red'>$children</font> 或 <font color='red'>$refs</font>

子组件访问父组件：<font color='red'>$parent</font>

### 插槽 slot

具名插槽 ------> 使用  **<font size="6" color="red">v-slot</font>**

```html
...
<div id="app">
    <cpn><span slot="center">some change</span></cpn>
</div>
...
<template id="cpn">
    <div>
        <slot name="left">left</slot>
        <slot name="center">left</slot>
        <slot name="right">left</slot>
    </div>
</template>
...
```

