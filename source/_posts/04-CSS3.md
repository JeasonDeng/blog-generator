---
title: CSS3
date: 2016-06-15 20:31:50
updated: 2016-06-15 20:31:50
tags: CSS
---
# CSS3 选择器

[选择器汇总](<http://www.w3school.com.cn/cssref/css_selectors.asp>)

<!-- more -->

## 基本选择器

1. id 选择器 (#)
2. 类选择器 (.)
3. 标签选择器 (标签名)
4. 通配符选择器 (*)
5. 后代选择器 (空格)
6. 分组选择器 (,)

## 基本选择器扩展

1. 子元素选择器 (>)
2. 兄弟选择器
   - 相邻 (**紧跟**) 兄弟 (+)

   - 通用 (**后边的所有**) 兄弟选择器 (~)

## 属性选择器(html 属性)

- 存在选择器

  - div[name]
  - *属性名不要加引号*

- 值选择器

  - div[name="hello"]

  - div[name~="hello"]

    - name 属性值中含有 hello, *以空格分隔其他属性值*

      ```css
      div[name~='hehe'] /* <div name="hehe hah"></div>*/
      ```

  - div[name|="hello"]

    - name 属性值是 hello 或者以 hello- 开头

      ```css
      div[name|='hehe'] /* <div name="hehe"></div> or <div name="hehe-hah"></div>*/
      ```

  - div[name^="hello"]

    - name 属性值以 hello 开头

  - div[name$="hello"]

    - name 属性值以 hello 结尾

  - div[name*="hello"]

    - name 属性值包含 hello 

## 伪类与伪元素选择器

### 伪类选择器 (表示标签的某些特殊状态)

- 链接伪类

  - 只作用于 a 元素

  - :link

  - :visited

    - 只有以下属性可以被应用到已访问过的链接
      - color
      - background-color
      - border-color

  - **:target** (使用 css 做选项卡)

    - 代表一个特殊的元素, 它的 id 是 URL 的片段标识符

      ```html
      <style>
        div {
          width: 200px;
          height: 200px;
          background-color: pink;
          display: none;
        }
        :target {
          display: block;
        }
      </style>
      
      <a href="#div1">div1</a>
      <a href="#div2">div2</a>
      <a href="#div3">div3</a>
      
      <div id="div1">div1</div>
      <div id="div2">div2</div>
      <div id="div3">div3</div>
      ```

- 动态伪类

  - :hover
  - :active(点击按住时)

- ⚠️:link 和 :visited 可以覆盖了所有的 a 标签的状态

  - 所以当 :link、:visited、:hover、:active 同时设置时, :link 和 :visited `不能放在最后`

- 表单伪类

  - :enabled
  - :disabled
  - :checked
  - :focus

- 结构性伪类

  - :nth-child(index)   (index 从 1 开始)

    ```css
    /* 查找规则 - 找 #warp 的第一个子元素是不是 ele 标签, 强调顺序， 在子元素中的顺序必须是 1*/
    #wrap ele:nth-child(1){
        
    }
    ```

    - :first-child

    - :last-child

    - :nth-child(even) 

    - :nth-child(odd) 

    - :nth-last-child(index)
      - `从后往前 `数

    - :only-child
      - 唯一的一个子元素

  - :nth-of-type(index)

    ```css
    /* 找到 #wrap 的子元素中的所有的 p,再在所有 p 中找第一个， p 的顺序可能不是第一位的*/
    #wrap p:nth-of-type(1) {
        background: red;    
    }
    ```

    - :first-of-type

    - :last-of-type

    - :only-of-type

    ```css
    /* 子元素中只能有一个 p 标签(可以有多个其他标签) */
    p:only-of-type {
        
    }
    ```

    - ⚠️: nth-of-type(index) 以 **元素属性** 为中心

  - :not

    ```css
    div > a:not(:last-of-type) {
        border-right: 1px solid pink;    
    }
    ```

  - :empty

    - 没有内容(空格都不行)

      ```css
      div:empty {
        background-color: pink;
      }
      ```

### 区分 nth-child 和 nth-of-type 

```html
<style>
  ul li:nth-child(1) {
    color: red; /* 不会选中任何内容， 因为第一个子元素不是 li 类型*/
  }
  
  ul li:nth-of-type(1) {
    color: red; /* 会选中 2 ， 因为 2 是第一个 type 为 li 的子元素*/
  }
</style>

<ul>
  <p>1</p>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ul>
```

### 自定义单选按钮

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    input {
      display: none;
    }
    input + span {
      display: inline-block;
      position: relative;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background-color: #ccc;
    }
    input:checked + span {
      background-color: pink;
    }
    span::after {
      content: "";
      position: absolute;
      left: 5px;
      top: 5px;
      width: 50%;
      height: 50%;
      border-radius: 50%;
      background-color: #ccc;
    }
  </style>
</head>
<body>
 <label>
   <input type="radio" name="sex">
   <span></span>
 </label> 
 <label>
   <input type="radio" name="sex">
   <span></span>
 </label> 
</body>
</html>
```

### 伪元素选择器

- **伪元素不存在 DOM 树上**, 伪元素选择器可以让我们选择到 DOM 以外的元素

- 伪元素选择器
  - ::before
  - ::after
    - ::after 和 ::before `必须有 content 属性`
  - ::first-letter
  - ::first-line
  - <font color=#c00>::selection</font>
    - 选中字体时的状态

### 单冒号和双冒号的区别

- 单冒号 `: ` 用来表示伪类
- 双冒号 `::` 用来表示伪元素

## 声明的优先级

- 选择器的特殊属性由选择器本身的组件确定, 特殊性值表示为 4 个部分
- 一个选择器的具体特殊性如下确定
  1. id 选择器 —— 0, 1, 0, 0
  2. 类选择器 —— 0, 0, 1, 0
  3. **属性选择器 —— 0, 0, 1, 0**
  4. 伪类 —— 0, 0, 1, 0
  5. 元素选择器 —— 0, 0, 0, 1
  6. 伪元素 —— 0, 0, 0, 1
  7. 通配符 —— 0, 0, 0, 0
  8. 内联 —— 1, 0, 0, 0
  9. !important —— 放在分号的前面

# 自定义字体

- 自定义

  - 需要去服务器取, 增大网络负担

  ```css
  @font-face {
      font-family: 'damu';
      src: url(fonts/baohaus93.ttf);
  }
  p {
      font: 12px "damu";
  }
  ```

- FontLab Studio 生成字体工具

- [IcoMoon](<https://icomoon.io/>)

- [Iconfont](<https://www.iconfont.cn/>)

# 新的 UI 方案

## 文本新增样式

- opacity
- rgba()
- 文本阴影
  - 可以加多层
  - text-shadow: 颜色 水平偏移 垂直偏移 模糊度[, 颜色 水平偏移 垂直偏移 模糊度];
- 文字描边
  - 只有 webkit 内核的浏览器才支持
  - -webkit-text-stroke: 4px pink;
- 文字排版
  - direction: ltr/rtl;
  - 要配合 unicode-bidi: bidi-override; 使用
- 溢出显示省略号
  - **text-overflow: ellipsis;**
  - 要配合 **overflow: hidden; white-wrap: nowrap;** 使用
  - 出现省略号的条件 —— 盒子不能由内容撑开

## 元素模糊

- **filter: blur(10px); —— 让整个元素模糊**

  - [使用参考](<https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter>)

    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />
            <title></title>
            <style type="text/css">
                *{
                    margin: 0;
                    padding: 0;
                }
                #wrap{
                    height: 100px;
                    background: rgba(0,0,0,.5);
                    position: relative;
                }
                #wrap #bg{
                    position: absolute;
                    left: 0;
                    right: 0;
                    top: 0;
                    bottom: 0;
                    background: url(img/avatar.jpg) no-repeat;
                    background-size:100% 100% ;
                    z-index: -1;
                    filter: blur(10px);
                }
                img{
                    margin: 24px 0 0 24px;
                }
            </style>
        </head>
        <body>
            <div id="wrap">
                <img src="img/avatar.jpg" width="64px" height="64px"/>
                <div id="bg"></div>
            </div>
        </body>
    </html>
    
    ```

## 盒模型新增样式

### 盒模型阴影

- box-shadow:  水平偏移 垂直偏移 模糊度 [扩散大小] 颜色 [inset];
- 阴影可叠加

### 倒影

- -webkit-box-reflect: left / right / bottom / top [10px];

### resize

- resize: both / horizatal / vertical;
- 要配合 overflow: auto; 使用

### box-sizing

- box-sizing: border-box;
  - width 和 height 包含内容区、内边距和边框

## 新增 UI 样式

### 圆角

- border-radius: 50%/50px[, 50px, 50px, 50px];
- **border-radius: 100px/150px;  椭圆**

### 边框图片

- border-image-source

  - 使用一张图片来代替边框样式

  - 值为 url()

  - 要和 border 搭配使用

    ```css
    border: 30px solid;
    border-image-source: url(border.png);
    ```

- border-image-slice

  - 默认会将图片分割为 9 个区域 - 四个角、四边以及中心区域
  - 值为百分数(基于图片)
  - border-image-slice: 33.33333333% [fill];

- border-image-repeat

  - 平铺
  - border-image-repeat: stretch / round / repeat;

- border-image-width

  - 定义图像边框宽度

- border-image-outset

  - 只能是正值
  - 边框图片外扩距离

### 背景

- background-image
  - 可以指定多背景, 前面的图片会覆盖后面的
  - background-image: url(img1.jpg), url(img2.jpg);
- <font color=#c40>background-position</font>
  - 值可以是 具体的值 / 百分比(<font color=#c40>**参照于背景区域的大小减去背景图片的大小**</font>) / 方位关键词
- background-attachment
  - 值 fixed(相对于视口) / scroll
  - ⚠️值为 fixed 时, **背景图在视口中固定定位, 但只在该元素范围内可见**
- background-origin
  - 背景图默认从 padding-box 开始渲染
  - 取值 border-box / padding-box / content-box
  - ⚠️ 可以和 background-clip 搭配做点击区域大于小图标区域的效果 (小图标来自雪碧图)
- background-clip
  - 从哪开始剪
  - 取值 border-box / padding-box / content-box
  - ⚠️: -webkit-background-clip: text;
    - 背景图剪切到文字内(文字 color 要设置透明)
- background-size
  - 取值
    - background-size: 100%;
    - background-size: 100% 100%;(可能会变形)

### 渐变

#### 线性渐变 (默认从上到下)

- 渐变是背景图属性
  - background-image: linear-gradient(red, green);
  - background-image: linear-gradient(to top, red, green);
  - background-image: linear-gradient(45deg, red, green);
  - background-image: linear-gradient(90deg, red 10%, green 20%);
- 重复渐变
  - background-image: repeating-linear-gradient(90deg, red 10%, green 20%);
- ⚠️角度问题
  - 如果第一个参数是角度
    - 0deg —— 代表从下到上
    - 90deg —— 代表从左到右
    - ......

#### [径向渐变](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient)

- background-image: radial-gradient(red, green);
- background-image: radial-gradient(red 10%, green 30%, pink 50%);
- background-image: radial-gradient(circle, red 10%, green 30%, pink 50%);
- background-image: radial-gradient(circle at 10px 10px, red 10%, green 30%, pink 50%);

# 过渡

- [可做动画的 CSS 属性](<https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties>)
- transition
  - tradition-duration
    - 过渡时间, 以 s / ms 为单位
    - 0s 必须带单位
  - transition-property
    - 默认是 all
  - transition-delay
    - 延迟时间
  - transition-timing-function
    - ease / ease-in / ease-out / linear / ease-in-out / [cubic-bezier()](https://cubic-bezier.com/#.17,.67,.83,.67) / steps(5[, start])
    - steps()
      - 第一个参数: 必须为整数, 指定步数
      - 第二个参数: 指定每一步的值发生的时间点(默认为 end)
  - 当属性值的列表长度不一致时
    - 超出的情况下会被全部截掉
    - 不够的时候, 关于时间的会重复列表, transition-timing-function 的时候使用的是默认值 ease

## 检测过渡是否完成

- 当过渡完成时会触发一个事件, 这个事件是 transitionend
  - 必须通过 dom2 的方式绑定
- 每一个拥有过渡的属性在其完成过渡时都会触发一次 transitionend 事件
  - 有几个过渡属性, transitionend 就会触发几次
- 在过渡完成前设置元素的 display: none, transitionend 事件不会触发

## 过渡注意点

1. transitionend 在元素首次渲染还没有结束的情况下, 是不会触发的

2. 过渡只关心元素的起始状态和结束状态, 没有办法获取元素在过渡中的每一帧

3. 在绝大部分变换样式的切换时, 变换组合的个数或者位置不一样时, 是没有办法过渡的

## 过渡简写

- transition: 过渡样式 过渡时间 [过渡曲线 延迟];
  ```css
  transition: width 2s 3s, height 3s;
  /* height 用 3s 过渡完成后, width 才开始动画 */
  ```

# 2d/3d 变形

## 2D 变形

- 旋转 (rotate)

  - 单位 deg
  - 正值按顺时针方向

- 平移 (translate)

  - 单位 px / %
  - translate 括号内若只有一个值表示的是 X 轴的位移

- 倾斜 (skew)

  - 单位 deg

- 缩放 (scale)

  - 不带单位

- 元素基点 (transform-origin)

  - transform-origin: left top;
  - transform-origin: 50px 50px;
  - transform-origin: 100% 100%;
    - 基于自身大小的百分比

  > ⚠️ transform-origin 的原点是基于元素自身的左上角的

## 2d变换组合

- 变换的顺序 **影响最终结果**
```css
.test:hover {
    transform: translateX(100px) scale(2);
}
/* 结果不同 */
.test:hover {
    transform: scale(2) translateX(100px);
}
```

- ⚠️: 在绝大部分变换样式切换时, <font color=#c40>如果变换函数的位置或个数不一致也不会触发过渡</font>

## 3d 变形

### 景深

- **眼睛到显示器的距离**, 让 3d 场景有近大远小的效果; 

  - 景深越大, 元素离我们越远, 灭点越远, 元素变形越小

- 加给做变换的元素的 `祖先元素`, 作用于后代元素
  ```css
  perspective: 100px;
  ```

- 景深函数

  - `加给自身`, 且 **必须放在首位**
    ```css
    #inner {
      transform: perspective(100px) rotate(0deg);
    }
    
    #wrap:hover #inner {
      transform: perspective(100px) rotate(360deg);
    }
    ```

- 景深基点 (视线角度)

  - 加给做变换的元素的 `祖先元素`, 作用于后代元素

  - 用来控制眼睛位置
    ```css
    perspective-origin: left top;
    ```

  - 默认值 50% 50%

- ⚠️ 要避免景深叠加

### transform-style

- 营造有层次的 3d 舞台 
  - 取值 `preserve-3d`
- 加给做变换的元素的 `父元素`, 作用于子元素

### transform-origin

- 设置元素基点
- transform-origin: center center -50px;
  - 默认值: center center 0;

### 隐藏背面

- backface-visibility
  - hidden  /visible

### 3d 旋转

- transform: rotate3d(1, 1, 1,  360deg);

### 3d 平移

- transform:translate3d(100px, 100px, 100px);
- 第三个参数(translateZ) `不能写百分数`
- **translateZ 需要配合景深才有作用**

### 3d 缩放

- transform:scale3d(1, 1, 2);

# 动画

css3 动画指的是 `元素从一种样式逐渐过渡到另一种样式`

- animation-name
  - 动画名称

- animation-duration

  - 动画时间

- animation-timing-function

  - 动画曲线 - 作用于一个关键帧周期, 而非整个动画周期
  - linear / ease / ease-in / ease-out / ease-in-out / steps(12[, start / end])
  - *steps(12, start) - 看不到第一帧*
  - *steps(12, end) - 看不到最后一帧*

- animation-delay

  - 延迟时间

- animation-iteration-count

  - 动画重复次数 (*动画延迟不会被循环*)
  - 重复的是 `关键帧`
  - 具体数值 / infinite (无限次)

- animation-direction

  - 动画方向
  - normal / reverse (反转的是 `关键帧` 和 `动画曲线`)
  - alternate (往返运动) / alternate-reverse (**必须和 infinite 一起使用才有效**)

- animation-fill-mode

  - **元素的开始位置和结束位置**
  - backfords (元素**开始位置在 from 处**) / forwards (元素结束后**在 to 位置**)
  - both (**开始位置在 from 处, 结束位置在 to 处**)

- animation-play-state

  - 开启 / 暂停动画

  - running (默认值) / paused

    ```css
    #wrap:hover #inner {
      animation-play-state: paused;
    }
    ```

- @keyframes 
  - 关键帧

  ```css
  @keyframes move {
      from {
          transform: rotate(0deg);
      }
      to {
          transform: rotate(360deg);
      }
  }
  
  /* 百分数分的是时间 */
  @keyframes move {
      0% {
          transform: translateY(-100px);
      }
      50% {
          transform: translateY(-50px);
      }
      100% {
          transform: translateY(100px);
      }
  }
  ```

- 简写
  - animation: 动画名称 动画时间 动画曲线 动画延迟 循环次数 动画方向 状态保持 启动暂停;
  - animation: move 3s ease 1s infinite alternate forwards;

## 运用 steps() 来做小动画

1. 数小图片有几帧 —— n
2. 看图片的尺寸 —— w
3.
```css
@keyframes move {
     from {
       background-position: 0 0;
     }
     to {
       background-position: -w 0; /* 这里就是图片尺寸, 不需要减去一帧的宽度; 动画实际有 n+1 帧*/
     }
   }
```

4.
```css3
.icon {
    background: url(img.png) no-repeat;
     background-position: 0 0;
     animation: move 1s steps(n) infinite;
   }
```

# 媒体查询

<font color=#c40>响应式布局的核心</font>
```css
@media screen {

  /* 规则 - 在某些条件下才起作用 */

}
```

- 媒体查询一般 `写在 css 最下面`
- 媒体类型

  - all
  - screen
  - print

## 媒体属性

- width /  height / device-width / -webkit-device-pixel-ratio / orientation(横/竖屏)

  - width /  height / device-width / -webkit-device-pixel-ratio 都可加前缀 (min- / max-)

  - orientation: portrait / landscape;
  ```css
  /* :后的值不要加引号 */
  @media (orientation: portrait) {
      #wrap {
          border: 10px solid;
      }
  }
  
  @media (min-width: 1000px) {
      #wrap {
          border: 10px solid;
      }
  }
  ```

## 关键字

- and 
  - 与, and 用来连接媒体类型和媒体属性
    ```css
    /* 设备像素比为2 且 为竖屏*/
    @media (-webkit-device-pixel-ratio: 2) and (orientation: portrait) {
        #wrap {
            border: 10px solid;
        }    
    }
    ```

- only

  - 老版本使用, 让老版本浏览器不响应

- ,
  - 或
    ```css
    /* 设备像素比为2 或者 为竖屏*/
    @media (-webkit-device-pixel-ratio: 2), (orientation: portrait) {
        #wrap {
            border: 10px solid;
        }    
    }
    ```

- not
  - 取反
    ```css
    /* 不是像素比为2的设备 或者 为竖屏*/
    @media not (-webkit-device-pixel-ratio: 2) , (orientation: portrait) {
        #wrap {
            border: 10px solid;
        }    
    }
    ```

# 规范

1. HTML 规范
2. JS 规范
   - [ECMA 规范](<http://www.ecma-international.org/publications/standards/Ecma-262-arch.htm>)
   - [DOM 规范](<https://www.w3.org/DOM/>)
     - DOM 是独立于平台和语言的借口, 该借口为程序和脚本提供了对文档的内容、结构和样式的动态获取和更新的功能
     - BOM 没有规范
3. [CSS 规范](<https://www.w3.org/Style/CSS/>)



