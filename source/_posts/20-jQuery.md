---
title: jQuery
date: 2016-07-19 10:15:13
updated: 2016-07-19 10:15:13
tags: jQuery
---
# 初识 jQuery

[官网](<https://jquery.com/>)

- 一个优秀的 JS 函数库, **封装简化 DOM 操作**
- 体积小
- 链式编程
<!--more-->

## jQuery 对象和原生 dom 对象的区别

1. jQuery 对象是封装后的 dom 对象，同样可以操作 dom 元素，只是操作的 API 不同
2. 打印输出 dom 对象，输出的是 html 结构；输出 jQuery 输出的是封装后的类数组对象

## 为什么使用 jQuery?

- 强大的选择器, 可以方便快速查找 DOM 元素
- 隐式遍历, 可以一次操作多个元素
- 读写合一, 读写数据是一个方法
- HTML 元素选取(选择器)
- HTML 元素操作
- CSS 操作
- HTML 事件处理
- JS 动画效果
- <font color=#c00>链式调用</font>
- <font color=#c00>读写合一</font> (同一个方法读写)
- 浏览器兼容
- 易扩展插件
- ajax 封装

## 本质

<font color=#c40>jQuery / $ 对象的本质是一个 `函数`</font>

```js
typeof $ // 'function'
```

## 基本使用

- jQuery 库向外暴露的就是 $ / jQuery
- 引入 jQuery 库后, 直接使用 $ 即可
  - 引入 jQuery 库
    - 可以 `本地引入`
    - 也可以 `远程引入`(有网)

- 使用 jQuery 核心函数
  - $ / jQuery 作为函数使用

- 使用 jQuery 核心对象
  - $ / jQuery 当作对象来使用

# jQuery 的 2 把利器

## jQuery 核心函数

- **当函数用 $()**
  1. 参数为函数 —— 当 DOM 执行完毕后, 执行此回调函数
  ```js
  // 绑定文档加载完成的监听
  $(function () {
  
  })
  ```

  2. 参数为选择器字符串 —— 查找所有匹配的标签, 并将它们封装成 jQuery 对象
  ```js
  $('#btn').click(function () {})
  ```

  3. 参数为 DOM 对象 —— 将 dom 对象封装成 jQuery 对象
  ```js
  $(this).html()
  ```

  4. 参数为 html 标签字符串 —— 动态创建标签对象并封装成 jQuery 对象
    ```js
  $('<h1>标题</h1>').appendTo('div')
    ```

## jQuery 核心对象

### 直接**当对象用 $.xxx()**

- $.each() —— 遍历数组 / 对象 /  伪数组
  > 注意与 $().each() 的区别

  ```js
  // 遍历数组
  $.each([0, 1, 2], function (index, item) {
    
  })
  // 遍历对象
  $.each({ name: 'zs', age: 18 }, function (index, item) {
    
  })
  
  // 遍历 jQuery 对象
  $('div').each(function (i) { // 这里的回调函数没有第二个参数, ⚠️
    console.log(this) // 要通过 this 获取每一次遍历的那个 DOM 对象, 不是 jQuery 对象
  })
  ```

- $.trim() —— 去除两端的空格

  ```js
  $.trim(' my   ')
  ```

- $.ajax({}) —— 发请求

### 或者执行 $('') 返回的对象

- jQuery 对象内部包含的是 dom 元素对象的伪数组 (可能只有一个元素) 
- jQuery 对象拥于很多属性和方法, 能方便地操作 dom

### 常用属性 / 方法

- 基本行为
  - size() / length —— 返回元素个数
  - [index] / get(index) —— 对应位置的 dom 元素
  - each() —— 遍历 jQuery 对象内部的所有的 dom 元素
  - index() —— 获取当前元素的索引 (<font color=#c40>在兄弟元素中的索引</font>)

# jQuery 对象与 js 对象的转换

1. 单个 jQuery 对象  转  js 对象
   ```js
   var btn = $('#btn')[0]
   // or
   var btn = $('#btn').get(0)
   ```

2. jQuery 对象列表  转  js 数组
   ```js
   var arr = $('div').get()
   console.log(Array.isArray(arr)) // true, 是真的数组
   ```

3. js 对象(*不管单个还是多个*)  转   jQuery 对象
   ```js
   // 单个
   var btn = document.querySelector('#btn')
   var $btn = $(btn) 
   
   // 多个
   var divs = document.querySelectorAll('div')
   var $divs = $(divs) 
   ```

# 使用 jQuery 

## jQuery 的选择器(字符串)

- 具有特定语法规则的字符串
  - 用来查找某个或某些 DOM 元素
- 基本选择器
- 层次选择器
- 过滤选择器
  - 在原有选择器基础上进一步过滤 
  - $('div:first')
  - $('div:last')
  - $('div:eq(1)')
  - $('div:not(.box)')
  - $('li:lt(3):gt(0)')
    - 先写 lt, 再写 gt 比较好
  - $('li:contains("hello")')
    - li 的内容中包含 'hello' 的 
  - $('li:hidden')
    - 选中 `display: none`  的 li
  - $('li[title]')
  - $('li[title=hello]')
- 表单选择器
  - $(':text:disabled')
    - 不可用的文本输入框
  - $(':checkbox:checked')
    - 勾选的 checkbox
  - $(':submit')
    - 选中 submit 表单

## $ 对象的常用方法

- $.each() —— 遍历 `数组或对象` 中的数据

- $.trim() —— 去除两端的空格

- $.type(param) —— 得到数据类型
  ```js
  $.type($) // 'function'
  ```

- $.isArray(param) —— 判断是否是数组
  ```js
  $.isArray([]) // true
  ```

- $.isFunction(param) —— 判断是否是函数

- $.parseJSON(json 字符串) —— json 字符串转 js 对象

## 操作 html 属性

- $('div').attr('title')
  - 获取 title 属性值
- $('div').attr('name', 'box')
  - 设置 name 属性值
- $('div').removeAttr('title')
  - 移除 title 属性
- $('div').addClass('box')
  - 添加类
- $('div').removeClass('box')
  - 删除类
- $('div').html()
  - 获取 html 内容
- $('li:first').html('<span>hahah</span>')
  - 设置 html 内容
- $(':text').val()
  - 获取 input 输入框的 value 值
- $(':text').val('hello')
  - 设置 input 输入框的 value 值
- <font color=#c00>$(':checkbox').prop('checked', true)</font>
  - 设置复选框的状态 

### attr() 和 prop()

- attr() —— 专门操作属性值为 `非布尔值` 的属性
  - attribute —— html 元素预定义 / 自定义属性
- **prop() —— 专门操作属性值为 `布尔值` 的属性**
  - property —— js 原生对象身上的属性
    - checked
    - selected
- ⚠️
  - *用户点击时, 浏览器操作的是 property 属性*

## 操作 css

1. 获取 css 属性
   ```js
   $('p').css('color')
   
   // 对比原生 js 获取 css 样式
   getComputedStyle(dom元素, null).color
   ```

2. 设置 css 样式
   ```js
   // 设置单个样式
   $('div').css('background', 'red')
   
   // 设置多个样式
   $('p:eq(1)').css({
       color: '#fff',
       background: 'pink',
       width: 300,  // 可以不加 px
       height: 30
   })
   ```

## 获取/设置标签的位置

- offset() —— 相对 `视口` 左上角的坐标
- position() —— 相对 `父元素` 左上角的坐标
```js
// 获取
var offset = $('#div1').offset()
console.log(offset.left, offset.top) // 整型数据, 不带 px

// 设置
$('#div1').offset({
    left: 100,
    top: 200
})

// 获取
var position = $('#div1').position()
console.log(position.left, position.top) // 整型数据, 不带 px
```

## 设置/获取元素的内容滚动的距离

- scrollTop() —— 设置/获取内容的垂直滚动值
- scrollLeft() —— 设置/获取内容的水平滚动值

```js
// 获取
$('div').scrollTop() // 不带单位
$('html').scrollTop() + $('body').scrollTop() // 兼容 IE

// 设置
$('div').scrollTop(200)
$('html, body').scrollTop(200) // 兼容 IE
```

## 元素的尺寸

- 内容尺寸 —— width / height

  - width()
  - height()
    ```js
    // 获取
    $('p').height() // 不带 px
    
    // 设置
    $('p').height(200)
    ```

- 内部尺寸 —— width + padding

  - innerWidth()
  - innerHeight()

- 外部尺寸 ——  width + padding + border

  - outerWidth()
  - outerHeight()

- 外部尺寸 ——  width + padding + border + margin

  - outerWidth(true)
  - outerHeight(true)

## 筛选元素

### 过滤元素
[参考](<https://api.jquery.com/category/traversing/filtering/>)

- 在 jQuery 对象的元素对象伪数组中过滤出来一部分元素

- first()

- last()

- **eq(index|-index)**  <font color=#c00>eq 是方法, 也是选择器, index 从 0 开始</font>

- filter(selector字符串)
  - selector字符串 **用来描述前边元素的某个属性或者某个状态**
  ```js
  $lis.filter('[title]').filter('[title!=hello]')
  // or
  $lis.filter('[title][title!=hello]')
  ```

- not(selector字符串)
  - selector字符串 **用来描述前边元素里边去掉什么元素**

- has(selector字符串)
  - selector字符串 **用来描述前边元素里边有什么元素**
  ```js
  $lis.has('span')
  ```

### 查找元素

[参考](<https://api.jquery.com/category/traversing/tree-traversal/>)

- children([selector]) —— 子标签中找
  ```js
  $ul.children('span:eq(1)')
  ```

- find(selector) —— 后代标签中找
  ```js
  $ul.find('span:eq(1)')
  ```

- parent() —— 父标签

- prevAll([selector]) —— 前面所有兄弟标签
  ```js
  $('#cc').prevAll('li')
  ```

- nextAll([selector]) —— 后面所有兄弟标签

- siblings([selector]) —— 所有兄弟标签
  ```js
  $('#cc').siblings('li')
  ```

## CRUD(增删改查)

[官网](<https://api.jquery.com/category/manipulation/dom-insertion-inside/>)

- 内部插入
  - append() —— 添加到最后
    ```js
    $ul.append('<li>apple</li>')
    ```

  ```
  - appendTo() —— 同上
  
    ```js
    $('<li>apple</li>').appendTo($ul)
  ```

  - prepend() —— 添加到最前

  - prependTo() —— 同上

- 外部插入

  - before() —— 在指定元素前添加
  - after() —— 在指定元素后添加

- 替换元素

  - replaceWith(html 字符串) —— 替换指定元素
  - replaceAll(selector 字符串) —— 替换指定元素

- 移除元素

  - empty() —— 清空后代元素
  - remove() —— 移除元素

## 事件处理

### 事件绑定与解除

[官网](<https://api.jquery.com/category/events/event-handler-attachment/>)

- on(events, [选择器字符串], [data], 回调函数]) —— 为指定元素绑定事件

  - events —— 事件名, *可以是多个事件名(之间用空格分隔)*
  - data —— 要传递给回调函数的参数, 通过 event.data 来访问
  ```js
  $('a').on('click focus', fn)
  ```

- off() —— 解除指定的事件
  ```js
  $('p').off() // 解除 p 的所有事件
  $('p').off('click') // 解除 p 的 click 事件
  ```

- trigger('事件名') —— 通过 js 代码触发指定事件
  ```js
  $("button").click(function(){ // 点击 button 触发 input 的 select 事件
    $("input").trigger("select")
  })
  
  $(window).on('resize', function () {
    ...
  }).trigger('resize')   // 一上来就触发 window 的 resize 事件
  ```

### 事件相关处理

- 阻止事件冒泡
  - event.stopPropagation()
- 阻止元素默认行为
  - event.preventDefault()

### 区别 mouseover 和 mouseenter

- mouseover / mouseout
  - 在 `移入子元素时也会触发`
    - 也就是说在从外边移入父元素时触发一次, 从父元素移入子元素又触发一次
- mouseenter / mouseleave
  - 只有移入当前元素才触发
    - 只有从外边移入父元素时触发一次, 从父元素移入子元素不再触发
- hover(function (){}, function (){})
  - 使用的是 mouseenter / mouseleave

### 事件委托

- 将多个子元素的事件监听 `委托给父元素` 处理
- 监听回调是加在父元素上
- 当操作任何一个子元素时, 事件会冒泡到父元素
- 父元素不会直接处理事件, 而是根据 `event.target` 得到发生事件的子元素
- 好处
  - 新增的元素直接拥有事件响应处理
  - 减少监听次数
- 用法
  - <font color=#c00>$('ul').on('click', 'li', fn)</font>
  - **fn 回调函数中的 this 是触发事件的子元素**

## 动画效果

### 淡入淡出

- [参考](<https://api.jquery.com/category/effects/fading/>)
- fadeOut([speed], [easing], [fn])
  - speed —— 'slow' / 'fast' / 'normal' / 毫秒数值
  - easing —— 'swing' / 'linear'
  - fn —— 动画完成时的回调
- fadeIn()
- fadeToggle()
  - 改变的是 `透明度(opacity)`

### 滑动动画

- [参考](<https://api.jquery.com/category/effects/sliding/>)
- slideDown()
  - speed —— 'slow' / 'fast' / 'normal' / 毫秒数值
  - easing —— 'swing' / 'linear'
  - fn —— 动画完成时的回调
- slideUp()
- slideToggle()
  - 改变的是 `高度`

### 显示与隐藏

- [参考](<https://api.jquery.com/category/effects/basics/>)
- 显示/隐藏 `默认没有动画`
- show()
  - speed —— 'slow' / 'fast' / 'normal' / 毫秒数值
  - easing —— 'swing' / 'linear'
  - fn —— 动画完成时的回调
- hide()
- toggle()
  - <font color='#c40'>改变宽、高、透明度</font>

### <font color=#c40>自定义动画</font>

[参考](<https://api.jquery.com/category/effects/custom-effects/>)

- animate(properties [, duration ] [, easing ] [, complete ] )
  - 移动到 `指定位置`
  ```js
  $('#div1').animate({
      width: 200,
      height: 200    
  }, 1000)
  
  // 链式调用
  $('#div1')
      .animate({
        width: 200
    })
      .animate({
        left: 50,
        top: 200
    })
  ```

- 移动 `指定的距离`
  ```js
  $('#div1').animate({
      left: '+=50',
      top: '+=50'
  })
  ```

- 停止动画
  - stop()

## 多库共存

[参考](<https://api.jquery.com/jQuery.noConflict/>)

- 如果 2 个库都有 $, 就存在冲突
- jQuery 库可以释放 $ 的使用权, 让另一个库可以正常使用, 此时 jQuery 库只能使用 jQuery 了

- 使用

  ```js
  // js 代码第一行加上
  jQuery.noConflict() // 释放 $ 的使用权
  ```

# onload 和 ready?

window.onload 和 $(document).ready()
- window.onload
  - 包括页面的 *图片加载完后才会回调* (晚)
  - 只能有一个监听回调

- $(document).ready()
  - **等同于 $(function(){})**
  - 页面结构加载完就回调 (早)
  - 可以有多个监听回调

# jQuery 插件

## 自定义插件

[参考](<https://api.jquery.com/category/utilities/>)

- 扩展 jQuery 工具的方法
- $.extend(object)
- 扩展 jQuery 对象的方法
  - $.fn.extend(object)
  ```js
  // myjQuery.js
  (function () {
      // 给 $ 添加方法
      $.extend({
          min: function (a, b) {
              return a < b ? a : b
          },
          max: function (a, b) {
              return a > b ? a : b
          }
      })
      // 给 jQuery 对象添加方法
      $.fn.extend({
          checkAll: function () {
              this.prop('checked', true)
          },
          uncheckAll: function () {
              this.prop('checked', false)
          },
          reverseCheck: function () {
              this.each(function () { // this - jQuery 对象
                  this.checked = !this.checked // this - dom 元素
              })
          }
      })
  })()
  
  // 使用
  $.min(3,5)
  
  $(':checkbox').reverseCheck()
  ```

## 常用插件

- [常用插件](<https://plugins.jquery.com/>)

### [jQuery-validation](<https://www.runoob.com/jquery/jquery-plugin-validate.html>)

- 表单验证插件

+ 程序员只需要声明各种验证规则, 可以自定义验证错误信息

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
      .error { /* 插件会自动给生成的错误信息添加 error 类 */
        color: red;
      }
    </style>
  </head>
  <body>
    <form id="myForm" action="xxx">
      <p>用户名(必须, 最小6位): <input name="username" type="text" required minlength="6"></p>
      <p>密码(必须,6到8位): <input id="password" name="pwd1" type="password" required minlength="6" maxlength="8"></p>
      <p>确认密码(与密码相同): <input name="pwd2" type="password" equalTo="#password"></p>
      <p><input type="submit" value="注册"></p>
    </form>
  
  <script type="text/javascript" src="jquery-1.11.1.js"></script>
  <script type="text/javascript" src="jquery.validate.js"></script>
  <script type="text/javascript">
    /*
        声明式验证: 程序员只需要声明各种验证规则, 可以自定义验证错误信息
     */
  
    // 对此表单开启验证, 定义验证错误信息
    $('#myForm').validate({
      messages: { // 属性名是要验证表单的 name 属性值, 属性值是验证规则名(验证规则名可以在 validate.js 中查找)
        username: {
          required: '用户名是必须的',
          minlength: '用户名至少为6位'
        },
        pwd1: {
          required: '密码是必须的',
          minlength: '密码至少为6位',
          maxlength: '密码最多8位'
        },
        pwd2: {
          equalTo: '必须与密码相同'
        }
      }
    })
  </script>
  </body>
  </html>
  ```

### [jQuery-ui](<https://jqueryui.com/demos/>)

### <font color=#c00>[**laydate (原生 js)**](<https://www.layui.com/laydate/>)</font>
- 日期控件

