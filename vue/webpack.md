[webpack](https://www.webpackjs.com/concepts/) 是一个 JavaScript 应用 的 <font color="red">静态模块打包工具</font>

![webpack.jpg](https://i.loli.net/2020/08/08/hdZOzn5WPE2SjYT.jpg)

### 安装

> npm install webpack -g					// 全局安装，如果要按照指定版本，使用 webpack@x.x.x

> npm install webpack --save-dev	 // 项目内开发时依赖安装

webpack4 需要额外安装 webpack-cli

> npm install webpack@4 webpack-cli --save-dev

### 打包命令

> webpack from.js to.js

webpack4 需要 -o 参数

> webpack from.js -o to.js

### loader

webpack 本身只能处理 JavaScript 模块，要处理其他类型，要安装对应的 loader

 css

> cnpm install css-loader style-loader --save-dev

### --save-dev

即 开发时依赖，打包时会被丢弃

### 参数

package.json

```json
{
  "name": "webpackdemo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "webpack" // 不支持注释，请清除注释
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^2.0.2",
    "style-loader": "^0.23.1",
    "webpack": "^3.5.6"
 }
}
```

webpack.config.js

```javascript
// webpack2 的配置
module.exports = {
    entry:  __dirname + "/app/main.js", 	// 唯一入口文件
    output: {
        path: __dirname + "/common", 		// 打包后的文件存放的地方
        filename: "index.js" 				// 打包后输出文件的文件名
    }
}
// webpack4 的配置
const path = require('path')
module.exports = {
    // development: 开发环境
    // production:  为生产环境
    mode: "development",
    entry:  __dirname + "/app/main.js",
    output: {
        path: path.resolve(__dirname, "common"), 		
        filename: 'index.js', 	
        publicPath: 'dist/'
    },
    module:{
        rules:[
            test: /\.css$/,
            use:{
               'style-loader',
               'css-loader',
               {loader: 'url-loader',
            	options: {limit: 8192, name:'img/[name].[hash:8].[ext]'}}
            }]
        ]
    }
}
```

“**__dirname**” 是node.js中的一个全局变量，它指向当前执行脚本所在的目录

配置后的打包命令：**npm run serve**

<font color="red">**命令行窗口直接 webpack 会运行全局的 webpack，而使用 npm 时，会优先执行本项目的 webpack**</font>

### 图片文件处理

> npm install --save-dev url-loader

当图片要大于 limit 时，要使用 file-loader

### ES6语法处理

> npm install --save-dev babel-loader@7 babel-core babel-preset-es2015

webpack.config.js

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['es2015'],
        }
      }
    }
  ]
}
```

