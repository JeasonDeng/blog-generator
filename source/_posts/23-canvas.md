---
title: canvas
date: 2016-07-22 09:59:04
updated: 2016-07-22 09:59:04
tags: canvas
---
# canvas
canvas 是 HTML5 新增的元素, 可以通过 js 中的脚本来 `绘制图形、创建动画`

- canvas 元素具有默认的宽高
  - width: 300px;  height:150px;

- canvas 标签的 width / height (<font color=#c40>**不能在 css 中指定**</font>)
  - width
  - height
  - **指定宽高只能在标签内指定 或者 js 设置**
    ```html
    <canvas id="canvas" width="300" height="150"></canvas>
    ```
    ```js
    canvas.width = document.documentElement.clientWidth
    canvas.height = document.documentElement.clientHeight
    window.onresize = function () {
        canvas.width = document.documentElement.clientWidth
        canvas.height = document.documentElement.clientHeight
    }
    ```

- 替换内容
  - 支持 canvas 的浏览器会忽略容器中的替换内容
  - 不支持的浏览器会显示替换内容
    ```html
    <canvas>
        <span>您的浏览器不支持canvas</span>
    </canvas>
    ```
<!--more-->

# canvas 基本用法

[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

## 渲染上下文（画笔）

canvas 元素只是创造一个固定大小的画布, 要想在上面去绘制内容, 我们需要找到它的渲染上下文

- canvas 元素有一个叫做 getContext() 的方法, 这个方法是用来获得渲染上下文的
  - getContext() 只有一个参数 —— 模式
    ```js
    var canvas = document.querySelector('canvas')
    if (canvas.getContext) {
        var ctx = canvas.getContext('2d')
    }
    ```

## 绘制矩形

HTML 中的 canvas 只支持一种原生的图形绘制 —— 矩形

- 所以其他图形的绘制都 *至少需要生成一条路径*

canvas 为画笔提供了三种绘制矩形的方法

1. fillRect(x, y, width, height) —— 填充矩形
   - 默认填充色为黑色

2. strokeRect(x, y, width, height) —— 边框矩形
   - 默认边框为一像素实心黑色线
   - 边框宽度是 `平分在偏移位置的两侧`
     ```js
     ctx.strokeRect(100, 100, 100, 100 ) // 边框线会渲染在 99.5 - 100.5 之间, 渲染为 2px
     
     ctx.strokeRect(100.5, 100.5, 100, 100 ) // 边框线会渲染在 100 - 101 之间, 渲染为 1px
     ```

3. clearRect(x, y, width, height) —— 清除指定矩形区域

  >  x, y 是相对于画布左上角, 不能带单位, 直接写数值
  >
  >  canvas 上的内容 css 无法拿到, 只能通过 canvas 的 API 来设置

## 添加样式和颜色

⚠️ 添加样式要在绘制之前才会生效

| 属性 API    | 使用示例                                   | 说明               |
| ----------- | ------------------------------------------ | ------------------ |
| fillStyle   | ctx.fillStyle = 'pink'                     | 填充色             |
| strokeStyle | ctx.strokeStyle = 'pink'                   | 边框色             |
| lineWidth   | ctx.lineWidth = 10                         | 线宽               |
| lineJoin    | ctx.lineJoin = 'round' / 'miter' / 'bevel' | 圆角 / 直角 / 斜角 |
| lineCap     | ctx.lineCap = 'round' / 'miter' / 'bevel'  | 线段末端属性       |

## 绘制路径

图形的基本元素是路径

- 路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合

绘制步骤

1. 创建路径起点
   - moveTo(x, y)
     - 将笔触移动到指定坐标
     - 当 canvas 初始化或者 beginPath() 调用后, 通常会使用 moveTo() **设置起点**
2. 使用画图命令去画路径
   - lineTo(x, y)
     - 绘制一条从当前位置到指定位置的直线
3. 封闭路径
   - closePath()
     - 闭合路径
4. 一旦路径生成, 你就能通过描边或填充去渲染图形
   - stroke()
     - 路径描边
   - fill()
     - 路径填充
     - 会 `自动闭合` 路径, *使用 fill() 时, 不需要使用 closePath()*

5. 开启新路径
   - beginPath()
     - 开启新路径

### 路径形式绘制矩形

rect(x, y, width, height)
```js
ctx.rect(10, 10, 100, 100)
ctx.fill()
```

## 基本模版

多个图形 `互不影响` 的写法

- **一个 save() 对应一个 restore()**
  <font color=#c40>相当于一个作用域</font>

```js
ctx.save()
// 关于样式的设置
......
ctx.beginPath()
// 关于路径
......
ctx.restore()

ctx.save()
// 关于样式的设置
......
ctx.beginPath()
// 关于路径
......
ctx.restore()
```

### 路径容器、样式容器和样式栈

- 路径容器
  - 每次调用路径 api 时, 都会往路径容器里做登记
  - 调用 beginPath() 时, 清空整个路径容器
- 样式容器
  - 每次调用样式 api 时, 都会往样式容器里做登记
  - 调用 save() 时, 将样式容器里的状态压入样式栈
  - 调用 restore() 时, 将样式栈的栈顶状态弹出到样式容器里进行覆盖
  - <font color=#c40>路径显示的样式是当前样式容器的状态</font>
- 样式栈
  - 调用 save() 时, 将样式容器里的状态压入样式栈
  - 调用 restore() 时, 将样式栈的栈顶状态弹出到样式容器里进行覆盖

## 绘制曲线

1. 绘制圆(弧)
   - arc(x, y, radius, startAngle, endAngle, anticlockwise)
     - x, y —— 圆心
     - radius —— 半径
     - startAngle, endAngle —— 开始结束 `弧度`(以 x 轴为基准)
     - anticlockwise —— true (逆时针) / false (顺时针: 默认)
   ```js
   // 扇形
   var canvas = document.querySelector('canvas')
     if (canvas.getContext) {
       var ctx = canvas.getContext('2d')
   
       ctx.moveTo(100, 100)
       ctx.arc(100, 100, 50, 0, 90 * Math.PI / 180[, false])
       ctx.closePath()
       ctx.stroke()
     }
   ```

2. 绘制弧线
   - arcTo(x1, y1, x2, y2, radius)
     - 根据给定的点和半径画一段弧线
     - 需要三个控制点
   ```js
   // 弧线 - (50, 50) (200, 50) (200, 100) 三点形成的角的内切弧线
   ctx.beginPath()
   ctx.moveTo(50, 50)
   ctx.arcTo(200, 50, 200, 100, 50)
   ctx.stroke()
   ```

3. 二次贝塞尔
   - quadraticCurveTo(cp1x, cp1y, x, y)
     - 需要 3 个控制点
     - cp1x, cp1y —— 第一个控制点
     - x, y —— 结束点
     - 起点为 moveTo() 指定的点
     > ⚠️ 曲线肯定会过起点和结束点

4. 三次贝塞尔
   - bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
     - 需要 4 个控制点
     - cp1x, cp1y —— 控制点1;  cp2x, cp2y —— 控制点2; 
     - x, y —— 结束点
     - 起点为 moveTo() 指定的点
     > ⚠️ 曲线肯定会过起点和结束点
   ```js
   ctx.beginPath()
   ctx.moveTo(50, 50)
   ctx.quadraticCurveTo(300, 0, 200, 100)
   ctx.stroke()
   ```

# canvas 中的变换样式

> 都是 ctx 的样式

- translate(x, y)
  - 用来移动 canvas 的 **`原点`** 到一个不同的位置
  - 在 canvas 中的 translate 是累加的
    ```js
    ctx.translate(50, 50)
    ctx.translate(50, 50)
    // 最终， 原点在 (100, 100) 的位置
    ```

- rotate(x, y)
  - 顺时针方向, 以弧度为单位 (eg: rotate(45 * Math.PI / 180))
  - **旋转的是 canvas 坐标轴**
  - 在 canvas 中的 rotate 是累加的

- scale(x, y)
  - 对形状、图形进行缩小或放大 (*坐标位置也会缩放*)
  - 在 canvas 中的 scale 是累加的

  ```html
  <!DOCTYPE html>
  <html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            *{
                margin: 0;
                padding: 0;
            }
            html,body{
                height: 100%;
                overflow: hidden;
            }
            body{
                background: pink;
            }
            #test{
                background: gray;
                position: absolute;
                left: 0;
                top: 0;
                right: 0;
                bottom: 0;
                margin: auto;
            }
        </style>
    </head>
    <body>
        <canvas id="test" width="300" height="300">
            <span>您的浏览器不支持画布元素 请您换成萌萌的谷歌</span>
        </canvas>
    </body>
    <script type="text/javascript">
        window.onload=function(){
            var flag =0;
            var scale = 0;
            var flagScale = 0;
            var canvas = document.querySelector("#test");
            if(canvas.getContext){
                var ctx = canvas.getContext("2d");
                
                ctx.save();
                ctx.translate(150,150);
                
                ctx.beginPath();
                ctx.fillRect(-50,-50,100,100);
                ctx.restore();
                
                setInterval(function(){
                    flag++;
                    ctx.clearRect(0,0,canvas.width,canvas.height);
                    ctx.save();
                    ctx.translate(150,150);
                    ctx.rotate(flag*Math.PI/180);
                    
                    
                    if(scale==100){
                        flagScale=-1;
                    }else if(scale==0){
                        flagScale=1;
                    }
                    scale+=flagScale;
                    ctx.scale(scale/50,scale/50);
                    
                    ctx.beginPath();
                    ctx.fillRect(-50,-50,100,100);
                    ctx.restore();
                },10)
            }
        }
                
    </script>
  </html>
  ```

# 在 canvas 中设置背景

- createPattern(image, repetition)

  - image —— 图像源

  - repetition

    - 'repeat'
    - 'repeat-x'
    - 'repeat-y'
    - 'no-repeat'

  - 一般情况下, 我们会将 createPattern 返回的对象作为 fillStyle 的值

    ```js
    var img = new Image()
    img.src = 'timg.jpg'
    img.onload = function () {
        draw()
    }
    function draw() {
        var pattern = ctx.createPattern(img, 'repeat')
        ctx.fillStyle = pattern
        ctx.fillRect(0, 0, 400, 400)
    }
    ```

# canvas 渐变

- 线性渐变
  - createLinearGradient(x1, y1, x2, y2)
    - x1, y1, x2, y2 —— 渐变的起点和终点 (控制渐变方向)

  - addColorStop(position, color)
    - position —— 0-1 之间的数值, 表示渐变中颜色所在的相对位置
    - color —— 颜色值
    ```js
    var gradient = ctx.createLinearGradient(0, 0, 400, 400)
    gradient.addColorStop(0, 'red')
    gradient.addColorStop(0.5, 'yellow')
    gradient.addColorStop(1, 'green')
    ctx.fillStyle = gradient
    ctx.fillRect(0, 0, 400, 400)
    ```

- 径向渐变
  - createRadicalGradient(x1, y1, r1, x2, y2, r2)
    - x1, y1, r1 —— 第一个(x1, y1) 为原点, 半径为 r1 的圆
    - x2, y3, r2 —— 另一个(x2, y2) 为原点, 半径为 r2 的圆

- ⚠️ 渐变色在 r1 ~ r2 之间 (*0 ～ r1 区间和 > r2 区间都是纯色区域* )
  ```js
  var gradient = ctx.createRadialGradient(200, 200, 0, 200, 200, 200)
  gradient.addColorStop(0, 'red')
  gradient.addColorStop(0.5, 'yellow')
  gradient.addColorStop(1, 'green')
  ctx.fillStyle = gradient
  ctx.fillRect(0, 0, 400, 400)
  ```

# canvas 中绘制文本

- canvas 提供了两种方法来渲染文本
  - fillText(text, x, y)
    - 在指定的 (x, y) 位置填充指定的 text
  - stroke(text, x, y)
    - 在指定的 (x, y) 位置绘制文本边框

- 文本样式
  - font = value
    - 默认的字体样式是 10px sans-serif
    - font 属性在指定时, <font color=#c40>必须要有大小和字体缺一不可</font>
      ```js
      var ctx = canvas.getContext('2d')
      ctx.fillStyle = 'pink'
      ctx.font = '30px sans-serif'
      ctx.fillText('健哥哥', 200, 200)
      ```

  - textAlign = 'left' / 'right' / 'center'
    - 文本的居中是基于你在 fillText 时指定的 x 的值, 也就是说一般文本在 x 的左边, 一半文本在 x 的右边

- 文本尺寸
  - measureText(text)
    - 返回一个 TextMetrics 对象, 包含关于文本尺寸的信息
  - *返回的对象只有 width 属性*

- **文本的水平垂直居中**
  ```js
  if (canvas.getContext) {
      var ctx = canvas.getContext('2d')
      ctx.fillStyle = 'pink'
      ctx.font = '30px sans-serif'
      ctx.textBaseline = 'middle'
      var w = ctx.measureText('健哥哥').width
      ctx.fillText('健哥哥', (canvas.width - w) / 2, (canvas.height - 30) / 2)
    }
  ```

# 在 canvas 中插入图片

- canvas 操作图片时, **必须等图片加载完才能操作**
- drawImage(image, x, y, width, height)
  - image —— 是 image 对象或者 canvas 对象 (canvas 本身就是图片, 可以右键另存为)
  - x, y —— 是其在 canvas 中的起始坐标
  - width, height —— 是其在 canvas 中的宽高, *如果不传宽高，默认采用图片尺寸*
    ```js
    if (canvas.getContext) {
        var ctx = canvas.getContext('2d')
        
        var value = 0
        var flag = 0
        setInterval(function () {
          value+= 10
          flag++
          if (flag > 8) {
            flag = 1
          }
          var img =  new Image()
          img.src = 'feiniao_' + flag + '.jpg'
          img.onload = function () {
            draw(this)       
          }     
        }, 10)
      
        function draw (img) {
          ctx.clearRect(0, 0, canvas.width, canvas.height)
          ctx.drawImage(img, value, 0)
        }
      }
    ```

# canvas 中的像素操作

- 可以直接通过 ImageData 对象操纵像素数据, 直接将读取或数据数组写入该对象中
- getImageData(x, y, w, h)
  - 获得一个包含画布场景像素数据的 ImageData 对象, 它代表了画布区域的对象数据
  - x, y —— 将要被提取的图像数据区域的左上角坐标
  - w, h —— 将要被提取的图像数据区域的宽高

- ImagData 对象
  - ImagData 对象中存储着 canvas 对象真实的像素数据, 它包含以下几个属性
    - width
    - height
    - data —— Unit8ClampedArray 类型的一维数组
      - RGBA 格式的数据类型, 范围在 0 - 255 之间
      - A 的值也是 0 - 255 之间

- putImageData(ImageData, x, y)
  - 对场景进行像素数据写入
  - ImageData —— ImagData 对象
  - x, y —— 要绘制像素数据的坐标
  ```js
  if (canvas.getContext) {
      var ctx = canvas.getContext('2d')
      ctx.fillRect(0, 0, 100, 100)
  
      // 获取要操作的像素点区域
      var imageData = ctx.getImageData(0, 0, 100, 100)
      // 修改像素点数据
      for(var i = 0; i < imageData.data.length; i++) {
        imageData.data[4 * i + 3] = 50
      }
      // 写入像素数据
      ctx.putImageData(imageData, 0, 0)
    }
  ```

- createImageData(w, h)
  - w,h —— ImageData 新对象的宽高
  - 默认创建出来的是 *黑色透明的*
    ```js
    if (canvas.getContext) {
        var ctx = canvas.getContext('2d')
        ctx.fillRect(0, 0, 100, 100)
    
        // 获取要操作的像素点区域
        var imageData = ctx.createImageData(100, 100)
        // 修改像素点数据
        for(var i = 0; i < imageData.data.length / 4; i++) {
          imageData.data[4 * i + 3] = 50
        }
        // 写入像素数据
        ctx.putImageData(imageData, 100, 100)
      }
    ```

# canvas 合成

## 全局透明度的设置

- ctx.globalAlpha = value
  - 这个属性影响到 canvas 里所有图形的透明度
  - 有效值范围是 0 - 1

## 覆盖合成

- source - 新的图像(源)
- destination - 已经绘制过的图形(目标)
- globalCompositeOperation
  - source-over (默认值) —— 源在上面, 新的图像层级比较高
  - source-in —— 只留下源与目标的重叠部分(源的那一部分)
  - source-out —— 只留下源超过目标的部分
  - source-atop —— 砍掉源溢出的部分
  - destination-over —— 目标在上面, 旧的图像层级比较高
  - destination-in —— 只留下源与目标的重叠部分(目标的那一部分)
  - destination-out —— 只留下目标超过源的部分
  - destination-atop —— 砍掉目标溢出的部分

# canvas 其他

## 将画布导出为图像

- toDataURL (注意是 canvas 元素接口上的方法)
  ```js
  var ret = canvas.toDataURL()
  console.log(ret) // ret 是一个图像地址
  ```

## 事件操作

- ctx.isPointInPath(x, y)
  - 判断在当前路径中是否包含监测点
  - x, y —— 检测点坐标
  - ⚠️ - 此方法只作用于最新画出的路径上
    ```js
    // 点此路径 - 不会触发事件
    ctx.beginPath()
    ctx.arc(100, 100, 50, 0, 360 * Math.PI /180)
    ctx.fill()
    // 点此路径 - 会触发事件， 因为 beginPath 清除了之前的路径
    ctx.beginPath()
    ctx.arc(200, 200, 50, 0, 360 * Math.PI /180)
    ctx.fill()
    canvas.onclick = function (e) {
        e = e || event
        var x = e.clientX - canvas.offsetLeft
        var y = e.clientY - canvas.offsetTop
        if (ctx.isPointInPath(x, y)) {
            alert('hehe')
        }    
    }
    ```

# 总结

注意点

- canvas 图像的渲染有别于 html 图像的渲染，
  - canvas 的渲染极快，不会出现代码覆盖后延迟渲染的问题
- 写 canvas 代码一定要具有同步思想
  - 在获取上下文时，一定要先判断
- 画布高宽的问题
  - 画布默认高宽 300*150
  - 切记一定要使用 html 的 attribute 的形式来定义画布的宽高
  - 通过 css 形式定义会缩放画布内的图像
- 绘制矩形的问题
  - a.边框宽度的问题，边框宽度是在偏移量上下分别渲染一半，可能会出现小数边框，
    一旦出现小数边框都会向上取整
  - b.canvas 的 api 只支持一种图像的直接渲染：矩形
- 我们没法使用 css 选择器来选到 canvas 中的图像

## 画布 api

- oc.getContext("2d") —— 获取画布的 2d 上下文(画笔)
- oc.width —— 画布在横向上 css 像素的个数
- oc.height —— 画布在纵向上 css 像素的个数
- oc.toDataUrl() —— 拿到画布的图片地址

## 上下文 api

- ctx.fillRect(x,y,w,h) —— 填充矩形
- ctx.strokeRect(x,ymwmh) —— 边框的矩形
- ctx.clearRect(0,0,oc.width,oc.height) —— 清除整个画布
  - 注意原点的位置
- ctx.fillStyle:
  - 可以是填充颜色
  - 背景 fillStyle 的值可以是 createPattern(image, repetition) 返回的对象
  - 线性渐变 fillStyle 的值可以是 createLinearGradient(x1, y1, x2, y2)) 返回的对象
    - addColorStop(position, color)
  - 径向渐变 fillStyle 的值可以是 createRadialGradient(x1, y1, r1, x2, y2, r2) 返回的对象
    - addColorStop(position, color)
- ctx.strokeStyle —— 线条颜色
- ctx.lineWidth —— 线条宽度
- ctx.lineCap —— 线条两端的展现形式
- ctx.lineJoin —— 线条连接处的展现形式
- ctx.moveTo(x,y) —— 将画笔抬起点到x，y处
- ctx.lineTo(x,y) —— 将画笔移到x，y处
- ctx.rect(x,y,w,h) —— 路径矩形
- ctx.arc(x,y,r,degS,degE,dir) —— 圆
- ctx.arcTo(x1,y1,x2,y2,r) —— 弧线
  - 结合 moveTo(x,y) 方法使用，
  - x,y:起始点
  - x1,y1：控制点
  - x2,y2：结束点
- ctx.quadraticCurveTo(x1,y1,x2,y2) —— 二次贝塞尔
  - 结合 moveTo(x,y) 方法使用，
  - x,y:起始点
  - x1,y1：控制点
  - x2,y2：结束点
  - 必须经过起点和终点
- ctx.bezierCurveTo(x1, y1, x2, y2, x3, y3) —— 三次贝塞尔
  - 结合moveTo(x,y)方法使用，
  - x,y:起始点
  - x1,y1：控制点
  - x2,y2：控制点
  - x3，y3：结束点
  - 必须经过起点和终点
- ctx.fill() —— 填充
- ctx.stroke() —— 描边
- ctx.beginpath() —— 清除路径容器
- ctx.closepath() —— 闭合路径
  - fill自动闭合
  - stroke需要手动闭合
- ctx.save()
  - 将画布当前状态 <font color=#c40>(样式相关 变换相关) </font>压入到样式栈中
- ctx.restore()
  - 将样式栈中栈顶的元素弹到样式容器中
  - 图像最终渲染依赖于样式容器
- ctx.translate(x,y) —— 将原点按当前坐标轴位移x，y个单位
- ctx.rotate(弧度) —— 将 **坐标轴** 按顺时针方向进行旋转
- ctx.scale(因子):
  - 放大：放大画布，画布中的一个css像素所占据的物理面积变大，画布中包含的css像素的个数变少
    画布中图像所包含的css像素的个数不变
  - 缩小：缩小画布，画布中的一个css像素所占据的物理面积变小，画布中包含的css像素的个数变多
    画布中图像所包含的css像素的个数不变
- ctx.drawImage(img,x,y,w,h)
  - 在canvas中引入图片一定在图片加载完成之后再去操作
- ctx.measureText("文本")
  - 返回一个持有文本渲染宽度的对象
- ctx.fillText()
- ctx.strokeText()
- ctx.font
- ctx.textAlign
- ctx.textBaseline
- shadowOffsetX = float
- shadowOffsetY = float
- shadowBlur = float
- shadowColor = color(必需项)
- ctx.getImageData(x,y,w,h)
  - ImageData对象
  - width：选中区域在横向上css像素的个数
  - height：选中区域在纵向上css像素的个数
  - data:数组
    - 选中区域所有像素点的rgba信息，rgba的取值从0到255
- ctx.putImageData(imgdata,x,y)
- ctx.createImageData(w,h)
  - 返回的是imgdata对象 默认像素点的信息是rgba(0,0,0,0)
- ctx.globalAlpha
  - 取值为0到1
- ctx.globalCompositeOperation
  - source-over(默认值):源在上面,新的图像层级比较高
  - source-in  :只留下源与目标的重叠部分(源的那一部分)
  - source-out :只留下源超过目标的部分
  - source-atop:砍掉源溢出的部分
  - destination-over:目标在上面,旧的图像层级比较高
  - destination-in:只留下源与目标的重叠部分(目标的那一部分)
  - destination-out:只留下目标超过源的部分
  - destination-atop:砍掉目标溢出的部分
- ctx.ispointinpath(x,y)
  - x,y这个点是否在路径上

## 实例

1. 时钟动画：结合了所有基础api
2. 飞鸟动画：结合图片创建动画
3. 马赛克：像素操作
4. 刮刮卡：合成+像素操作

# canvas 画气泡

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            *{
                margin: 0;
                padding: 0;
            }
            body{
                background: pink;
            }
            canvas{
                position: absolute;
                left: 0;
                right: 0;
                top: 0;
                bottom: 0;
                margin: auto;
                background: white;
            }
        </style>
    </head>
    <body>
        <canvas width="150" height="400"></canvas>
    </body>
    <script type="text/javascript">
        window.onload=function(){
            var oc = document.querySelector("canvas");
            if(oc.getContext){
                var ctx = oc.getContext("2d");
                            
                var arr=[];
                
                //将数组中的圆绘制到画布上
                setInterval(function(){
                    console.log(arr)
                    ctx.clearRect(0,0,oc.width,oc.height);
                    //动画
                    for(var i=0;i<arr.length;i++){
                        arr[i].deg+=5;
                        arr[i].x = arr[i].startX +  Math.sin( arr[i].deg*Math.PI/180 )*arr[i].step*2;
                        arr[i].y = arr[i].startY - (arr[i].deg*Math.PI/180)*arr[i].step ;
                        
                        if(arr[i].y <=50){
                            arr.splice(i,1) // 清除已出界的数组元素
                        }
                    }
                    
                    
                    //绘制
                    for(var i=0;i<arr.length;i++){
                        ctx.save();                     ctx.fillStyle="rgba("+arr[i].red+","+arr[i].green+","+arr[i].blue+","+arr[i].alp+")";
                        ctx.beginPath();
                        ctx.arc(arr[i].x,arr[i].y,arr[i].r,0,2*Math.PI);
                        ctx.fill();
                        ctx.restore();
                    }
                },1000/60)              
                
                //往arr中注入随机圆的信息
                setInterval(function(){
                    var r =Math.random()*6+2;
                    var x = Math.random()*oc.width;
                    var y = oc.height - r;
                    var red =   Math.round(Math.random()*255);
                    var green = Math.round(Math.random()*255);
                    var blue =  Math.round(Math.random()*255);
                    var alp = 1;
                            
                    var deg = 0;
                    var startX = x;
                    var startY = y;
                    //曲线的运动形式
                    var step =Math.random()*20+10;
                    arr.push({
                        x:x,
                        y:y,
                        r:r,
                        red:red,
                        green:green,
                        blue:blue,
                        alp:alp,
                        deg:deg,
                        startX:startX,
                        startY:startY,
                        step:step
                    })
                },50)
            }
        }   
        
    </script>
</html>

```