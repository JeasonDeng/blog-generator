---
title: AJAX
date: 2016-07-15 09:57:46
updated: 2016-07-15 09:57:46
tags: 请求与响应
---
# 概述

Web 程序最初的目的就是将信息(数据)放到公共的服务器，让所有网络用户都可以通过浏览器访问

- 在此之前, 我们可以通过以下几种方式让 `浏览器发出对服务端的请求`，获得服务端的数据
  - 地址栏输入地址，回车，刷新
  - 特定元素的 href 或 src 属性
  - 表单提交
- XMLHttpRequest 是 js 原生提供的 <font color=#c40>用来与服务器交互的核心对象类型</font>, ***使用 xhr 对象可以在不影响用户操作的情况下更新页面局部内容***
- AJAX(Asynchronous JavaScript and XML)，最早出现在 2005 年的 Google Suggest，是在浏览器端进行网络编程(发送请求、接收响应)的技术方案，它使我们 `可以通过 JavaScript 直接获取服务端最新的内容而不必重新加载页面`。让 Web 更能接近桌面应用的用户体验
- 说白了，<font color=#c40)>A JAX 就是浏览器提供的一套 API，可以通过 JavaScript 调用，从而实现通过代码控制请求与响应</font>。实现网络编程
- 涉及 AJAX 请求的页面 ''不能 ''以 `文件协议方式` 访问
  - 因为 AJAX 默认不能发送不同源请求
  - 但是, 如果服务端设置了允许跨域请求 (**Access-Control-Allow-Origin**), 则可以以文件协议方式访问
<!--more-->

# 快速上手

使用 AJAX 的过程可以类比平常我们访问网页过程
```js
// 创建一个 XMLHttpRequest 类型的对象 ———— 相当于打开了一个浏览器 
var xhr = new XMLHttpRequest()

// 打开与一个网址之间的连接 ———— 相当于在地址栏输入访问地址 
xhr.open('GET', './time.php')

// 通过连接发送一次请求 ———— 相当于回车或者点击访问发送请求 
xhr.send(null)

// 指定 xhr 状态变化事件处理函数 ———— 相当于处理网页请求后的操作 
xhr.onreadystatechange = function () {
// 通过 xhr 的 readyState 判断此次请求的响应是否接收完成 
    if (this.readyState === 4) {
        // 通过 xhr 的 responseText 获取到响应的响应体内容
        console.log(this.responseText)
    }
}
// or
xhr.onload = function () { // html5 提供的事件, 响应完成时调用
  console.log(this.readyState) // 4
  console.log(this.responseText)
}
```

## readyState

由于 readystatechange 事件是 <font color=#c40>**在 xhr 对象状态变化时触发** (不单是在得到响应时)</font>

- 也就意味着 ***这个事件会被触发多次***，所以我们有必要了解每一个状态值代表的含义:
| readyState | 状态描述         | 说明                                                       |
| ---------- | ---------------- | ---------------------------------------------------------- |
| 0          | UNSENT           | 代理 XHR 被创建，但尚未调用 open() 方法                    |
| 1          | OPENED           | open() 方法已经被调用，建立了连接                          |
| 2          | HEADERS_RECEIVED | send() 方法已经被调用结束，已经可以获取状态行和 **响应头** |
| 3          | LOADING          | 响应体下载中， responseText 属性可能已经包含部分数据       |
| 4          | DONE             | 响应体下载完成，可以直接使用 responseText                  |

- 通过理解每一个状态值的含义得出一个结论: <font color=#c40>一般我们都是在 readyState 值为 4 时，执行响应的后续逻辑</font>

## 遵循 HTTP

**本质上 XMLHttpRequest 就是 JavaScript 在 Web 平台中发送 HTTP 请求的手段**，所以我们发送出去的请求任然是 HTTP 请求，同样符合 HTTP 约定的格式: 
```js
// 设置请求报文的请求行
xhr.open('GET', './time.php')

// 设置请求头
xhr.setRequestHeader('Accept', 'text/plain')

// 设置请求体
xhr.send(null)

xhr.onreadystatechange = function () {
    if (this.readyState === 4) {
        // 获取响应状态码
        console.log(this.status)
    // 获取响应状态描述
        console.log(this.statusText)
        // 获取响应头信息
        console.log(this.getResponseHeader('Content‐Type')) // 响应头类型
        console.log(this.getAllResponseHeader()) // 全部响应头
        // 获取响应体
        console.log(this.responseText) // 文本形式
        console.log(this.responseXML) // XML 形式，了解即可不用了
    }
}
```

# 数据接口
**返回数据的地址** 一般称作数据接口

# 具体用法

## GET 请求

通常在一次 GET 请求过程中，`参数传递都是通过 URL 地址中的 ? 参数传递`
```js
var xhr = new XMLHttpRequest()

// GET 请求传递参数通常使用的是问号传参
// 这里可以在请求地址后面加上参数，从而传递数据到服务端
xhr.open('GET', './delete.php?id=1')

// 一般在 GET 请求时无需设置响应体，可以传 null 或者干脆不传
xhr.send(null)

xhr.onreadystatechange = function () {
    if (this.readyState === 4) {
        console.log(this.responseText)
    }
}
// 一般情况下 URL 传递的都是参数性质的数据，而 POST 一般都是业务数据
```

## POST 请求

POST 请求过程中，`都是采用请求体承载需要提交的数据`

- **请求头的设置 —— xhr.setRequestHeader()** 
  - 根据 **请求体内容格式设置 Content-type 的值**, 告诉服务器我们 <font color=#c40>发送的数据格式</font>
    - 如果发送的是 `'key1=value1&key2=value2'` 
      > xhr.setRequestHeader('Content‐Type', 'application/x‐www‐form‐urlencoded')

      告诉服务器此次请求体格式为 `urlencoded` 以便于服务端接收数据

    - 如果发送的是 json 字符串, `'{"foo": "bar"}'`
      > xhr.setRequestHeader('Content‐Type', 'application/json')

- **请求体内容 —— xhr.send(请求体内容)**
  ```js
  var xhr = new XMLHttpRequest()
  
  // open 方法的第一个参数的作用就是设置请求的 method
  xhr.open('POST', './add.php')
  
  // 设置请求头中的 Content‐Type 为 application/x‐www‐form‐urlencoded
  // 告诉服务器此次请求体格式为 urlencoded 以便于服务端接收数据
  // 如果发送的是 json 字符串, 则 Content‐Type 为设为 application/json
  //        xhr.send('{"foo": "bar"}')
  xhr.setRequestHeader('Content‐Type', 'application/x‐www‐form‐urlencoded')
  
  // 需要提交到服务端的数据可以通过 send 方法的参数传递
  // 格式:key1=value1&key2=value2
  xhr.send('key1=value1&key2=value2')
  
  xhr.onreadystatechange = function () {
    if (this.readyState === 4) {
        console.log(this.responseText)
    }
  }
  ```

## 同步与异步 (同步 AJAX 已被废弃)

xhr.open() 方法第三个参数要求传入的是一个 bool 值

- 其作用就是 `设置此次请求是否采用异步方式执行`，默认为 true; 
  - 如果需要同步执行可以通过传递 false 实现

```js
// 异步
console.log('before ajax')
var xhr = new XMLHttpRequest()

// 默认第三个参数为 true 意味着采用异步方式执行 
xhr.open('GET', './time.php', true) 

xhr.send(null)
xhr.onreadystatechange = function () {
    if (this.readyState === 4) { 
        // 这里的代码最后执行 
        console.log('request done')
    } 
}
console.log('after ajax')
```

如果采用同步方式执行，则代码会卡死在 xhr.send() 这一步

- 等到代码完全执行完毕才接着执行
```js
// 同步
console.log('before ajax')
var xhr = new XMLHttpRequest()

// 同步方式
xhr.open('GET', './time.php', false)

// 等到请求的响应全部执行完才会执行之后的代码
xhr.send(null) 
console.log(xhr.readyState) // 4, 之后一直是 4

// readystatechange 无法触发, 因为此时 readyState 不会有变化
xhr.onreadystatechange = function () {
    if (this.readyState === 4) { 
        console.log('request done')
    } 
}
console.log('after ajax')
```

- 了解同步模式即可，切记不要使用同步模式
- ⚠️ **同步模式下, 绑定 readystatechange 事件要放在 send() 之前, 否则无法触发**

# 响应数据格式

服务端返回何种格式的数据，这种格式如何在客户端用 JavaScript 解析?

- 服务端应该设置一个合理的 `Content-type`
  - eg
    ```php
    <?php
      header('Content-Type: application/json')
    ?>
    ```

## JSON

一种数据描述手段，类似于 JavaScript 字面量方式
服务端采用 ` JSON 格式` 返回数据，客户端按照 JSON 格式解析数据

# 处理响应数据渲染

[模版引擎](<https://aui.github.io/art-template/zh-cn/docs/>)

- 模板引擎实际上就是一个 API
  - 模板引擎有很多种，使用方式大同小异，目的为了可以更容易的将数据渲染 HTML 中

# 封装

## AJAX 请求封装

函数就可以理解为一个想要做的事情，函数体中约定了这件事情做的过程，直到调用时才开始工作

- 将函数作为参数传递就像是将一个事情交给别人，这就是委托的概念
  ```js
  /**
  * 发送一个 AJAX 请求
  * @param {String} method 请求方法
  * @param {String} url 请求地址
  * @param {Object} params 请求参数
  * @param {Function} done 请求完成过后需要做的事情(委托/回调) 
  */
  
  function ajax (method, url, params, done) { 
      var xhr = new XMLHttpRequest()
      
      // 统一转换为大写便于后续判断
        method = method.toUpperCase()
      
        // 对象形式的参数转换为 urlencoded 格式 
      if (typeof(params) === 'object') {
        var arr = []
        for (var key in params) {
            arr.push(key + '=' + params[key])
            } 
          params = arr.join('&')
      }
      
      // 如果是 GET 请求就设置 URL 地址 问号参数 
      if (method === 'GET') {
        url += '?' + params
        }
      
      xhr.open(method, url)
  
      var data = null
      
        // 如果是 POST 请求就设置请求体 
        if (method === 'POST') {
            xhr.setRequestHeader('Content‐Type', 'application/x‐www‐form‐urlencoded')
            data = params
        }
      
      xhr.send(data)
      
        xhr.addEventListener('readystatechange', function () {
        if (this.readyState !== 4) return
          
            // 尝试通过 JSON 格式解析响应体 
        done && done(JSON.parse(this.responseText))
    })
  }
  
  ajax('get', './get.php', { id: 123 }, function (data) {
    console.log(data)
  })
  ajax('post', './post.php', { foo: 'posted data' }, function (data) {
    console.log(data)
  })
  ```

# jQuery 中的 AJAX

jQuery 中有一套专门针对 AJAX 的封装，功能十分完善，经常使用，需要着重注意

- [参考](<http://www.lovean.com/doc/jquery1.12/jquery.ajax.html>)
- [官网参考]()

## $.ajax() 使用

```js
$.ajax({
    url: './get.php',
    type: 'get',
    dataType: 'json', 
    data: { id: 1 }, 
    beforeSend: function (xhr) { // open 之前
       console.log('before send', xhr.readyState) // 0
    },
    success: function (res) {
        // res 会自动根据服务端响应的 Content-type 自动转换为对象; 这是 jQuery 内部实现的
        // 如果服务端没有设置 Content-type, 可以设置配置选项 dataType: 'json'
       console.log(res)
    },
    error: function (xhr) {
       console.log('error', xhr)
    },
    complete: function (xhr) {
       console.log('request completed', xhr)
    } 
})
```

- 常用选项参数介绍
  - url —— 请求地址
  - type —— 请求方法，默认为 get
  - data —— 需要传递到服务端的数据，<font color=#c40>如果是 GET 请求则通过 URL 传递，如果 POST 则通过请求体传递  </font>
  - dataType —— 设置服务端 `响应数据类型`
    ⚠️ **data 和 dataType 没有关系**,  *一个是发送给服务端的数据, 一个是服务端返回的响应数据的类型*
  - contentType —— 请求体内容类型，默认 application/x-www-form-urlencoded  
  - timeout —— 请求超时时间
  - beforeSend —— 请求发起之前触发 (在 open 之前, readyState 为 0)
  - success —— 请求成功之后触发(响应状态码 200) 
    - **设置了 dataType 选项, 就不再关心服务端响应数据的 Content-Type 了**
  - error —— 请求失败触发 
  - complete —— 请求完成触发 (不管成功与否) 

 ### $.get()

GET 请求快捷方法
```js
$.get('json.php', { id: 1 }, function (res) {
    console.log(res)
})
```

### $.post()

POST 请求快捷方法
```js
$.post('json.php', { id: 1 }, function (res) {
    console.log(res)
})
```

### $.getJSON()

如果服务端没有指定返回数据的类型
- 可以这样将服务端返回的数据 `强制转为 json 格式对象`
```js
$.getJSON('json.php', { id: 1 }, function(res) {
    console.log(res)
})
```

## $(selector).load(url, [data,[callback]])

载入远程 HTML 文件代码并插入至 DOM 中 (<font color=#c40>实现局部加载</font>)
- 默认使用 GET 方式

  ```html
  <div class="container pt-4">
      <h1>会员中心</h1>
      <hr>
      <div class="row">
        <aside class="col-md-3">
          <div class="list-group">
            <a class="list-group-item list-group-item-action" href="index.html">我的资料</a>
            <a class="list-group-item list-group-item-action" href="cart.html">我的购物车</a>
            <a class="list-group-item list-group-item-action" href="orders.html">我的订单</a>
          </div>
        </aside>
        <main id="main" class="col-md-9">
          <h2>我的个人资料</h2>
          <hr>
        </main>
      </div>
  ```

  ```js
  $(function($) {
      // 有一个独立的作用域，顺便确保页面加载完成执行
      $('.list-group-item').on('click', function () {
          var url = $(this).attr('href')
          $('#main').load(url + ' #main > *') // 加载指定页面上的具体标签的内容
          return false
      })
  })
  ```

## ajax 全局处理函数

$.ajaxStart(callback)
- **只要有 ajax 请求发生就会执行**

$.ajaxStop(callback)

- **只要有 ajax 请求结束就会执行**
```js
$(document)
    .ajaxStart(function () {
        NProgress.start()
    })
    .ajaxStop(function () {
        NProgress.done()
    })
```

# 其他 AJAX 封装库

[Axios](<http://www.axios-js.com/>)

# 同步请求和异步请求的区别

1. 通过 AJAX 发送的请求都是异步请求，其他都是同步请求
2. 异步请求页面不会刷新，同步请求页面会刷新转圈
3. <font color=#c40>异步请求服务端只返回客户端需要的数据部分，而同步请求返回的是页面模版 + 数据处理好后的结构</font>
4. 不管同步请求还是异步请求，只要是 `get` 请求，服务端获取客户端参数都是通过 rēq.query; 只要是 `post` 请求，服务端获取客户端参数都是通过 rēq.body (需要配置 body-parse)

