---
title: art-template
date: 2016-07-31 11:11:52
updated: 2016-07-31 11:11:52
tags: JS 高级
---
# 介绍

art-template 是一个简约、超快的模板引擎

- 它采用作用域预声明的技术来优化模板渲染速度，从而获得接近 JavaScript 极限的运行性能，并且 `同时支持 NodeJS 和浏览器`

[官网](<https://aui.github.io/art-template/zh-cn/docs/>)

<!--more-->

## 特性

1. 拥有接近 JavaScript 渲染极限的的性能
2. 调试友好：语法、运行时错误日志精确到模板所在行；支持在模板文件上打断点（Webpack Loader）
3. 支持 Express、Koa、Webpack
4. 支持模板继承与子模板
5. 浏览器版本仅 6KB 大小

# 基础语法

art-template 同时支持两种模板语法

- 标准语法 —— 可以让模板更容易读写
  - 标准语法支持基本模板语法以及基本 JavaScript 表达式
- 原始语法 —— 具有强大的逻辑处理能力
  - **原始语法中可以使用任意合法的 js 语句**

## 变量/表达式

```html
// 标准语法
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}

// eg
<img src="/uploads/{{ $value.image }}" />

// 原始语法
<%= value %>
<%= data.key %>
<%= data['key'] %>
<%= a ? b : c %>
<%= a || b %>
<%= a + b %>
```

## 条件渲染

```html
// 标准语法
{{if value}} 
... 
{{/if}}

{{if v1}} 
... 
{{else if v2}} 
... 
{{/if}}

// 原始语法
<% if (value) { %> 
    ... 
<% } %>
    
<% if (v1) { %> 
    ... 
<% } else if (v2) { %> 
    ... 
<% } %>
```

## 循环

target 支持 数组 和 对象

```html
// 标准语法
// $value  和 $index 可以自定义: {{each target val key}}

{{each target}}
    {{$index}} {{$value}}
{{/each}}

// 原始语法
<% for(var i = 0; i < target.length; i++){ %>
    <%= i %> <%= target[i] %>
<% } %>
```

## 自定义变量

```html
// 标准语法
{{set temp = data.sub.content}}

// 原始语法
<% var temp = data.sub.content %>
```

## 过滤器

```js
// 注册过滤器
// 过滤器函数第一个参数接受目标值
template.defaults.imports.dateFormat = function(date, format){/*[code..]*/}
template.defaults.imports.timestamp = function(value){return value * 1000}
```

```html
// 使用
// 标准语法
{{date | timestamp | dateFormat 'yyyy-MM-dd hh:mm:ss'}}

// 原始语法
<%= $imports.dateFormat($imports.timestamp(date), 'yyyy-MM-dd hh:mm:ss') %>
```

# 使用

## 浏览器中使用

1. 下载[template-web.js](<https://unpkg.com/art-template@4.13.2/lib/template-web.js>)

2. 编写 script 模版标签

   ```html
   <script src="lib/template-web.js"></script>
   <script id="tpl-user" type="text/template">
   {{if user}}
     <h2>{{user.name}}</h2>
   {{/if}}
   </script>
   ```

3. 使用 template() 方法生成模版字符串，挂载到页面上

   ```js
   var htmlStr = template('tpl-user', { user })
   
   ul.html(htmlStr)
   ```

⚠️  浏览器无法加载外部文件，template() 的第一个参数为存放模板的 script 元素 id (**不带 #**)

### 总结浏览器端模版引擎的使用步骤

1. 选择一个模版引擎
2. 下载模版引擎 js 文件
3. 引入到页面中
4. 准备一个模版
5. 准备好数据
6. 通过模版引擎提供的 API 将模版和数据整合得到 html 字符串
7. 将得到的 `模版字符串` 渲染到页面中

### 为什么使用 script 标签写模版?

1. 如果写在 js 代码里, 维护不方便, 不能换行, 标签没有着色
2. script 标签的内容不会显示在界面上

## 服务端使用

1. npm 下载 art-template

   ```shell
   $ npm i art-template
   ```

2. 引入并使用

   ```js
   // 1.在 Node 中使用
   var template = require('art-template')
   var html = template(__dirname + '/tpl-user.html', {
       user: {
           name: 'aui'
       }
   })
   res.end(html) // 直接把完整的页面字符串返回到客户端
   
   // 2.配置了 ecpress 后使用
   app.get('/', function (req, res) {
       res.render('index.art', {
           user: {
               name: 'aui',
               tags: ['art', 'template', 'nodejs']
           }
       })
   })
   ```

# 模版文件内部无法访问 js 文件中变量的解决方法

我们无法在模版文件中使用 $ 变量， 如何解决？

- 使用辅助方法定义在模版内可以使用的函数

  ```js
  template.helper('getJquery', function () {
    return jQuery
  })
  ```

  ```html
  // 然后在模版文件中调用此方法
  <% 
    var $ = getJquery() 
    $.each(arr, function (i, item) {
  %>
    <li><%=item.name%></li>
    
  <% }) %>
  ```

# 在浏览器端和服务端使用模版引擎的区别

- 在浏览器端 *只能在 script 标签中使用* art-template 语法
- 在服务端 *可以在 模版页面 的任何位置* 加 art-template 语法
- [区别参考](https://www.cnblogs.com/chuanzi/p/10512516.html)
- <font color=#c40>模版继承和子模版语法只能在服务端使用</font>

# 客户端和服务端用同一个模版引擎带来的问题

- 问题
  - 如果我们在服务端和客户端都使用 art-template 模版引擎，服务端在返回页面模版时会对客户端 script 标签里的模版语法先进行解析，导致我们在客户端数据无法写入 script 中的模版中
- 解决
  - 服务端可以用 [Numjucks](https://mozilla.github.io/nunjucks/) 模版引擎
  - 客户端用 art-template 原始语法 
    - 因为 numjucks 能够解析 `{{ }}` 语法，但是不能解析 `<%`

# 高阶语法(服务端语法)

## 模版继承

- 标准语法

  ```html
  {{extend './layout.art'}}
  {{block 'head'}} 
  ... 
  {{/block}}
  ```

- 原始语法

  ```html
  <% extend('./layout.art') %>
  <% block('head', function(){ %> 
      ... 
  <% }) %>
  ```

- 模板继承允许你构建一个<font color=#c40>基本模板“骨架”</font>

  ```html
  <!-- layout.html -->
  <!Doctype html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>{{block 'title'}}My Site{{/block}}</title>
  
      {{block 'head'}}
      <!-- 默认内容, 如果 head 中没有内容, 则使用默认; 有内容, 则替换掉此处内容 -->
      <link rel="stylesheet" href="main.css"> 
      {{/block}}
  </head>
  <body>
      {{block 'content'}}{{/block}}
  </body>
  </html>
  ```

  ```html
  <!-- index.html -->
  <!-- 继承基本模版, 再去填模版中的坑 -->
  {{extend './layout.html'}}
  
  {{block 'title'}}{{title}}{{/block}}
  
  {{block 'head'}}
      <link rel="stylesheet" href="custom.css">
  {{/block}}
  
  {{block 'content'}}
  <p>This is just an awesome page.</p>
  {{/block}}
  ```

## 子模版

把子模版中的内容全部拿过来放到对应位置

- 标准语法

  ```html
  {{include './header.html'}}
  {{include './header.html' data}}
  ```

- 原始模版

  ```html
  <% include('./header.html') %>
  <% include('./header.html', data) %>
  ```

- `data` 默认值为 `$data`；标准语法不支持声明 `object` 与 `array`，只支持引用变量，而原始语法不受限制

