---
title: DOM位置尺寸API
date: 2016-07-09 11:12:41
updated: 2016-07-09 11:12:41
tags: JS 之 DOM
---
# 获取元素大小

## 元素属性获取

| 属性/API                | 返回值                                                       | 说明           |
| ----------------------- | ------------------------------------------------------------ | -------------- |
| clientWidth             | css 宽 + padding                                             | 只读，不带单位 |
| offsetWidth             | css 宽 + padding + border                                    | 只读，不带单位 |
| scrollWidth             | 如果其子元素大小没有超出，返回值 —— 自身 clientWidth ；如果其子元素大小超出，返回值 —— 其子元素的 offsetWidth | 只读，不带单位 |
| getBoundingClientRect() | 返回包含元素 width / height / left / top / right / bottom 的**对象**；包含 padding, 也包含 border； | 只读，不带单位 |

<!-- more -->
[使用方法](<https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect>)
```js
element.getBoundingClientRect().width

// left / top / right / bottom 都是相对于视口的
element.getBoundingClientRect().left
```

# 获取视口大小

获取视口大小

- **document.documentElement.clientWidth**
- **document.documentElement.clientHeight**

# 位置相关 API

## offsetLeft / offsetTop

元素相对于其 offsetParent `内边距` 的偏移量 

- *如果 offsetParent 有 border, border 不算在内*
- 如果元素自身有 translate, translate 的值也不算在内

> 所有元素都有 offsetLeft / offsetTop, 并不是只有定位的元素才有

### offsetParent

| 情形               | offsetParent                             | 说明                             |
| ------------------ | ---------------------------------------- | -------------------------------- |
| 有定位的祖先元素   | 距离自身**最近的拥有定位属性**的祖先元素 | offsetLeft 计算基于 offsetParent |
| 没有定位的祖先元素 | body                                     | 同上                             |
| 自身为 fixed 元素  | null(Chrome);  body (FireFox)            | offsetLeft 计算基于 **视口**     |

## scrollLeft / scrollTop

这两个属性是 **加给带滚动条的父元素** 的 —— 获取当前元素的内容滚动出去的距离

- 当一个元素的内容没有产生垂直方向的滚动条，那么它的 `scrollTop` 值为 `0`
- <font color=#c00>当满足 ele.scrollHeight - ele.scrollTop = ele.clientHeight 时, 表示垂直滚动条已经到底了</font>

### scrollTop 注意点

1. document.body.scrollTop 永远是 0，要想获取页面滚动出去的距离，使用 **document.documentElement.scrollTop**
2. window 没有 scrollTop 属性 (*jQuery 中有scrollTop 方法：$(window).scrollTop()*)

### 网页向上卷曲出去的高度问题

```js
var st = document.documentElement.scrollTop
```

## scrollTo()

**滚动条滚动到哪里(只对文档流起作用)**
```js
// 滚动到页面底部
// 参数1 —— 水平滚动条位置
// 参数2 —— 垂直滚动条位置
window.scrollTo(0, document.body.scrollHeight)
/* - document.documentElement.clientHeight 可有可无 */
```

## scrollX/scrollY

window 的属性，返回文档在垂直方向已滚动的像素值
```js
window.onscroll = function (e) {
  console.log(window.scrollY)
}
```

# 相对视口的位置

getBoundingClientRect()

- 获取元素的大小 *及相对视口位置*
- 返回值 —— 包含元素 width / height / left / top / right / bottom 的对象
  - left / top / right / bottom 都是相对于**视口**的
    ```js
    element.getBoundingClientRect().left
    ```

## 如何获取绝对位置（相对于页面的位置）？

获取相对于页面的位置分两种情况

1. 页面没有滚动
   此时，相对于页面的位置和相对于视口的位置是一样的，就是 getBoundingClientRect() 的 left 和 top 值

2. 页面发生了滚动
   此时，`相对于页面的位置 = getBoundingClientRect() 的 top 值 + 页面滚动出去的距离`

⚠️ 对于 offsetParent 是 body 的元素而言，ele.offsetTop 就是绝对位置

# 获取鼠标点的坐标（都是 e 的属性）

## clientX/clientY

鼠标相对于 **视口左上角** 的水平/垂直坐标值

## pageX/pageY

鼠标相对于 **页面左上角** 的水平/垂直坐标值

## offsetX/offsetY

鼠标相对于 **触发事件元素左上角** 的水平/垂直坐标值
```js
e.offsetX
```

## screenX/screenY

鼠标相对于 **当前设备屏幕** 的水平/垂直坐标值
![](/images/x:y.png)