---
title: JS基础之BOM
date: 2016-07-12 14:27:27
updated: 2016-07-12 14:27:27
tags: JS 之 BOM
---
# BOM 

浏览器对象模型

- BOM 使我们可以 `通过 JS 操作浏览器`
- 在 BOM 中为我们提供了一组对象, 用来完成对浏览器的操作
  - 这些对象在浏览器中 *都是作为 window 对象的属性* 保存的
  - 可以通过 window 点出来, 也可以直接使用

[参考手册](<http://www.w3school.com.cn/jsref/dom_obj_window.asp>)
<!-- more -->

# window 对象

代表的是整个浏览器的窗口, 同时 window 也是网页中的全局对象
```js
console.log(window)
```

# navigator 对象

代表当前浏览器的信息, 通过该对象可以识别不同的浏览器
- userAgent 属性
  - 判断浏览器信息
  - 返回一个 `字符串`

- 如果通过 userAgent 不能判断, 还可以通过一些浏览器中特意的对象来判断
  - eg:  ActiveXObject
  ```js
  // 判断浏览器
  var ua = navigator.userAgent
  if (/firefox/i.test(ua)) {
      alert('火狐')
  } else if (/chrome/i.test(ua)) {
      alert('谷歌')
  } else if ('ActiveXObject' in window) {
      alert ('IE')
  }
  
  // 判断 window 对象上是否有 ActiveXObject 属性
  if ('ActiveXObject' in window) {
      alert('你是 IE')
  }
  ```

# location 对象

代表 `当前浏览器的地址栏信息`

- 通过 location 可以 <font color=#c40>获取地址栏信息</font>, 或者操作浏览器跳转页面
- 该对象中封装了浏览器的地址栏信息, 直接打印 loaction , 可以获取到当前页面的完整路径
  - `location.search` 可以 **获取查询字符串信息**
- 如果直接将 location 修改为一个绝对路径或相对路径, 页面会自动跳转到该页面
- assign()
  - 跳转到其他页面, 作用和直接修改 location 一样
  - 会生成历史记录, 能回退
    ```js
    location = 'http://www.baidu.com'
    // or
    location.assign('http://www.baidu.com')
    ```

- reload()
  - 重新加载当前文档 **(刷新)**
    ```js
    location.reload()     // 刷新当前页面
    
    location.reload(true) // 强制清空缓存刷新
    ```

- replace()
  - 使用新的页面替换当前页面
  - 不会生成历史记录, 不能回退
    ```js
    location.replace() 
    ```

# history 对象

代表 `浏览器的历史记录`

- 可以通过该对象来操作浏览器的历史记录
- 只能操作浏览器向前 / 后翻页, 而且该操作只在当次访问时有次
- length 属性
  - 返回当次访问的链接的数量
    ```js
    history.length
    ```

- back()
  - 回退到上一个页面
    ```js
    history.back()
    ```

- forward()
  - 跳转到下一个页面
    ```js
    history.forward()
    ```

- <font color=#c00>go()</font>
  - 可以用来跳转到 `指定页面`
  - 需要一个整数作为参数
    - 1 —— 向前跳转 1 个页面;     2 —— 向前跳转 2 个页面; 
    - -1 —— 向后跳转 1 个页面;    -2 —— 向后跳转 2 个页面;
    ```js
    history.go(-1)
    ```

# screen 对象

代表用户的屏幕信息
- 通过该对象可以获取到用户显示器的相关信息

