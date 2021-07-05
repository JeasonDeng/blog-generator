---
title: Flex布局
date: 2016-06-21 10:09:01
updated: 2016-06-21 10:09:01
tags: CSS
---
# flex 布局是什么?

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性

> 布局的传统解决方案，基于[盒状模型](https://developer.mozilla.org/en-US/docs/Web/CSS/box_model)，依赖 [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display) 属性 + [`position`](https://developer.mozilla.org/en-US/docs/Web/CSS/position)属性 + [`float`](https://developer.mozilla.org/en-US/docs/Web/CSS/float)属性。它对于那些特殊布局非常不方便，比如，[垂直居中](https://css-tricks.com/centering-css-complete-guide/)就不容易实现

<!-- more -->
任何一个容器都可以指定为 Flex 布局

```css
.box {
    display: flex;
}
```

行内元素也可以使用 flex 布局

```css
.box {
    display: inline-flex;
}
```

⚠️ **设为 flex 布局后, 子元素的 float、clear、vertical-align 属性将失效**

# 教程参考

[菜鸟教程](<https://www.runoob.com/w3cnote/flex-grammar.html>)

# 基本概念

采用 Flex 布局的元素，称为 Flex 容器（flex container）, 简称"**容器**"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"**项目**"

![](/images/flex.png)

- 容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）
  - 主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`
  - 交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`
- **项目默认沿主轴排列**
- 单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`

# 容器的属性

以下 6 个属性设置在 `容器` 上

- flex-firection
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

## flex-direction

`flex-direction` 属性决定主轴的方向（即项目的排列方向）

```css
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

| 取值             | 说明                       |
| ---------------- | -------------------------- |
| row (**默认值**) | 主轴为水平方向, 起点在左端 |
| row-reverse      | 主轴为水平方向, 起点在右端 |
| column           | 主轴为垂直方向, 起点在顶部 |
| column-reverse   | 主轴为垂直方向, 起点在底部 |

## flex-wrap

默认情况下, 项目都排列在一条线上

> flex-wrap 属性定义如果在一条线上排不下,  如何换行

```css
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

- nowrap(默认)

  - 不换行

    ![](https://www.runoob.com/wp-content/uploads/2015/07/9da1f23965756568b4c6ea7124db7b9a.png)

- wrap

  - 换行, 第一行在上方

    ![](https://www.runoob.com/wp-content/uploads/2015/07/3c6b3c8b8fe5e26bca6fb57538cf72d9.jpg)

- wrap-reverse

  - 换行, 第一行在下方

    ![](https://www.runoob.com/wp-content/uploads/2015/07/fb4cf2bab8b6b744b64f6d7a99cd577c.jpg)

## flex-flow

flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 `row nowrap`

```css
.box {
    flex-flow: row nowrap;
}
```

## justify-content

justify-content 属性定义了**项目在主轴上的对齐方式**

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![](https://www.runoob.com/wp-content/uploads/2015/07/c55dfe8e3422458b50e985552ef13ba5.png)

## align-items

align-items 属性定义**项目在交叉轴上如何对齐**

```css
.box {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![](https://www.runoob.com/wp-content/uploads/2015/07/2b0c39c7e7a80d5a784c8c2ca63cde17.png)

## align-content

align-content 属性定义了多根轴线(*就是**要换行***)的对齐方式。*如果项目只有一根轴线，该属性不起作用*

- <font color=#c40>把所有 items 当作一个整体来看待， 就是看这个整体在交叉轴上的对齐方式</font>

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![](https://www.runoob.com/wp-content/uploads/2015/07/f10918ccb8a13247c9d47715a2bd2c33.png)

# 项目的属性

以下 6 个属性设置在 `项目` 上

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

## order

order 属性定义项目的排列顺序。**数值越小，排列越靠前**，默认为 0

```css
.item {
    order: 1;
}
```

## flex-grow

flex-grow 属性定义项目的<font color=#c40>**放大比例**</font>，默认为 0，即如果 *存在剩余空间*，也不放大

```css
.item {
    flex-grow: 2;
}
```

- 如果所有项目的 flex-grow 属性都为 1，则它们将**等分剩余空间**（如果有的话）

  - 如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍

  ![](https://www.runoob.com/wp-content/uploads/2015/07/f41c08bb35962ed79e7686f735d6cd78.png)

## flex-shrink

flex-shrink 属性定义了项目的<font color=#c40>**缩小比例**</font>，默认为1，即如果 *空间不足*，该项目将缩小

```css
.item {
    flex-shrink: 1;
}
```

- 如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小

  - 先计算总共超出多少，再分别按比例缩小超出部分

- 如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小

- 负值对该属性无效

## flex-basis

flex-basis 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）

浏览器根据这个属性，计算主轴是否有多余空间

它的默认值为auto，即项目的本来大小

## flex

flex 属性是 flex-grow, flex-shrink 和 flex-basis 的简写，默认值为 0 1 auto。**后两个属性可选**

- 该属性有三个常用值

  - auto —— 相当于 1 1 auto

  - none —— 相当于 0 0 auto

  - **1 —— 相当于 1 1 0%**

    ```css
    .item {
        flex: 1; /* 可以实现等分布局, 相当于将所有项目的 flex-basis 置为 0, 再等分 */
        /* 等价于
            flex-grow: 1;
            flex-shrink: 1;
            flex-basis: 0%;
        */
    }
    ```

⚠️ `flex: 1; `**只能用于单行等分**的情况；如果有换行就不起作用

# flex 不要和定位混用

**有定位属性的元素就不再具有 flex 布局的特性了**

# flex 布局元素的高度问题

flex 布局的元素的高度问题

- 如果本身设置了 height, 则按设置的来；
- 如果本身没有设置 height, height 跟父元素一致；
- 如果父元素也没有设置 height, 则跟同一行的元素的最大高度一致

# flex 实现栅格布局

```html
<div class="row">
  <div class="col-12">占满一行</div>
  <div class="col-6">两列均分</div><div class="col-6">两列均分</div>
  <div class="col-4">三列均分</div><div class="col-4">三列均分</div><div class="col-4">三列均分</div>
  <div class="col-3">四列均分</div><div class="col-3">四列均分</div><div class="col-3">四列均分</div><div class="col-3">四列均分</div>
  ...
  <div class="col">任意均分</div>......<div class="col">任意均分</div>
</div>
```

```css
.row {
  display: flex;
  flex-wrap: wrap;
}
.col-12 {
  flex: 0 0 100%;
}
.col-6 {
  flex: 0 0 50%;
}
.col-4 {
  flex: 0 0 33.3333%;
}
.col-3 {
  flex: 0 0 25%;
}
.col {
  flex: 1;
}
```

