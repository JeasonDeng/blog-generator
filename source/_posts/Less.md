---
title: Less
date: 2016-06-11 12:30:56
updated: 2016-06-11 12:30:56
tags: CSS
---
# 什么是 less?

[官网](<http://lesscss.cn/>)

Less 是一门 CSS 预处理语言, 它扩展了 CSS 语言, 增加了变量、Mixin、函数等特性, 使 CSS 更容易维护和扩展

Less 可以运行在 Node 或浏览器端
<!--more-->

# 使用

⚠️ 最好在 less 文件开头加

```less
@charset "UTF-8";
```

## 浏览器端用法

less 无法在浏览器端直接使用

- 解决方式

  ```html
  <!-- 以下两行必须写在头部 head 标签里; 并且 link 的 type 属性必须写 -->
  <link rel="stylesheet/less" type="text/less" href="styles.less" />
  <script src="less.js"></script>
  ```

- less 自带监听模式 (无刷新预览效果, **有延迟**)

  ```html
  <!-- 紧挨着上面两行之后 -->
  <script>less.watch();</script>
  ```

- 解决原理

  > 1. 浏览器端通过发送请求到服务端 
  >
  > 2. 拿到 less 文件的内容 
  >
  > 3. 然后再通过 less.js 解析 less 文件内容
  >
  > 4. 将解析后的结果追加到 html 中 (会在 html 中生成 style 标签)

所以, 引入 less 文件的网页 `必须以 http 形式打开`

# less 编译

## 命令行编译

```shell
lessc style.less style.css
```

## Koala工具编译

[Koala](<http://koala-app.com/index-zh.html>)

⚠️ 拖目录, 不要拖文件

# less 语法

## less 中的注释

1. 以 // 开头的注释, 不会被编译到 css 文件中

2. 以 /**/ 开头的注释, 会被编译到 css 文件中

3. 建议使用 `/**/`

## less 中的变量

使用 @ 来声明一个变量 —— `@pink: pink;`

- 变量是 `块级作用域`

- 属性值作为变量 —— 直接使用 @pink

  ```less
  @color: #ccc;
  a {
      color: @color;
  }
  ```

- 选择器或属性名作为变量 —— @{ 变量名 }

  ```less
  @className: .box;
  @{className} {
      margin: 0 auto;
  }
  ```

- 作为 URL —— @{ url }

- 变量的延迟加载

## less 的混入(Mixin)

将一系列属性从一个规则集引入到另一个规则集的方式

- 混入的定义在 less 规范里有明确的指定, 使用 `.` 来定义

### 类混入

也叫 `普通混入`

- 缺点

  - 类混入定义的样式会被编译到 css 文件中

  ```less
  .w50 {
      width: 50%;
  }
  .f_left {
      float: left;
  }
  
  /* 类混入 */
  .box {
      .w50();    /* 相当于函数调用 */
      .f_left();
      color: red;
  }
  ```

### 函数混入

可以带参数, 也可以不带参数

- 不会被编译到 css 文件中

  ```less
  /* 不带参数 */
  .w50() {
      width: 50%;
  }
  
  /* 带参数的混入在调用时, 必须传参 */
  .f(@direction) {
      float: @direction;
  }
  
  /* 带默认参数, 调用时可传可不传 */
  .bordeRadius(@radius: 100px) {
      border-radius: @radius;
  }
  
  /* 多个参数 */
  .m(@v: 0, @h) {
    margin: @v @h;
  }
  
  /* 函数混入 */
  .box {
      .w50();    /* 相当于函数调用 */
      .f(left);
      .borderRadius();
      color: red;
        .m(@h: auto); /* 命名参数 当实参与形参个数不一致时, 可以采用这种命名参数形式 */
  }
  ```

- **匹配模式**

  - 调用 .triangle(L, 10px, red) 时
    - 第一个参数为 `@_` 的混合(*放共用的重复代码*) 会自动调用
    - 然后再调用同名的 .triangle(L, @w, @c)

  ![](/images/匹配模式.png)

- argumrnts 变量

  ```less
  .border(@a, @b, @c) {
      border: @arguments;
  }
  #wrap .sjx {
      .border(1px, solid, black);
  }
  ```

## less 的嵌套

基本嵌套规则

- **& 的使用 —— 同级**

  ```less
  .container {
      width: 100%;
      a {
          color: #000;
          &:hover {
            color: red;
        }
      }
      > div {
          display: inline;
      }
  }
  ```

## less 中使用媒体查询

⚠️ 多个 @media 之间使用分号

```less
// mixins.less
.1-px(@color) {
  position: relative;
  &::before {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 1px;
    background: @color;
    @media (-webkit-device-pixel-ratio: 2){
      transform: scaleY(.5);
    };
    @media (device-pixel-ratio: 2) {
      transform: scaleY(.5);
    };
    @media (-webkit-device-pixel-ratio: 3){
      transform: scaleY(.333333);
    };
    @media (device-pixel-ratio: 3) {
      transform: scaleY(.33333);
    };
  }
}
```

## 导入 import

使用 @import "文件名";

```less
/* variables.less */
@charset "UTF-8";
@mainColor: #e32322;

/* main.less */
@charset "UTF-8";
@import "variables";
@import "mixin"
```

⚠️ less 中的 @import 和 css 中的 @import 不一样

less 中 —— 合并代码

css 中 —— 发请求

## less 的运算函数

在 less 中可以进行 `加减乘除` 的运算, *计算的一方带单位就可以了*

- 在 less 中可以使用 [内置函数](<http://lesscss.cn/functions/>) 进行运算

  ```less
  .sjx {
      width: (100 + 100px);
  }
  
  @num: 8;
  ul {
      width: 100% * @num;
      li {
          width: 100% / @num;
          color: lighten(red, 20%); /* 比红色亮 20% */
      }
  }
  ```

## less 继承

继承 **只能继承类样式, 不能加 () 来传递参数**

- 比混合性能高, 没有混合灵活

- 语法

  1. .a:extend(.b) { ... }

  2. .a {

     ​  &:extend(.b all);

     ​  ...

     }

     `all` 表示继承与 b 相关的所有样式

  ```less
  // extend.less
  @import './juzhong-extend.less';
  #wrap {
      position: relative;
      width: 300px;
      height: 300px;
      border: 1px solid;
      .inner {
          &:extend(.juzhong all);
          &:nth-child(1) {
              width: 100px;
              height: 100px;
              background: pink;
          }
          &:nth-child(2) {
              width: 50px;
              height: 50px;
              background: deeppink;
          }
      }
  }
  
  // juzhong-extend.less
  .juzhong {
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      bottom: 0;
      margin: auto;
  }
  ```

## less 避免编译

使用 ~ "属性值" 避免被编译

```less
* {
    margin: 0;
    padding: ~"cacl(100 + 100px)";
}
```

