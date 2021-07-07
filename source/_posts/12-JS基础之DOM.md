---
title: JS基础之DOM
date: 2016-07-07 13:15:53
updated: 2016-07-07 13:15:53
tags: JS 基础
---
#  DOM介绍

[参考手册](<http://www.w3school.com.cn/htmldom/index.asp>)

Document Object Model 文档对象模型

- DOM 干嘛的? 
  - 就是 **操作网页** 的
  - JS 通过 DOM 来对 HTML 文档进行操作
- 文档
  - 表示的就是整个 HTML 网页文档
- 对象
  - 将网页中的每一个部分都转换为一个对象
- 模型
  - 使用模型来表示对象之间的关系, 方便我们获取对象

<!-- more -->
## 文档的加载

浏览器在加载页面时, 是按照从上到下的顺序加载的, 读取到一行就运行一行

> 如果将 js 代码写到页面的上面, 在代码执行时, 页面还没有加载
> 把 js 代码写到页面最后, 是为了保证页面加载完毕后, 再执行 js 代码

- onload 事件会在 **页面加载完成之后** 才触发
  ```js
  window.onload = function () {
      
  }
  ```

# 节点 (Node)

构成网页的最基本的组成部分, 网页中的每一个部分都可以称为节点

- 常用节点分类
  - 文档节点 —— document (整个 HTML 文档)
  - 元素节点 —— 标签
  - 属性节点 —— 元素的属性
  - 文本节点 —— 标签中的文本内容
  - [节点类型](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)

- 浏览器为我们提供了文档节点(document) , 这个对象是 window 的属性

  document 可以在页面中直接使用, `文档节点代表的是整个网页`

## 获取元素节点API

### 通过 document 对象获取

| API                                            | 说明                                                 |
| ---------------------------------------------- | ---------------------------------------------------- |
| getElementById()                               | 通过 id 获取 **一个** 元素对象                       |
| getElementsByTagName()                         | 通过标签名获取 **一组** 元素对象                     |
| getElementsByName()                            | 通过 name 属性获取 **一组** (*表单*)元素对象         |
| getElementsByClassName()                       | 通过类名获取 **一组** 元素对象                       |
| <font color=#c00>**querySelector()**</font>    | 返回 **一个** 元素对象 (需要 **CSS 选择器作为参数**) |
| <font color=#c00>**querySelectorAll()**</font> | 返回 **一组** 元素对象                               |
| document.body                                  | 获取 body 元素                                       |
| document.documentElement                       | 获取 html 元素                                       |
| document.all                                   | 获取所有元素                                         |
| document.getElementsByTagName('*')             | 同上                                                 |

```js
var inputs = document.getElementsByName('gender')

var div = document.querySelector('.box div')
```

⚠️ querySelectorAll 获取的是静态列表，如果 dom 结构发生改变(*新增元素*)，querySelectorAll 将获取不到任何元素 —— *如果改变 dom 结构，需要再次重新获取*

### 通过具体的元素节点获取

| API                                 | 说明                                            |
| ----------------------------------- | ----------------------------------------------- |
| getElementsByTagName()              | 返回当前节点的 **指定标签名的后代节点**         |
| childNodes (**不常用**)             | 属性, 返回当前节点的所有子节点 (包含文本节点)   |
| children (**常用**)                 | 属性, 返回当前节点的所有子元素 (不包含文本节点) |
| firstElementChild (IE8 不支持)      | 属性, 返回当前节点的第一个子元素                |
| lastElementChild (IE8 不支持)       | 属性, 返回当前节点的最后一个子元素              |
| parentNode                          | 属性, 返回当前节点的父元素                      |
| parentElement                       | 同上                                            |
| previousElementSibling (IE8 不支持) | 属性, 返回当前节点的前一个兄弟元素              |
| nextElementSibling (IE8 不支持)     | 属性, 返回当前节点的后一个兄弟元素              |

```js
var lis = ul.getElementsByTagName('li')

var fir = ul.firstElementChild

var pre = li.previousElementSibling
```

## 增删改节点方法

| API             | 说明                                     |
| --------------- | ---------------------------------------- |
| createElement() | 创建元素节点(需要一个 `标签名` 作为参数) |
| appendChild()   | 向父节点的最后添加新的子节点             |
| insertBefore()  | 将节点插入到指定节点前边                 |
| replaceChild()  | 使用指定的节点替换已有的子节点           |
| removeChild()   | 删除指定的子节点                         |
| remove()        | 删除自身                                 |

```js
document.createElement('li')

父节点.appendChild(子节点)

父节点.insertBefore(新节点, 旧节点)

父节点.replaceChild(新节点, 旧节点)

父节点.removeChild(子节点)

el.remove() // 删除自身
```

### 使用 innerHTML 也可以完成增删改节点操作

```js
ul.innerHTML += '<li>苹果</li>'
```

## 获取/设置元素的属性

- 元素对象.属性名
  ```js
  元素对象.value
  ```

- 表单元素.disabled
  ```js
  input.disabled = true  // 禁用
  input.disabled = false // 启用
  ```

- class 是个例外
  - 读取 class 属性要使用 `元素对象.className`
    ```js
    input.className
    ```

### 类的操作

通过修改 class 属性来修改元素的样式
```js
box.className += ' b2'
```

### 添加/删除类

通过 classList 属性来添加删除类
```js
ele.classList.add('foo')
ele.classList.remove('bar')
```

## 获取元素节点的内部内容

1. 元素对象.innerHTML
   - 只能获取 `非自结束标签 (非单标签) `的内容
2. 元素对象.innerText
   - 只获取文本内容, 如果有标签, 会自动去除 html 标签

# 操作/获取元素样式

| API                                         | 说明                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| 元素.style.样式名 = 值                      | 设置的是 **内联样式**(具有较高的优先级，样式名使用 **驼峰命名法**) |
| 元素.style.样式名                           | 获取的是 **内联样式**, **无法获取到 css 样式表中的样式**，带单位 |
| 元素.currentStyle.样式名 **(只有 IE 支持)** | 获取 `当前显示的样式`; **谁生效获取谁**，只读，不可修改；*如果要修改, 必须通过 style 属性* |
| getComputedStyle()                          | 获取 `当前显示的样式`; window 的方法, 直接使用 **(IE8 不支持)**；获取到的样式是 <font color=#c00>只读的</font>, 不能修改; *如果要修改, 必须通过 style 属性* |

```js
box.style.width = '300px'

// 第一个参数 —— 要获取样式的元素
// 第二个参数 —— 一般传 null
// 返回一个对象, 对象中封装了当前元素的样式
console.log(getComputedStyle(box1, null).width) // 100px
```

```js
// 获取元素样式的兼容方法
function getStyle(element, attr) {
    return window.getComputedStyle ? getComputedStyle(element, null)[attr] : element.currentStyle[attr]
}
```

# 获取 DOM 元素的尺寸&位置

## client 系列 (padding-box)

- clientHeight/clientWidth
  - 获取元素的 **`可见大小`**
  - 返回的是数值, *不带 px*
  - 包含 padding, 不包含 border
  - **只读的, 不能修改**


## offset 系列 (border-box)

- offsetWidth/offsetHeight
  - 获取元素的大小
  - 包含 padding 和 border
- element.offsetParent
  - 获取当前元素的**`定位`**父元素
  - 返回父元素对象
  - 获取到离当前元素最近的 **开启了定位** 的祖先元素
- offsetLeft/offsetTop
  - 获取当前元素相对于其定位父元素的 `偏移量`

## scroll 系列

- scrollWidth/scrollHeight
  - 获取当前元素整个实际的大小

- element.scrollLeft
  - 获取当前元素水平滚动出去的距离

- **element.scrollTop**
  - 一个元素的 `scrollTop` 值是这个元素的 **内容顶部**（卷起来的）到它的视口可见内容（的顶部）的距离的度量
  - 当一个元素的内容没有产生垂直方向的滚动条，那么它的 `scrollTop` 值为 `0`
  - <font color=#c00>当满足 ele.scrollHeight - ele.scrollTop = ele.clientHeight 时, 表示垂直滚动条已经到底了</font>
  - ⚠️ 网页向上卷曲出去的高度问题
    ```js
    var st = document.documentElement.scrollTop
    ```

# 事件

[事件参考](https://www.runoob.com/jsref/dom-obj-event.html)
[事件汇总MDN](https://developer.mozilla.org/zh-CN/docs/Web/Events)
文档或浏览器窗口中发生的一些 **特定的交互瞬间**, 就是用户和浏览器之间的交互行为

- JS 和 HTML 之间的额交互是通过 `事件` 实现的
- 代表性事件
  - 点击、鼠标移入/出、键盘按下......
  - 事件被触发时, 对应的函数执行

## 事件对象

当事件响应函数被触发时, 浏览器每次都会将一个 **事件对象** 作为实参传递给响应函数

- 在事件对象中, 封装了当前事件相关的一切信息, 比如:<font color=#c00>鼠标坐标、键盘哪个按键被按下、阻止事件冒泡、阻止浏览器默认行为、判断触发事件的对象...</font>

### event 对象常用属性

- clientX/clientY
  - 鼠标相对于 **当前可见窗口左上角** 的坐标值
- pageX/pageY (IE8 不支持)
  - 鼠标相对于 **页面左上角** 的坐标值
  ```js
  // 实时获取鼠标的坐标
  areaDiv.onmousemove = function (e) {
      e = e || window.event // 兼容 IE8 写法
      console.log(e) // [object MouseEvent]
      var x = e.clientX
      var y = e.clientY
      
      showMsg.innerHTML = 'x=' + x + ', y=' + y
  }
  ```

- offsetX/offsetY
  - 鼠标相对于 **事件元素左上角** 的坐标值
- target
  - 返回触发此事件的元素
- keyCode
  - 对于 keydown 和 keyup 事件，返回按键编码
- cancelBubble
  - 阻止事件冒泡
- preventDefault()
  - 阻止浏览器默认行为, IE8 不支持
- ⚠️在 IE8 中, 响应函数触发时, 浏览器不会传递事件对象; 而是将事件对象作为 window 的属性保存的

  **window.event**

- 解决兼容性问题
  ```js
  event = event || window.event   // 兼容 IE8 写法
  ```

## 事件冒泡(Bubble)

指的就是事件的向上传导, 当后代的元素被触发时, 其祖先元素的**`相同`**事件也会被触发

- 在触发事件的元素(*后代元素*)的响应事件函数中 **取消冒泡**
  ```js
  event.stopPropagation()
  ```

## 事件委派

将事件绑定给元素共同的 `祖先元素`, 这样当后代元素的事件触发时, 会一直冒泡到祖先元素
从而通过祖先元素的响应函数来处理事件

1. 只绑定一次事件, 即可应用到多个元素上; <font color=#c00>**即使元素是后添加的**</font>
2. 可以减少事件绑定次数, 提高性能
- **event.target**
  - 返回触发事件的对象
  ```js
  ul.onclick = function (event) {
      // 判断触发事件的对象
      if(event.target.className === 'link') {
          alert('111')
      }
  }
  ```

## 事件的绑定

使用 `对象.事件 = 响应函数` 的形式只能为一个元素的同一个事件绑定 **一个** 响应函数

- 如何同时绑定多个?
  - **addEventListener()**  (IE8 不支持)
    - 第一个参数 —— 事件字符串(不带 on)
    - 第二个参数 —— 事件处理函数
    - 第三个参数 —— false (控制事件阶段)
    - 当事件被触发时, 会按绑定顺序执行
    - 回调函数中的 this 是绑定事件的对象
    ```js
    btn.addEventListener('click', function () {
        alert('1')    
    }, false)
    
    btn.addEventListener('click', function () {
        alert('2')    
    }, false)
    ```

  - attachEvent() (IE8)
    - 第一个参数 —— 事件字符串(带 on)
    - 第二个参数 —— 事件处理函数
    - 当事件被触发时, 会按后绑定的先执行
    - 回调函数中的 this 是 window
    ```js
    btn.attachEvent('onclick', function () {
        alert('1')
    })
    btn.attachEvent('onclick', function () {
        alert('2')
    })
    ```

  - 兼容方法
    ```js
    // ele —— 要绑定事件的对象
    // eventStr —— 事件字符串(不带 on)
    // callback —— 回调函数
    function bind(ele, eventStr, callback) {
        if (ele.addEventListener) {
            // 大多数浏览器
            ele.addEventListener(eventStr, callback, false)    
        } else {
            // IE8
            ele.attachEvent('on' + eventStr, function () {
                callback.call(ele)
            })    
        }    
    }
    
    bind(btn, 'click', function () {
        alert('1')
    })
    bind(btn, 'click', function () {
        alert('2')
    })
    ```

## 事件的解绑

dom 0 方式绑定的事件的解绑
```js
btn.onclick = null
```

dom 2 方式绑定的事件的解绑 —— removeEventListener()
```js
this.removeEventListener('transitionend', fn)
```

## 事件的传播

1. 捕获阶段 —— 从外向里
2. 目标阶段 —— 最开始选择的那个元素
3. 冒泡阶段 —— 从里向外

## 加载事件 onload

- onload 
  - 页面/图片加载事件
  - 在页面/图片元素完全 `渲染完毕` 才会执行

## 鼠标事件 onmouse**

- onmousemove 
- onmousedown
- onmouseup
- onmouseenter/onmouseleave

## 鼠标滚轮事件 onwheel

`onwheel` 通常用于处理滑轮的滚动事件

触发条件 —— 鼠标滚轮、触摸板

- onwheel 
  - event.deltaX —— 横向滚动量
  - event.deltaY —— 纵向滚动量
    - 向下滚 > 0
    - 向上滚 < 0

## 滚动事件 onscroll

`onscroll` 用于处理某个对象内容的滚动

触发条件 —— 鼠标滚轮、上下键、拖动滚动条、触摸板

⚠️ 注意点

1. 不要给 body 添加 onscroll 事件
   ```js
   document.body.onscroll = function () {
     console.log(this) // window, 而不是 body, 且 body.scrollTop 的值永远为 0
   }
   ```

2. 获取页面滚动距离时，最好加给 document
   ```js
   document.onscroll = function () {
     console.log('scroll', document.documentElement.scrollTop)
   }
   ```

3. 给其他元素添加 onscroll 事件需要指定高度
   ```css
   .box {
     width: 200px;
     height: 500px;
     border: 1px solid #000;
     overflow: auto;
   }
   p {
     height: 1000px;
     background-color: yellow;
   }
   ```

   ```js
   var box = document.querySelector('.box')
   
   box.onscroll = function () {
     console.log('scroll', this.scrollTop)
   }
   ```

## 拖拽事件

- 拖拽流程
  - 1.鼠标在被拖拽元素上 **`按下`** 开始拖拽
  - 2.鼠标 **`移动`**时, 被拖拽元素跟随鼠标移动
  - 3.鼠标 **`松开`**时, 被拖拽元素固定在当前位置
    ```js
    // 鼠标按下时
    box.onmousedown = function (e) {
        // 设置 box 捕获所有按下事件
        box.setCapture && box.setCapture() // 兼容 IE8
        var distanceX = e.clientX - this.offsetLeft
        var distanceY = e.clientY - this.offsetTop
        // 鼠标拖动时
        document.onmousemove = function (e) {
            e = e || window.event
            var left = e.clientX
            var top = e.clientY
            box.style.left = left - distanceX + 'px'
            box.style.top = top - distanceY + 'px'
        }
        // 鼠标松开时
        document.onmouseup = function () {
            // 取消鼠标拖动事件
            document.onmousemove = null
            // 取消鼠标松开事件
            document.onmouseup = null
            // 取消对事件的捕获
            box.releaseCapture && box.releaseCapture() // 兼容 IE8
        }
        return false
    }
    ```

- 当我们拖拽网页中的内容时, 浏览器会默认去搜索引擎中搜索内容, 此时导致拖拽功能异常, 这是浏览器的默认行为; 如果不希望发生这个行为, 可以通过 return false 来取消默认行为 (IE8 无效)

## 键盘事件 onkey**

键盘事件 <font color=#c00>**一般会绑定给可以获取到焦点的对象(表单)或者 document**</font>

- 可以通过 keyCode 获取按键 Unicode 编码
- onkeydown
  - 键盘按下
  - 如果一直按着不松手, 则事件会一直触发 (*但是第一次会有卡顿,可用定时器解决*) 
    ```html
    <!--定时器解决移动卡顿的例子-->
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
      <style>
        #box {
          width: 100px;
          height: 100px;
          background-color: pink;
          position: absolute;
        }
      </style>
    </head>
    <body>
      <div id="box"></div>
      <script>
        // 获取 box 元素
        var box = document.getElementById('box')
        // 定义方向变量
        var dir = 0
        // 速度
        var speed = 10
    
        // 为 document 绑定键盘按下事件
        document.onkeydown = function (e) {
          e = e || window.event
          // 判断是否按下 ctrl
          if (e.ctrlKey) {
            speed = 200
          } else {
            speed = 10
          }
          // 判断方向
          dir = e.keyCode
        }
    
        // 为 document 绑定键盘松开事件
        document.onkeyup = function () {
          dir = 0
        }
    
        setInterval(function () {
          switch (dir) {
            case 37:
              // alert('左')
              box.style.left = box.offsetLeft - speed + 'px'
              break
            case 38:
              // alert('上')
              box.style.top = box.offsetTop - speed + 'px'
              break
            case 39:
              // alert('右')
              box.style.left = box.offsetLeft + speed + 'px'
              break
            case 40:
              // alert('下')
              box.style.top = box.offsetTop + speed + 'px'
              break
          }
        }, 20)
      </script>
    </body>
    </html>
    ```

- onkeyup
  - 键盘松开
    ```js
    document.onkeydown = function (e) {
        e = e || window.event
       // console.log(e.keyCode)
        if (e.keyCode === 89) {
            console.log ('y 被按下了')
        }
    }
    document.onkeyup = function () {
        
    }
    
    input.onkeydown = function (e) {
        e = e || window.event 
        // 不让输入数字
        if (e.keyCode >=48 && e.keyCode <=57) {
            // 在 input 中输入内容, 属于 onkeydown 的默认行为; 
            return false   // 如果取消了 onkeydown 的默认行为, 则无法在文本框中输入内容
        }    
    }
    ```

- keyCode
  - 按键编码
- altKey
  - alt 是否被按下, 返回 true/false
    ```js
    if(event.altKey && event.keyCode === 89) {
        alert('alt 和 y 都被按下了') 
    }
    ```

- ctrlKey
  - ctrl 是否被按下, 返回 true/false

- shiftKey
  - shift 是否被按下, 返回 true/false

## 右键菜单 oncontextmenu

```js
document.oncontextmenu = function (e) {
  // 自定义右键菜单
  // 1.获取鼠标点的坐标
  。。。
  // 2.设置给自定义菜单元素的 left/top
  ...
  // 3.阻止默认的右键菜单
  return false
}
```

# 浏览器的默认行为

1. a 链接的跳转
2. 右键出现菜单
3. 鼠标复制后搜索

# 阻止浏览器默认行为

阻止 dom0 的默认行为

- **return false** 或 e.preventDefault() 都可以
阻止 dom2(通过 addEventListener 方式绑定的事件) 的默认行为

- **只能使用 e.preventDefault()**
  - IE8 不支持 e.preventDefault
  - <font color=#c40>兼容写法</font>
  ```js
  e.preventDefault && e.preventDefault()
  return false
  ```

# 弹出框

[参考手册](<http://www.w3school.com.cn/jsref/dom_obj_window.asp>)
- confirm('确认删除吗?')
  - 带有确认和取消按钮
  - 点击确认, 返回 true
  - 点击取消, 返回 false
  ```js
  if (confirm('确认删除吗?')) {
      
  } else {
      
  }
  ```

# html 格式字符串转 js 字符串

[html2js](https://www.html.cn/tool/html2js/)

