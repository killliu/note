# backend

**ide: goland**

新建项目

### **gin**

> ```shell
> go get -u github.com/gin-gonic/gin
> ```

main.go

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```

<font color="red">error: require 的模块显示红色，即没有将模块导入项目</font>

ctrl + alt + s ---> Go ---> Go Modules ---> Enable Go Modeules integration

将如上选项打上勾 ---> Apply 

### **gorm**

<font color="darkgray">golang 数据库 解决方案</font>

> ```shell
> go get -u github.com/go-sql-driver/mysql
> go get -u github.com/jinzhu/gorm
> ```

并在使用 gorm 初始化的地方引入 mysql

> ```go
> _ "github.com/go-sql-driver/mysql"
> ```

###  **jwt**

<font color="darkgray">json web token</font>

> go get github.com/dgrijalva/jwt-go

由三部分组成

1. **Header**：TOKEN 的类型，就是JWT，签名的算法，如 HMAC、 SHA256
2. **Payload**：携带的信息，比如用户名、过期时间等，一般叫做 Claim
3. **Signature**：签名，是由header、payload 和你自己维护的一个 secret key 经过加密得来的。

**解码**

生成 token 时使用 SigningMethodHS256 加密

三段中的某一段解码 ( macos | linux )

> echo your_token_part | base64 -D

windows 可以在浏览器中 [解码](https://c.runoob.com/front-end/693)

### **viper**

<font color="darkgray">golang 配置 解决方案</font>

> ```shell
> go get github.com/spf13/viper
> ```

# frontend

nodejs		-- todo install

yarn			-- todo install

vue 			-- todo install

### 创建项目

cmd 进入项目根目录

vue create frontend	---->  回车

--> Manually select features 选择手动选择功能模块

按空格 选择 或 取消选择 功能模块

---> Babel + Router + Vuex + CSS Pre-processors + Linter / Formatter ---> 回车

---> y 选择 history 模式

---> Sass/SCSS (with node-sass)

---> ESLint + Airbnb config

---> Lint on save	保存时检查代码

---> In dedicated config files	配置文件保存在单独的文件中

---> n	这里选 y 的话，可以把当前配置保存为预设，下次可以从预设创建项目

等待完成...

### eslint

webstrom 自带 eslint

---> ctl + alt + s 

---> Languages & Frameworks ---> JavaScript ---> Code Quality Tools ---> ESLint

---> Manual ESLint configuration

---> ESLint package ---> 选择当前项目 node_modules\eslint 目录

---> Run eslint --fix on save ---> 勾选启用自动修复代码规范

---> apply

### botstrap vue

[官网](https://bootstrap-vue.org/) 安装

> ```shell
> yarn add vue bootstrap-vue bootstrap
> ```

引入项目

main.js 中加入

```vue
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
Vue.use(BootstrapVue)
Vue.use(IconsPlugin)
```

引入样式

创建文件夹 scss ---> ./assets/index.scss

```vue
// import bootstrap
@import '~bootstrap';
@import '~bootstrap-vue';
```
并在 main.js 中引入
```vue
import './assets/scss/index.scss';
```

### vuelidate

表单验证插件 [官网](vuelidate.js.org) ，安装：

> ```bash
> npm install vuelidate --save
> ```

引入项目

```vue
import Vue from 'vue'
import Vuelidate from 'vuelidate'
Vue.use(Vuelidate)
```

### vue-axios

Axios 是一个基于 promise 的 **HTTP 库**，可以用在浏览器和 node.js 中。

vue-axios 是将 axios 集成到 Vue.js 的小包装器

[官网]() 安装：

> npm install --save axios vue-axios

引入项目

```vue
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)
```

