改变网页链接，但是不刷新页面请求

1. 通过 hash 实现， console ->

> location.hash = 'your_content'

2. 通过 history 实现，console ->

> history.pushState({}, '', 'home')

pushState 类似入栈（先进后出） ，进栈后可以在浏览器上按返回按钮 back到上一个界面

> history.back()

back 类似出栈

> history.forward()

forward 与 back 相反

> history.go(1)

go 跳转到指定序号的 state，go(-1) = back() ，go(1) = forward()

> history.replaceState({}, '', 'about')

replaceState 替换页面，替换后不可 back 到上一个界面

### 安装 及 使用

>  npm install vue-router --save

#### 1. <font color='red'>导入 并 使用</font> 
router/index.js
   ```vue
   import VueRouter from 'vue-router'
   import Vue from 'vue'
   
   Vue.use(VueRouter)
   
   const routes = [
   
   ]
   // 创建 router 实例
   const router = new VueRouter{
   	routes,
   	mode: 'history' // 默认是修改哈希的方式 #，这里改成 html5 的 history 模式
   }
   
   export default router
   ```

#### 2. 创建<font color='red'>实例</font>

#### 3. 挂载
main.js
   ```vue
   import Vue from 'vue'
   import App form './App'
   import router from './router' // 默认自动导入该目录下的 index.js
   
   Vue.config.productionTip = false
   
   new Vue({
   	el:'#app',
   	router,
   	render: h => h(App)
   })
   
   ```

#### 4. 创建路由组件
#### 5. 配置路由映射 
redirect 重定向属性，可以配置默认首页

#### 6. 使用路由 
<router-link> 和 <router-view>

```html
<template>
	<div id="app">
		<router-link to="/home" tag="button" replace>Home</router-link>
        <router-link to="/about">About</router-link>
        <!-- 代码的方式 -->
        <button @click="homeClick">Home</button>
        <button @click="aboutClick">About</button>
        
        <router-view></router-view>
    </div>
</template>
...
<script>
    export default{
        name:'App',
        methods:{
            homeClick(){
                this.$router.push('/home') 
                // 或者 this.$router.replace('/home') 
            },
            aboutClick(){
                this.$router.push('/about')
            }
        }
    }
</script>
```

router-link 的其他属性

- <font color="red">tag</font>: router-link 默认渲染成 a 标签，可以使用 tag 属性修改标签类型

- <font color="red">replace</font>: router-link 的 replace 属性，可以修改为改标签使用 replaceState，即不可 back 或 forward

- <font color="red">active-class</font>: router-link 被点击的标签会自动打上 router-link-active 的 class 属性，这个类名可以在路由表中统一修改，eg : /router/index.js

```vue
const router = new VueRouter({
	routes,
	mode:'history',
	linkActiveClass:'active'
})
```

### 懒加载

1. 结合 Vue 的异步组件和 webpack ： 
    const home = resolve =>{ require.ensure(['../components/home.vue'], ()={ resolve(require('../components/home.vue')) })};

2. AMD
    const home = resolve => require(['../components/home.vue'], resolve)

3. es6

   const home = ()=> import('../components/home.vue')

### 嵌套路由

home.vue  <font color="darkgray">(child: news.vue msg.vue)</font>

```html
...
<div>
   <router-link to="/home/news">新闻</router-link>
   <router-link to="/home/msg">消息</router-link>
   <router-view></router-view>
</div>
...
```

router/index.js

```javascript
const Home = ()=>import('../components/home')
const HomeNews = ()=>import('../components/homenews')
const HomeMsg = ()=>import('../components/homemsg')
...
const routes = [
	{
		path: '/home',
		component: Home，
		children:{
			{
				path: 'news',
				component: HomeNews,
        		meta:{
        			title:"新闻"
    			}
			}，{
				path: 'msg',
				component: HomeMsg,
    			meta:{
        			title:"消息"
    			}
			}
		}
	}
]
...
```

### 路由传参

主要有两种方式：<font color="red">params</font>, <font color="red">query</font>

- params

  配置路由格式：/router/:id

  传递的方式：在 path 后面跟上对应的值

  传递后形成的路径： /router/abc

  使用 eg. $route.params.id

  ```html
  <template>
      <div id="User">
          <h1>
              {{$soute.params.id}}
          </h1>
      </div>
  </template>
  <script>
      export default{
          name: "#User",
          computed:{
              userID(){
                  return this.$route.params.id
              }
          }
      }
  </script>
  ```

- query

  配置路由格式：/router

  传递的方式：对象中使用 query 的 key 作为传递方式

  传递后形成的路径： /router?id=abc
  
  > <router-link :to="{path:'/profile', query:{name}}">profile</router-link>
  
  取数据时使用：

  > $route.query.id

###  全局导航守卫

监听全局跳转 /router/index.js

前置守卫（全局）

```vue
router.beforeEach((to, from, next)=>{
	document.title = to.matched[0].meta.title // 这里需要在 route 中增加 meta -> title 属性
	next()
})
```

后置守卫（全局）

> router.afterEach(to, from)=> { }

- 路由独享守卫
- 组件内守卫

参考链接：[Vue Router 导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)

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