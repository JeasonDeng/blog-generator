---
title: Zepto.js
date: 2016-07-20 10:54:51
updated: 2016-07-20 10:54:51
tags: Zepto
---
# zepto 入门

zepto 是轻量级的 JavaScript 库, <font color=#c40>专门为移动端定制的 js 库</font>
与 jQuery 有着类似的 API, 俗称: 会 jQuery 就会用 zepto 

[官网](<https://zeptojs.com/>)

[中文 API](<https://www.html.cn/doc/zeptojs_api/>)
<!--more-->

## zepto 的特点

1. 轻量级
2. 针对移动端
3. 响应、执行快
4. 语法 API 大部分同 jQuery 一样, `都是以 $ 为核心函数`
5. 目前 API 完善的框架中 `体积最小` 的一个

# zepto 与 jQuery

## 相同点

## 不同点

### attr 和 prop 的区别

prop 多用在标签对象的固有属性, 布尔值属性. 

attr 多用在自定义属性

- jQuery 中用 attr 获取布尔值属性且布尔值属性在标签上没有显式定义时, 得到的是 `undefined`
- zepto 中用 attr 也可以获取布尔值属性 (对于在标签上没显式定义的布尔值属性, 得到的是 `false`)
  - prop 在读取属性的时候, 优先级高于 attr, `布尔值属性的读取还是建议使用 prop`
  - removeProp() 在 `zepto 1.2+ 才支持`

### DOM 操作的区别

jQuery 中插入 DOM 元素的时候, 添加配置对象(id 、class、等)不起作用
zepto 中插入 DOM 元素的时候, 可以添加配置对象(id 、class、等)
```js
$(function () {
    var $p = $('<p>我是p</p>', {
        id: 'p1'
    })
    $('#box').append($p)
})
```

### each 方法的区别

- jQuery 里的 each
  - 可以遍历数组, 以 `index, item` 的形式
  - 可以遍历对象, 以 `key, value` 的形式
  - 不可以遍历字符串
- zepto 里的 each
  - 可以遍历数组, 以 `index, item` 的形式
  - 可以遍历对象, 以 `key, value` 的形式
  - 可以遍历字符串
  - 可以遍历 json, 以字符串形式遍历

### offset() 方法的区别

- jQuery 里的 offset
  - 获取匹配元素在 `当前视口的相对偏移`
  - 返回的对象包含 **两个** 整型属性 top 和 left 
  - 此方法只对可见元素有效

- zepto 里的 offset
  - 获取匹配元素在 `当前视口的相对偏移`
  - 返回的对象包含 **四个** 整型属性 top 、left 、width(*包含 padding 和 border*) 、height
  - 此方法只对可见元素有效

### width() 和 height() 区别

- jQuery 里获取宽高的方式
  - width() 和 height()
    - **内容区宽高**, 不带单位
  - css('width') 和 css('height')
    - 内容区宽高, 带单位
  - innerWidth() 和 innerHeight()
    - **加上 padding**, 不带单位
  - outterWidth() 和 outterHeight()
    - **加上 padding 和 border**, 不带单位
- zepto 里获取宽高的方式
  - width() 和 height()
    - 根据 `盒模型`来取值, `包含 padding 和 border`
  - 没有 innerWidth()、 innerHeight()、outterWidth() 、 outterHeight()
  - css('width') 和 css('height')
    - **内容区宽高**, 带单位
  - *无法获取隐藏元素的宽高*

### 事件委托

jQuery 里的事件委托

- **on('click', '.demo', function () {})**

zepto 里的事件委托

- 委托的事件先被依次放入数组队列里, 然后由自身开始往后找, 直到找到最后, 期间符合条件的元素委托的事件都会执行

  - 条件1 - 委托在同一个父元素
  - 条件2 - 同一个事件
  - 条件3 - 委托关联, 操作的类要进行关联
  - 条件4 - *从当前位置往后看*
  ```js
  /* 结构 —— box 包含 a 和 b， a 在前边 */
  $('#box').on('touchstart', '.a', function () {
    console.log('点击 a 触发') // 点击 a 会触发 a, b
  })
  $('#box').on('touchstart', '.b', function () {
    console.log('点击 b 触发') // 点击 b 只会触发 b
  })
  ```

# Event

## zepto 同 jQuery 类似的事件

- on()
  - 绑定事件

- off()
  - 移除用 on() 绑定的事件

- bind()
  - 为每个匹配元素的特定事件绑定事件处理函数, 可同时绑定多个事件, 也可以自定义事件

- trigger()
  - 触发有 bind() 定义的事件(通常是自定义事件)
  - 页面加载时触发
    ```js
    $('.btn').bind('myTouch', function () {
        alert('hehe')
    })
    $('.btn').trigger('myTouch')
    ```

  - **或者触发其他元素的事件**
    ```js
    $('.btn').on('click', function () {
        // 触发 p 的点击事件
        $('p').trigger('click')  // or   $('p').click()
    })
    ```

- unbind()
  - bind() 的反向操作, 删除 bind() 绑定的事件

- one()
  - 为匹配元素绑定一个一次性事件, `只执行一次`

## zepto 的 touch 事件

⚠️ <font color=#f40>切记</font>， 在使用 zepto 的 touch 事件之前要先给绑定 touch 事件的元素加上 `touch-action: none;` 否则，无法触发

- tap()
  - 点击事件, 利用在 document 上绑定 touch 事件来模拟 tap 事件的, 并且 tap 事件会冒泡到 document 上

- singleTap()
  - 单击事件
    ```js
    $('.btn').singleTap(function () {
        alert('单击')
    })
    // or
    $('.btn').on('singleTap', function () {
        alert('单击')
    })
    ```

- doubleTap()
  - 双击事件

- longTap()
  - 长按事件, 当按住 `超过 750ms 时`触发

- swipe()
  - swipeLeft() 、swipeRight() 、swipeTop() 、swipeDown() 
  - 滑动事件, 同一方向滑动 `超过 30px 时`触发

- 禁止默认`点击相关`行为
  ```css
  #btn { /* 最好只加给需要操作的元素 */
      touch-action: none; 
  }
  
  
  /* 慎用*/
  * {
    touch-action: none; /* 会禁止所有默认的 touch 事件， 导致页面无法上下滑动*/
  }
  ```

## zepto 的 touch.js 并不好用

在各大浏览器都不支持

⚠️ 使用自定义的 touch.js 来实现滑动事件
```js
// touch.js
(function($) {
    var options, Events, Touch;
    options = {
        x: 20,
        y: 20
    };
    Events = ['swipe', 'swipeLeft', 'swipeRight', 'swipeUp', 'swipeDown', 'tap', 'longTap', 'drag'];
    Events.forEach(function(eventName) {
        $.fn[eventName] = function() {
            var touch = new Touch($(this), eventName);
            touch.start();
            if (arguments[1]) {
                options = arguments[1]
            }
            return this.on(eventName, arguments[0])
        }
    });
    Touch = function() {
        var status, ts, tm, te;
        this.target = arguments[0];
        this.e = arguments[1]
    };
    Touch.prototype.framework = function(e) {
        e.preventDefault();
        var events;
        if (e.changedTouches) events = e.changedTouches[0];
        else events = e.originalEvent.touches[0];
        return events
    };
    Touch.prototype.start = function() {
        var self = this;
        self.target.on("touchstart",
        function(event) {
            event.preventDefault();
            var temp = self.framework(event);
            var d = new Date();
            self.ts = {
                x: temp.pageX,
                y: temp.pageY,
                d: d.getTime()
            }
        });
        self.target.on("touchmove",
        function(event) {
            event.preventDefault();
            var temp = self.framework(event);
            var d = new Date();
            self.tm = {
                x: temp.pageX,
                y: temp.pageY
            };
            if (self.e == "drag") {
                self.target.trigger(self.e, self.tm);
                return
            }
        });
        self.target.on("touchend",
        function(event) {
            event.preventDefault();
            var d = new Date();
            if (!self.tm) {
                self.tm = self.ts
            }
            self.te = {
                x: self.tm.x - self.ts.x,
                y: self.tm.y - self.ts.y,
                d: (d - self.ts.d)
            };
            self.tm = undefined;
            self.factory()
        })
    };
    Touch.prototype.factory = function() {
        var x = Math.abs(this.te.x);
        var y = Math.abs(this.te.y);
        var t = this.te.d;
        var s = this.status;
        if (x < 5 && y < 5) {
            if (t < 300) {
                s = "tap"
            } else {
                s = "longTap"
            }
        } else if (x < options.x && y > options.y) {
            if (t < 250) {
                if (this.te.y > 0) {
                    s = "swipeDown"
                } else {
                    s = "swipeUp"
                }
            } else {
                s = "swipe"
            }
        } else if (y < options.y && x > options.x) {
            if (t < 250) {
                if (this.te.x > 0) {
                    s = "swipeLeft"
                } else {
                    s = "swipeRight"
                }
            } else {
                s = "swipe"
            }
        }
        if (s == this.e) {
            this.target.trigger(this.e);
            return
        }
    }
})(window.jQuery || window.Zepto);
```

# zepto 的使用

实现轮播图
```html
...
    <script src="https://cdn.bootcss.com/zepto/1.2.0/zepto.min.js"></script>
    <script src="js/fx.js"></script>
    <script src="js/touch.js"></script>
    <script src="js/index.js"></script>
</body>
```

```js
$(function () {
  // 轮播图效果
  var screenWidth = $('.sn_banner').width()
  var imgBox = $('.sn_banner').find('ul')
  var points = $('.sn_banner').find('ol>li')

  var index = 1

  var animateFun = function () {
    // 清定时器
    clearInterval(timer)
    imgBox.animate({ transform: 'translateX(' + (-index * screenWidth) + 'px)' }, 300, function () {
      if (index >= 9) {
        index = 1
        imgBox.css({ transform: 'translateX(' + (-index * screenWidth) + 'px)' })
      } else if (index <= 0) {
        index = 8
        imgBox.css({ transform: 'translateX(' + (-index * screenWidth) + 'px)' })
      }
      // 点跟随  
      points.removeClass('active').eq(index - 1).addClass('active')
      // 开定时器
      timer = setInterval(function () {
        index++
        animateFun()
      }, 2000)
    })
  }

  // 自动轮播
  var timer = setInterval(function () {
    index++
    animateFun()
  }, 2000)

  // 左滑
  imgBox.on('swipeLeft', function () {
    index++
    animateFun()
  })

  // 右滑
  imgBox.on('swipeRight', function () {
    index--
    animateFun()
  })
})
```

# ajax

**如何取消一次 ajax 请求?**
- abort() —— 取消当次请求
- abort() 是 xmlHttpRequest 对象的方法

案例

- 场景：点击获取验证码的按钮，用户十秒时候可以再次获取，当十秒过后第一次请求没有返回，用户再次点击获取, 然后因为网速好或者其他原因，两次请求同时返回，该怎么解决?

  // 需要用户可以再次点击的时候取消之前的请求。
  * disabled 禁止用户点击，操作按钮、表单项的时候 `只是针对 click 事件`，并没有对 touch 事件作出处理（*就是不管用*）

    ```js
    $(function () {
        var flag = false // 标识用户是否可点击, false 为可点击
        var xmlHttp = null
    
        $('.btn').on('touchstart', function () {
            if (flag) {
                return
            }
            flag = true // 标识为不可点击
    
            $(this).css('background', 'gray')
            setTimeout(function () {
                $('.btn').css('background', 'red')
                flag = false // 2s 后标识为可点击
            }, 2000)
    
            if (!xmlHttp) { // 判断用户是否发送过请求
                xmlHttp = send() // 发送请求
            } else {
                xmlHttp.abort() // 取消上一次发送的请求
                xmlHttp = send() // 发送新的请求
            }
        })
    
        function send() {
            // ajax 方法调用后返回 xmlHttpRequest 对象
            var xmlHttp = $.ajax({
                type: 'get',
                dataType: 'json',
                url: 'http://localhost:3000/',
                success: function (data) {
                    console.log(data)
                }
            })
            return xmlHttp
        }
    })
    ```
