---
title: webpack
date: 2016-09-14 10:16:39
updated: 2016-09-14 10:16:39
tags: JS 高级
---
#  项目构建(build)

## 静态资源

什么叫静态资源？
- 我们通过标签的某些属性 (href、src、url...) 引入的资源叫静态资源

常见的静态资源？

1. js
2. css
3. images
4. fonts

静态资源的特点？

1. 需要发送请求拿到具体内容
2. 会在页面请求位置加载对应的资源

## 什么是项目构建?

- 编译项目中的 js、sass、less
- 合并 js / css 等资源文件
- 压缩 js / css / html 等资源文件
- js 语法检查

## 项目构建的作用

- 解决网页中引入静态资源过多加载速度慢、错综复杂的依赖关系的问题
- 如何解决?
  - 合并压缩
- 简化项目构建, 自动化完成构建

# Webpack

[官网](<https://www.webpackjs.com/>)

Gulp 是 `基于任务` 的, Webpack 是 `基于整个项目` 的

<!--more-->

## 什么是 webpack?

- webpack 是一个静态模块打包器, 是基于 node.js 开发出来的 <font color=#c40>前端工具</font>
- 在 webpack 看来, 前端所有的资源文件 (js / json / css / image / less) 都会作为模块处理
  - *唯独不把 html 文件当作模块*
- 它将根据模块的依赖关系进行静态分析, 生成对应的静态资源

## webpack 能做什么?

1. 能处理 js 文件的互相依赖关系
2. 能处理 js 语法兼容问题, **把高级语法转为浏览器能正常识别的语法**
3. 默认 css 样式文件和较小的图片也会被打包到 js 文件中 (*我们可以在发布阶段再提取出来*)

```shell
$ webpack 要打包文件路径 输出文件路径

$ webpack ./src/main.js ./dist/bundle.js
```

## webpack 基本概念

[核心概念](https://www.cnblogs.com/sanhuamao/p/13596032.html)

# 安装

```shell
# 全局安装
$ npm i webpack -g

# or
# 局部安装
$ npm i webpack webpack-cli@3.3.12 -D
```

此时，我们只需在控制台输入 `webpack` 或者 ` npx webpack` 就可以实现打包输出

⚠️ webpack 4.x 以上必须安装 webpack-cli(**最好用3.3.12版本，不然跟 webpack-dev-server 起冲突**)

# 配置文件 webpack.config.js

就是一个 js 文件, 通过 Node 中的模块操作, 向外暴露了一个配置对象

- 在根目录下新建 `webpack.config.js` 配置文件
  - 在 webpack.config.js 里指定打包的入口 (entry) 和 出口 (output) 文件 

    ```js
    const path = require('path')
    
    module.exports = {
        entry: './src/main.js', // 打包入口文件路径
        output: {  // 输出
            path: path.join(__dirname, './dist'), // 打包输出根目录
          filename: 'js/[name].bundle.js' // 打包后输出的文件名
        }
    }
    ```

- 当我们在控制台输入 `webpack` 命令的时候, webpack 做了以下几步
  1. 首先, webpack 发现我们并没有通过命令的形式指定入口和出口
  2. webapck 就会去项目根目录中找 webpack.config.js 配置文件
  3. 当找到配置文件后, webpack 会去解析执行这个配置文件, 当解析执行完配置文件后, 就得到了配置文件中导出的配置对象
  4. 当 webpack 拿到配置对象后, 就 **拿到了配置对象中指定的入口和出口** , 然后进行打包构建


> ⚠️ <font color=#c40>如果修改了配置文件必须重新编译</font>

# NPM 脚本(NPM Scripts)

```json
"scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "build": "webpack"
    },
```

配置好脚本后，我们可以 `npm run build` 来实现构建

# 配置对象的基本属性

- 入口(entry)
  - 应用程序的起点入口
  - 每个 HTML 页面都有一个入口起点。单页应用(SPA)：一个入口起点，多页应用(MPA)：多个入口起点

    ```js
    entry: path.join(__dirname, './src/main.js')
    
    // or
    entry: {
      home: "./home.js",
      about: "./about.js",
      contact: "./contact.js"
    }
    ```

- 输出(output)
  - 指示 webpack 如何去输出、以及在哪里输出你的 bundle、asset 和其他你所打包或使用 webpack 载入的任何内容

    ```js
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/[name].bundle.js'
      }
    ```

- loader
  - webpack 只能理解 JavaScript 和 JSON 文件
  - loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 [模块](https://webpack.docschina.org/concepts/modules)，以供应用程序使用

    ```js
    module: {
        rules: [
          {
            test: /\.(js|jsx)$/,
            use: 'babel-loader'
          }
        ]
    },
    ```

- 插件(plugin)
  - 打包优化，资源管理，注入环境变量等
  - 想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中
    - 多数插件可以通过选项(option)自定义

    ```js
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    
    plugins: [
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
    ```

- mode
  - 通过选择 `development`, `production` 或 `none` 之中的一个，来设置 `mode` 参数
  - 你可以启用 webpack 内置在相应环境下的优化

    ```js
    mode: 'production'
    ```

## 配置属性之 devServer

webpack-dev-server 能够用于快速开发应用程序，可以帮助你在代码发生变化后自动编译代码

- [webpack-dev-server指南](<https://www.webpackjs.com/guides/development/#%E4%BD%BF%E7%94%A8-webpack-dev-server>)
- `webpack-dev-server` 为你*提供了一个简单的 web 服务器*，并且能够实时重新加载 (live reloading)

### 安装 webpack-dev-server

webpack-dev-server 依赖于 webpack, 也就是说本地也必须安装 `webpack `

```shell
$ npm install --save-dev webpack-dev-server webpack
```

### 修改配置文件

告诉开发服务器(dev server)，在哪里查找文件：

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
+   devServer: {
+     contentBase: './dist', // 指定托管的根目录
+     open: true,            // 自动打开浏览器
+     port: 3000             // 指定端口号
+   },
    plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
        title: 'Development'
      })
    ],
    output: {
      filename: 'js/[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
```

### 添加开发脚本

```json
"scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "start": "webpack-dev-server"
  },

// or 将 devServer 的配置写在脚本中
"scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  },
```

我们可以使用一下命令开启开发服务

```shell
$ npm start
```

# 管理输出

## 重新生成 html

我们用插件 `HtmlWebpackPlugin` 生成的 `index.html` 注入了script 标签的文件替换掉我们的原来的 `dist/index.html`

> 执行 webpack-dev-server 生成的文件都存在于内存中，在输出目录下不可见
> 只有执行 webpack 时才会在输出目录生成打包后的文件

- 安装

  ```shell
  $ npm i html-webpack-plugin -D
  ```

- 修改 webpack.config.js

  ```js
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  
  +   plugins: [
  +     new HtmlWebpackPlugin({
  +       title: 'Output Management'
  +     })
  +   ],
  ```

  - ⚠️ *如果不指定模版文件，HtmlWebpackPlugin 会生成一个空的 html 页面*

- **如果有多页面的情况，new HtmlWebpackPlugin() 可以使用多次，且需要指定模版文件**

  ```js
  plugins: [
          new HtmlWebpackPlugin( {   
              // 输出的文件名，可配置路径
              filename: "views/main.html",   
              // 采用的模板文件，可配置路径  
              template: "src/views/main.html",   
              // 生成的html文件中引入的js文件，与entry入口和slpitChunks分离等配置的js文件名相同
              chunks: [ "index", "vendors", "common" ]     
          } ),
          new HtmlWebpackPlugin( {
              title: "about",
              filename: "views/about.html",
              template: "src/views/about.html",
              chunks: [ "about", "vendors", "common" ]
          } ),
          new HtmlWebpackPlugin( {
              title: "list",
              filename: "views/list.html",
              template: "src/views/list.html",
              chunks: [ "list", "vendors", "common" ]
          } ),
    ...
    ]
  ```

- 还可以配置压缩选项

  ```js
  new HtmlWebpackPlugin({
        template: path.resolve(__dirname, './src/index.html'),
        filename: 'index.html',
        minify: { // 压缩 HTML 页面
          removeComments: true, // 删除注释
          removeScriptTypeAttributes: true // 删除 script 标签的 type 属性
        }
  }),
  ```

## 每次打包先清除 dist 再重新生成

我们利用插件 CleanWebpackPlugin 在每次构建前清理 `/dist` 文件夹

```shell
$ npm i clean-webpack-plugin -D
```

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

new CleanWebpackPlugin(), // 每次打包先删除 dist 再重新生成
```

# 管理资源

- **webpack 本身只能加载 js / json 模块**,
  - 如果要加载其他类型的文件(模块), 就需要使用对应的 loader 进行转换 / 加载

- loader 本身也是运行在 node.js 环境中的 js 模块 
  它本身是一个函数, 接收源文件作为参数, 返回转换的结果

- loader 一般以 xxx-loader 的方式命名, xxx 代表了这个 loader 要做的转换功能, 比如 json-loader

- loader 调用是 `从右向左`

## 处理 CSS文件

把 css 文件以 import './css/index.css' 的形式引入 index.js 中打包会报错，需要专门的 loader 去解析

- 安装

  ```shell
  npm i style-loader css-loader@2.1.0 -D
  ```

- 修改 webpack.config.js

  ```js
  // 新增 module 节点, 设置第三方文件的匹配和处理规则
  module: {
      rules: [
          { 
              test: /\.css$/, 
              // 先调用 css-loader, 处理结果交给 style-loader 进一步处理, 然后交给 webpack 处理
              use: [ 'style-loader', 'css-loader' ] 
          },   
      ]
  }
  ```

### css 模块化

[css 模块化](https://www.webpackjs.com/loaders/css-loader/#modules)

```js
{
  test: /\.css$/,
  use: [
    {
      loader: 'css-loader',
      options: {
        modules: true,
        localIdentName: '[path][name]__[local]--[hash:base64:5]'
      }
    }
  ]
}
```

## 处理 Less 文件

- 安装

  ```shell
  npm i less-loader less -D
  ```

- 修改 webpack.config.js

  ```js
  module: {
      rules: [
          { 
              test: /\.less$/, 
              use: ['style-loader', 'css-loader', 'less-loader'] 
          }
      ]
  }
  ```

## 处理图片路径 / 字体文件

- 安装

  ```shell
  npm i url-loader file-loader@2.0.0 -D
  ```

- 修改 webpack.config.js

  ```js
  module: {
      rules: [
          // limit 给定的值是图片大小, 单位是byte, 当引用的图片小于给定的值会转为base64位的字符串
          // name  设置编译后的图片路径 [hash:8]-[name].[ext]  8位 hash 值-原图名字
          { 
              test: /\.(jpg|png|gif|bmp|jpeg)$/, 
              use: [
                {
                  loader: 'url-loader',
                  options: {
                    limit: 8192,
                    name: '[hash:6]-[name].[ext]',
                    outputPath: './images'
                  }
                }
            ]
          }, 
          // 处理字体文件
          { 
              test: /\.(ttf|eot|svg|woff|woff2)$/, 
              use: ['url-loader'] 
          } 
      ]
  }
  ```

## 处理 img 标签中的图片

- 安装

  ```shell
  $ npm i -D html-loader
  ```

- 配置

  ```js
  rules: [{
      test: /\.html$/,
      use: [ {
        loader: 'html-loader',
        options: {
          minimize: true
        }
      }],
    }]
  ```

## 处理 es6 语法(babel 的配置)

webpack 默认只能处理一部分 es6 语法，其他不能处理的语法需要 babel-loader 来处理

- 安装

  ```shell
  npm i babel-loader @babel/core @babel/preset-env webpack -D
  npm i @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D
  npm i @babel/runtime
  ```

- 修改 webpack.config.js

  ```js
  module: {
    rules: [ 
      { 
          test: /\.js$/, 
          exclude: /node_modules/,   // exclude 排除 node_modules 文件夹
          use: [
              { 
                  loader: 'babel-loader', 
                  options: { 
                      presets: ['@babel/preset-env'],
                      plugins: ['@babel/plugin-proposal-class-properties', '@babel/transform-runtime']
                  }
              }
          ] 
      }
    ]
  }
  ```

## 处理 vue 文件

- 安装

  ```shell
  npm i vue-loader vue-template-compiler -D
  ```

- 修改 webpack.config.js

  ```js
  const VueLoaderPlugin = require('vue-loader/lib/plugin')
  
  {
      test: '/\.vue$/',
      use: ['vue-loader']
  }
  
  plugins: [
      new VueLoaderPlugin()
  ]
  ```

# publicPath

为项目中的所有资源指定一个基础路径
指定在浏览器中所引用的资源对应的**公开 URL** (会在默认的 url 前加上指定的 publicPath)

1. 相对 URL(relative URL) 会被相对于 HTML 页面解析
2. 相对于服务的 URL(Server-relative URL) 会相对于托管目录来解析
3. 相对于协议的 URL(protocol-relative URL) 或绝对 URL(absolute URL) 也可是可能用到的
   - 例如：当将资源托管到 CDN 时

```js
module.exports = {
  //...
  output: {
    // One of the below
    publicPath: 'https://cdn.example.com/assets/', // CDN（总是 HTTPS 协议）
    publicPath: '//cdn.example.com/assets/', // CDN（协议相同）
    publicPath: '/assets/', // 相对于服务(server-relative)
    publicPath: 'assets/', // 相对于 HTML 页面
    publicPath: '../assets/', // 相对于 HTML 页面
    publicPath: '', // 相对于 HTML 页面（目录相同）
  }
}
```

# 插件

- 插件可以完成一些 loader 不能完成的功能
- 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定

## webpack.ProvidePlugin

全局暴露，会自动加载 jquery 注意 q 是小写，无需在 import jQuery from ...

```js
new webpack.ProvidePlugin({   // 全局暴露 jquery,则无需再在项目中引入
      "$": "jquery",
      "jQuery": "jquery",
      "window.jQuery": "jquery"
}),
```

# webpack.config.js 配置参考基本模版

```js
const path = require('path')
const webpack = require('webpack')

const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
// 用到 vue 才引入
// const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  mode: 'development', // 声明当前环境

  entry: {
    app: './src/js/index.js',
    common: './src/js/common.js',
  },

  output: {
    // 打包输出配置
    path: path.join(__dirname, './dist'), // 打包输出的根目录
    filename: 'js/[name].bundle.js', // 打包后输出的 js 文件名
    publicPath: '', // 指定静态资源的读取目录
  },

  plugins: [
    // new VueLoaderPlugin(),
    new CleanWebpackPlugin(), // 每次打包先删除 dist 再重新生成
    new HtmlWebpackPlugin({
      title: 'index',
      filename: 'index.html', // 输出的文件名，可配置路径
      template: 'src/index.html', // 采用的模板文件，可配置路径
      // 生成的html文件中引入的js文件，与entry入口和slpitChunks分离等配置的js文件名相同
      chunks: ['app', 'vendors', 'common'],
    }),
    new webpack.ProvidePlugin({
      // 全局暴露 jquery,则无需再在项目中引入
      $: 'jquery',
      jQuery: 'jquery',
      'window.jQuery': 'jquery',
    }),
  ],

  module: {
    rules: [
      {
        test: /\.css$/,
        // 先调用 css-loader, 处理结果交给 style-loader 进一步处理, 然后交给 webpack 处理
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.less$/,
        use: ['style-loader', 'css-loader', 'less-loader'],
      },
      // limit 给定的值是图片大小, 单位是byte, 当引用的图片小于给定的值会转为base64位的字符串
      // name  设置编译后的图片路径 [hash:8]-[name].[ext]  8位 hash 值-原图名字
      {
        test: /\.(jpg|png|gif|bmp|jpeg)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
              name: '[hash:6]-[name].[ext]',
              outputPath: './images', // 图片文件打包后的输出目录，相对于 dist
            },
          },
        ],
      },
      // 处理字体文件
      {
        test: /\.(ttf|eot|svg|woff|woff2)$/,
        use: ['url-loader'],
      },
      {
        // 处理 img 标签中的图片
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: {
              minimize: true,
            },
          },
        ],
      },
      {
        test: /\.js$/,
        exclude: /node_modules/, // exclude 排除 node_modules 文件夹
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env'],
              plugins: ['@babel/plugin-proposal-class-properties', '@babel/transform-runtime'],
            },
          },
        ],
      },
      {
        test: '/.vue$/',
        use: ['vue-loader'],
      },
    ],
  },

  resolve: {
    extensions: ['.js', '.jsx', '.vue'],
  },

  devServer: {
    contentBase: './dist', // 指定托管的根目录
    open: true, // 自动打开浏览器
    port: 3000, // 指定端口号
  },
}

```

# webpack 的发布策略

*开发环境(development)*和*生产环境(production)*的构建目标差异很大。在*开发环境*中，我们追求高效的开发能力；

生产环境则追求更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。

为此，通用有以下两种配置方案

## 新建一个完整配置文件 `webpack.publish.config.js`

1. 将 webpack.config.js 的配置拷贝过来, 删除一些开发配置即可
- 删除那些用户用不到的测试数据和文件

```js
// 删除 devServer 节点
devServer: {
    hot: true,
    open: true,
    port: 4321
}
```

2. 新增 pub 命令
⚠️ 我们在开发阶段使用 webpack-dev-server 执行打包命令生成的 index.html、bundle.js、图片文件等存在于内存中（相当于托管在项目的根目录（contentBase）下，*但是我们看不到*）

- **但是，在发布阶段，我们需要生成的index.html、bundle.js、图片等文件存在于 dist 目录中，那么，我们就要使用 webpack 命令来进行打包；还需要指定图片存放路径**
- 在 `package.json` 中的script节点下新增 `pub` 命令，通过 `--config` 指定webpack启动时要读取的配置文件

  ```json
  "pub": "webpack --config webpack.publish.config.js"
  ```

## 保留一个“通用”配置

- 保留一个“通用”配置, 再根据环境分别合并开发和生产配置
- [配置参考](https://www.webpackjs.com/guides/production/#%E9%85%8D%E7%BD%AE)

1. 安装 `webpack-merge` 

   ```shell
   $ npm install --save-dev webpack-merge
   ```

2. 新建三个配置文件

   ```js
   webpack-demo
     |- package.json
   - |- webpack.config.js
   + |- webpack.common.js
   + |- webpack.dev.js
   + |- webpack.prod.js
     |- /dist
     |- /src
       |- index.js
       |- math.js
     |- /node_modules
   ```

3. 从 webpack.config.js 中抽出公共代码

   ```js
   // webpack.common.js
   // 在 webpack.common.js 中，我们设置了 entry 和 output 配置
   // 并且在其中引入这两个环境公用的全部插件
   + const path = require('path');
   + const {CleanWebpackPlugin} = require('clean-webpack-plugin');
   + const HtmlWebpackPlugin = require('html-webpack-plugin');
   +
   + module.exports = {
   +   entry: {
   +     app: './src/index.js'
   +   },
   +   plugins: [
   +     new CleanWebpackPlugin(),
   +     new HtmlWebpackPlugin({
   +       title: 'Production'
   +     })
   +   ],
   +   output: {
   +     filename: 'js/[name].bundle.js',
   +     path: path.resolve(__dirname, 'dist')
   +   }
   + };
   ```

4. 配置开发环境

   ```js
   // webpack.dev.js
   + const merge = require('webpack-merge');
   + const common = require('./webpack.common.js');
   +
   + module.exports = merge(common, {
   +   devServer: {
   +     contentBase: './dist',
   +     open: true,
   +     port: 3000
   +   }
   + });
   ```

5. 配置生产环境

   ```js
   // webpack.prod.js
   + const merge = require('webpack-merge');
   + const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
   + const common = require('./webpack.common.js');
   +
   + module.exports = merge(common, {
   +   plugins: [
   +     new UglifyJSPlugin()
   +   ]
   + });
   ```

6. 配置 scripts

   ```json
   // package.json
   "scripts": {
   -     "start": "webpack-dev-server --open",
   +     "start": "webpack-dev-server --open --config webpack.dev.js",
   -     "build": "webpack"
   +     "build": "webpack --config webpack.prod.js"
   },
   ```

## 发布的进一步处理

<font color=#c40>我们需要对 webpack.prod.js 进行进一步处理</font>

> 压缩代码
> 抽离第三方包和公用模块

### 分离第三方包和公用模块

[SplitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/)

- bundle.js 中只存放自己的代码
- 第三方包的引入代码抽离到另外的 js 文件中
- 公用模块也抽离出来

  ```js
  // webpack.prod.js
  
  // 提取公共模块，包括第三方库和自定义工具库等
      optimization: {
          splitChunks: {
                // async表示抽取异步模块，all表示对所有模块生效，initial表示对同步模块生效
              chunks: "all",  
              cacheGroups: {
                  vendors: {  // 抽离第三方包
                      test: /[\\/]node_modules[\\/]/,  // 指定是node_modules下的第三方包
                      name: "vendors",
                      priority: -10                   // 抽取优先级
                  },
                  commons: {   // 抽离公用模块
                      name: "common",
                      minChunks: 2,   // 表示公用模块被不同文件引用了多少次，才能抽离
                      priority: -20
                  }
              }
          }
      }
  ```

### 压缩 js 代码

- 安装

  ```shell
  npm i terser-webpack-plugin --D
  ```

- 配置压缩 js 代码

  ```js
  const TerserPlugin = require('terser-webpack-plugin')
  
  optimization: {
      minimize: true,
      minimizer: [new TerserPlugin()],
    },
  ```

### 压缩 HTML 页面（可以放在 webpack.common.js 中）

- 配置

  ```js
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  
  plugins: [
      new HtmlWebpackPlugin({
          template: path.join(__dirname, './src/index.html'), 
          filename: 'index.html',
          minify: {
              collapseWhitespace: true, // 移除空格
                        removeComments: true,     // 移除注释
                        removeScriptTypeAttributes: true, // 移除 script 标签的 type 属性
          }
      })
  ]
  ```

### 抽离样式文件，处理 CSS 路径问题（可以放在 webpack.common.js 中）

- 安装

  ```shell
  npm i mini-css-extract-plugin -D
  ```

- 修改配置

  ```js
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");
  
  module.exports = {
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            {
              loader: MiniCssExtractPlugin.loader,
              options: {
                publicPath: '../' // // 指定抽取的时候, 自动为路径加上../ 前缀
              },
            },
            'css-loader',
          ],
        },
        {
           test: /\.scss$/,
           use: [
            {
              loader: MiniCssExtractPlugin.loader,
              options: {
                publicPath: '../' 
              },
            },
            'css-loader',
            'sass-loader'
          ],
        }
      ]
    },
    plugins: [
      new MiniCssExtractPlugin({
        // Options similar to the same options in webpackOptions.output
        // all options are optional
        filename: 'css/styles.css',
        ignoreOrder: false
      })
    ]
  }
  ```

### 压缩 css

- 安装

  ```shell
  npm i optimize-css-assets-webpack-plugin -D
  ```

- 配置

  ```js
  const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin')
  
  plugins: [ 
     new OptimizeCSSAssetsPlugin()
   ]
  ```

### 不显示 `*Entrypoint mini-css-extract-plugin = **` 警告
加入
```js
// webpack.pub.config.js
  stats: {
      // One of the two if I remember right
      entrypoints: false,
      children: false
  },
```
# 使用参考
[多页面打包](https://blog.csdn.net/lyandgh/article/details/90906110)
