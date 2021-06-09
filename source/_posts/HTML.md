---
title: HTML
date: 2016-06-05 18:50:13
updated: 2016-06-05 18:50:13
tags: HTML
---
# 网站是什么

[参考资料](http://c.biancheng.net/view/7410.html)

网站是由一个一个网页构成的，要想理解网站是什么，首先要理解网页是什么

可以认为，网站就是放在服务器上的一个文件夹，该文件夹中可以包含子文件夹以及各种各样的文件，这些文件都可以通过域名来访问
<!--more-->

## 网页是什么

网页其实就是放在服务器上的一个文件，当我们浏览网页时，浏览器向服务器发请求，然后对请求的结果进行解析，渲染出各种漂亮的界面，比如表格、图片、标题、列表等

网页的本质 —— **由 HTML 代码构成的纯文本文件**

#  网页的基本构成

结构

- HTML —— 用于描述页面的结构
- 使用 **`标签`** 的形式来标识网页中的不同组成部分
- `纯文本`只能保存文本内容

表现

- CSS —— 用于控制页面中元素的样式

行为

- JS —— 用于响应用户操作

# 什么是 HTML

HTML 全称 —— 超文本标记语言

HTML 可以将符合规范的字符串让浏览器解析生成网页，没有 html 包裹的字符是无法被浏览器识别的

HTML 是由一系列节点组成的，我们网页结构中的标签、注释、属性、文本都是节点

总结起来，**HTML 就是将这些节点通过浏览器解析后生成网页的一种计算机语言**

# 标签

[**HTML 标签汇总**](<http://www.w3school.com.cn/tags/index.asp>)

## 网页版本声明 doctype

doctype

- 加在网页最顶部

```html
<!-- html5 的版本声明 -->
<!DOCTYPE html>
```

## html 标签

此元素可告知浏览器其自身是一个 HTML 文档

根标签, 一个网页中有且只有一个.网页中所有内容都应该写在根标签中

```html
<html lang="en">
  <head>
  </head>
  <body>
  </body>
</html>
```

lang 指明你的网页是英文还是中文网页，当指定值和本地浏览器默认值不一样时，浏览器会提示 `是否翻译为中文`, 可以不指定，也可以指定为 `zh-CN`

| 常用属性值 | 说明                             |
| ---------- | -------------------------------- |
| "en"       | 声明此网页为英文网页             |
| "zh-CN"    | 声明此网页为中文网页             |
| 不写 lang  | 浏览器自行判断是否弹出`是否翻译` |

## head

meta、title、link、style 以及 script 标签是可以放在 head 标签内部的

## meta 标签

meta 标签内的信息不会显示在页面中

meta 标签有很多功能

1. 可以定义文档中的关键字
2. 可以对文档进行描述
3. 可以配合自身的属性设置网页的过期时间

### meta 的 charset 属性

用来定义页面的编码格式

| 常用属性值 | 说明                                               |
| ---------- | -------------------------------------------------- |
| ISO-8859-1 | 表示网页的默认编码格式                             |
| UTF-8      | 表示万国码，是目前最常用的编码格式                 |
| gb2312     | 表示国际汉字码，不包含繁体                         |
| gbk        | 表示国家标准扩展版。增加了繁体，包含所有亚洲字符集 |

```html
<meta charset="UTF-8" />
```

### 乱码问题

- 计算机只认识 0 和 1

  - 编码——依据一定的规则, 将字符转换成二进制编码的过程
  - 解码——依据一定的规则,将二进制编码转换为字符的过程
  - 字符集——编码和解码的规则
    - 常见的字符集
      - ASCII
      - ISO-8859-1
      - GBK
      - GB2312
      - UTF-8 (最常用)
      - ANSI (自动以系统的默认编码来保存文件)

- 产生乱码的原因

  - 编码和解码采用的字符集不同

  - 解决方法

    ```html
    <meta charset="UTF-8" />
    ```

### meta 的 name 属性

name 属性可以用来定义网页的关键字、描述、作者以及版权信息

| 常用属性值                            | 说明                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| keywords                              | 用来定义网页的关键字。关键字可以是多个，之间需要用英文逗号`,`隔开 |
| description                           | 用来定义网页的描述                                           |
| author                                | 用来定义网页的作者                                           |
| copyright                             | 用来定义网页的版权信息                                       |
| viewport                              | 优化移动浏览器的显示                                         |
| theme-color                           | 浏览器地址栏变色                                             |
| format-detection                      | 忽略页面中符合格式的字符识别为电话号码和邮箱                 |
| apple-mobile-web-app-capable          | 启用 WebApp 全屏模式                                         |
| apple-mobile-web-app-status-bar-style | 隐藏状态栏/设置状态栏颜色：只有在开启WebApp全屏模式时才生效(content的值为default \| black \| black-translucent) |

name 属性需要和 content 属性一起使用才能生效

```html
<meta name="keywords" content="<head>标签描述">
<meta name="description" content="这篇文章主要对<head>标签进行详细讲解">
<meta name="author" content="author">
<meta name="copyright" content="本站所有教程均为原创，版权所有，禁止转载。否则将追究法律责任。">

<meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>

<meta name="format-detection" content="telphone=no, email=no"/>  

<meta name="apple-mobile-web-app-capable" content="yes" />

<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

### mea 的 http-equip 属性

可以设置网页的过期时间，自动刷新

| 常用属性值      | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| expires         | 设置网页的过期时间                                           |
| refresh         | 设置网页自动刷新的时间间隔，单位是秒                         |
| content-type    | 定义文件的类型，用来告诉浏览器该以什么格式和编码来解析此文件 |
| X-UA-Compatible | 避免 IE 使用兼容模式                                         |

```html
<meta http-equiv="content-type" content="text/html"> 用来告诉浏览器本网页编写的是 HTML 代码，需要用 HTML 的格式来进行解析
<meta http-equiv="expires" content="Dec 20 2090"> 定义网页于 2090 年 12 月 20 日过期
<meta http-equiv="refresh" content="1000"> 设置了页面每隔 1000 秒就会进行一次刷新
<meta http-equiv="X-UA-Compatible" content="IE=edge">

<!-- 5s 后跳到百度 -->
<meta http-equiv="refresh" content="5;url=http://www.baidu.com" />
```

http-equiv 也需要搭配 content 属性来使用

## title

用来表示文档的标题, 搜索引擎在检索页面时, 会首先检索 title 标签 中的内容

## link

<link> 标签配合 href 属性来引用外部 CSS

- **链入一个文档, 通过 rel 属性指定链入文档内容的类型**

  ```html
  <link rel="stylesheet" href="index.css" />
  ```

## style

用来在内部编写 CSS 样式

## script

用来引入外部文件, 配合 src 属性

*不建议写在 head 中*

### href 和 src 的区别

- src 是 source 的简写，表示来源地址，用来引入地址中的内容。**引入的内容会嵌入到当前标签所在的位置**，所以浏览器在解析引入文件时，会停止对后续文档的处理，直到 src 的内容加载完毕
- href 是 Hypertext Reference 的简写，表示超文本引用。在使用 href 时，浏览器不会停止解析当前文件。因为 href 属性中的内容只是与当前页面有关联，然后当前页面对它进行一次引用

# a

<a> 标签来表示超链接

href 属性用来指明要跳转到的 url，target 属性用来指明新页面的打开方式

```html
<a href="http://c.biancheng.net" target="_blank">C语言中文网</a>
```

## a 标签的 href 属性

属性指定链接的目标，也就是要跳转到什么位置，href 可以有多种形式

href 使用的路径可以是绝对路径，也可以是相对路径

## a 标签的 target 属性

使用 target 属性来改变目标窗口的打开方式

| 属性值 | 说明                                           |
| ------ | ---------------------------------------------- |
| _self  | 默认，在现有的窗口中打开新页面，原窗口将被覆盖 |
| _blank | 在新的窗口中打开新页面                         |

## a 标签的默认样式

a 标签默认带有下划线，字体为蓝色

可以重置 a 标签的样式

# img

用来引入一张图片，src 属性的值可以是绝对路径，也可以是相对路径

```html
<img src="../image/logo.jpg"/>
```

img 标签如果只指定宽度(或高度)，图片默认是等比缩放的

img 元素默认有 3px 下边距

- 如何解决？
  1. display: block;
  2. vertical-align: middle;
  3. img 父元素设置 font-size: 0;

# form

HTML 中的表单不仅可以供用户填写信息还可以为用户提供信息，将用户信息提交给服务器

表单元素应该放在 <form> 标签中, form 不可以嵌套

| form 常用属性 | 属性值                                | 说明                               |
| ------------- | ------------------------------------- | ---------------------------------- |
| action        | url                                   | 表单被提交到的位置                 |
| method        | get / post                            | 表单提交时的请求方式               |
| autocomplete  | "autocomplete"                        | 输入提示                           |
| novalidate    | "novalidate"                          | 浏览器开启表单验证                 |
| enctype       | "url-encoded" / "multipart/form-data" | 提交的数据编码，有文件域时必须设置 |

```html
<form action="url" method="post" name="formName"></form>
```

## input

是自闭合标签，根据其 type 属性值的不同分为很多种，例如单行文本框、密码框、单选按钮、复选框、隐藏域、文件上传域、普通按钮、提交按钮以及重置按钮等

| 常用 type 值 | 说明             |
| ------------ | ---------------- |
| text         | 单行文本框       |
| password     | 密码框           |
| radio        | 单选按钮         |
| checkbox     | 复选按钮         |
| button       | 普通按钮         |
| submit       | 带提交功能的按钮 |
| reset        | 带重置功能的按钮 |
| file         | 文件域           |
| hidden       | 隐藏域           |

**hidden 表示隐藏域，在页面中对于用户是不可见的，在表单中插入隐藏域可以方便收集或发送信息**

## 单行文本框

type="text" 就是单行文本框

⚠️ 当 input 内还有其他元素时，**注意层级问题**

### maxlength

maxlength —— 设置文本框中最多可以输入的字符数量（包括空格）

```html
<input type="text" maxlength="3" placeholder="请输入..." />

<input type="password" maxlength="3" />
```

## 单选按钮

1. name 属性值应该相同

```html
<form action="http://vip.biancheng.net/login.php" method="post" name="formName">
  性别：<input type="radio" name="sex" checked>女
  <input type="radio" name="sex" checked>男
</form>
```

2. checked 属性用来设置页面加载时单选按钮的选中状态

3. value 值并不会显示在页面中, 但是提交表单时，提交给服务器的是 value 值

## 复选框

```html
<input type="checkbox" name="hobby" value="0" />足球
<input type="checkbox" name="hobby" value="1" checked />羽毛球
<input type="checkbox" name="hobby" value="2" />篮球
```

和单选按钮的区别

- 提交表单时，单选按钮提交的是选中项的 value 字符串；而复选框提交的是选中项的 value 组成的数组

## 文件域

要保证文件可以正确提交给表单服务器, 

1. 需要设置 form 的 enctype 属性为 multipart/form-data,

2. 还需要指定文件类型 accept
3. 必须是 post 请求

```html
<form action="http://vip.biancheng.net/register.php" method="post" enctype="multipart/form-data">
    <input type="file" name="file" accept="image/png"/>
</form>
```

### multiple

当给上传文件字段设置了 multiple 属性时，就表示可以同时选择多个文件一起上传

### 常用的文件类型

| 常用文件类型 | 对应的 accept 属性值                    |
| ------------ | --------------------------------------- |
| .jpg         | image/jpg                               |
| .png         | image/png                               |
| .gif         | image/gif                               |
| .jpeg        | image/jpeg                              |
| .html        | text/html                               |
| .css         | text/css                                |
| .js          | text/javascript、application/javascript |
| .txt         | text/plain                              |
| .zip         | application/zip                         |
| .mp4         | audio/mp4、video/mp4                    |

## textarea

多行文本框，又叫做文本域

文本域要想正确提交，也必须设置 name 属性

默认右下角显示缩放按钮，可以通过样式让其不可缩放 `resize:none;`

```html
<form action="http://vip.biancheng.net/register.php" method="post">
    描述信息：<textarea name="description" style="resize:none;">此处是描述信息</textarea>
</form>
```

## select

下拉列表，配合 option 使用

下拉列表要想被正确提交，也需要设置 name 属性

- multiple —— 按住`ctrl`+鼠标左键就可以选择多个选项

- selected —— 加给 option 实现某一项的预先选中

- value —— 用来定义当下拉列表在提交时，发送给服务器的值

- 在 select 中可以使用 optgroup 对选项进行分组, 同一个 optgroup 中的选项是一组

  ```html
  <select name="fav" multiple>
      <option value="0" selected>胡歌</option>   
      <option value="1">彭于晏</option> 
      <option value="2">吴彦祖</option>
  </select>
  
  <select name="fav">
      <optgroup label="男明星">
          <option value="0" selected>胡歌</option>   
        <option value="1">彭于晏</option> 
        <option value="2">吴彦祖</option>
      </optgroup>
      <optgroup label="女明星">
          <option value="3">胡歌</option>   
        <option value="4">彭于晏</option> 
        <option value="5">吴彦祖</option>
      </optgroup>
  </select>
  ```

## 按钮

```html
<!-- 普通按钮，不会提交表单 -->
<input type="button" value="按钮" />

<!-- 提交按钮，点击会提交表单 -->
<input type="submit" value="提交" />

<!-- 重置按钮，点击会清空表单数据, reset 按钮只能清空用户输入的内容，提前设置好的内容不会清空 -->
<input type="reset" value="重置" />
```

## label 标签

可以指定一个 for 属性, 该属性值需要指定一个表单项的 id 值

```html
<label for="username">用户名</label>
```

## fieldset

在表单中可以使用 fieldset 来为表单分组

```html
<fieldset>
    <legend>用户信息</legend>
    用户名<input type="text" name="username" />    
    密码<input type="password" name="password" />
</fieldset>
```

## 图像按钮

既有 img 的属性，也有提交按钮的功能

```html
<form>
  <input type="image" src="btn.jpg" height="100" />
</form>
```

## 表单元素的 name 属性

HTML 规定如果表单要想正确地被提交给表单处理器，必须为每个字段都设置 name 属性

如果未设置，默认不提交其数据信息

## 表单元素的 disabled 属性

如果为 input 标签的某个控件设置了`disabled="disabled"`，表示将禁用该控件，使其不能再获得焦点或被修改。被禁用后，它的值不会提交到后台

## 表单元素的 readOnly 属性

一般用在单行文本框和密码框中

控件的值可以显示，但不能修改

# 标题标签  `h1 - h6`

对搜索引擎来说, h1 的重要性仅次于 title, 会影响到 SEO, 页面里只写一个 h1

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```

# 段落标签  p

默认独占一行, 且有行高

- **内部不能嵌套 div**

```html
<p>
    一个段落……
</p>
```

# 换行标签  `br`

在 HTML 中, 字符之间写再多的空格, 浏览器也会当成一个空格解析, 换行也会当作一个空格解析

- 可以使用 br 标签来换行

  ```html
  <p>
      锄禾日当午,<br />    
      汗滴禾下土,<br />
      谁知盘中餐,<br />
      粒粒皆辛苦.
  </p>
  ```

# 水平线  `hr`

```html
<hr />
```

hr 默认的颜色要通过 background-color 来设置

```css
hr {
  height: 1px;
  background-color: #6EECB4;
  border: none;
}
```

# iframe 内联框架  `iframe`

可以在一个页面里, 引入一个外部页面

- 内联框架的内容不会被搜索引擎检索到

- src —— 相对路径

- width —— 宽度

- height —— 高度

- name —— 名字

  ```html
  <iframe src="demo01.html" name="tom"></iframe>
  ```

# center 标签  `center` (不常用)

内容会默认在页面中居中显示

- 可以把所有要居中的内容放在 center 标签里

  ```html
  <center>
    <p>哈哈哈</p>
    <h1>标题内容</h1>
  </center>
  ```

# 文本标签

| 标签        | 说明                                                        |
| ----------- | ----------------------------------------------------------- |
| em / i      | 倾斜                                                        |
| strong / b  | 加粗                                                        |
| ins / u     | 下划线                                                      |
| del / s     | 删除线                                                      |
| small       | 比父元素文字小                                              |
| cite        | 带书名号的内容可以加上 cite                                 |
| q           | 短引用(名人语录, 浏览器会为 q 标签的内容加上引号), 行内元素 |
| block quote | 长引用, 块级元素                                            |
| sup         | 上标                                                        |
| sub         | 下标                                                        |
| pre         | 预格式标签, 怎么写的就怎么显示                              |
| code        | 代码段标签, (一般结合 pre 标签使用)                         |

# 无序列表

ul > li

- 通过 type 属性可以修改圆点的样式

# 有序列表

ol > li

- 有序号, 通过 type 属性可以修改序号的类型
- 列表之间可以相互嵌套

# 自定义列表

对一些词汇或内容进行定义

- dl > dt + dd

- dt —— 被定义的内容

- dd —— 对定义内容的描述

  ```html
  <dl>
      <dt>武汉</dt>
      <dd>武汉市湖北省省会...</dd>
      <dd>拥有长江...</dd>
  </dl>
  ```

# 表格  table

用来表示一些<font color=#c00>`格式化`的数据</font>的

- th 标签

  - 表头内容 (特殊的 td)
  - 文字默认居中, 加粗

- colspan 横向合并单元格

  ```html
  <td colspan="2">D3</td>
  ```

- rowspan 横向合并单元格

  ```html
  <td rowspan="2">D3</td>
  ```

- border-spacing (table 和 td 之间的距离)

  ```css
  border-spacing: 0;
  ```

- <font color=#c00>border-collapse (边框合并, `加给 table`)</font>

  - 设置了 border-collapse, border-spacing 会失效

  ```css
  border-collapse: collapse;
  ```

- 如果表格中没有写 tbody, 浏览器会自动在表格中添加 tbody, 并将所有的 tr 都放到 tbody 中; 

  所以注意, `tr 并不是 table 的直接子元素`

# 框架集

框架集和内联框架的作用类似, 都是用于在一个页面中引入其他外部的页面

- 框架集可以同时引入`多个页面`, 内联框架只能引入一个

- 使用 frameset 来创建一个框架集

  - frameset 不能和 body 出现在同一个页面中

  - frame 来指定要引入的页面

    - rows —— 指定引入的框架一行一行地排列
    - cols —— 指定引入的框架一列一列地排列
    - 这两个属性, frameset `必须选择一个`
    - frameset 里可以嵌套 frameset

    ```html
    <frameset cols="30%, *,  30%">
        <frame src="01.html" />   
        <frame src="02.html" />    
        <frameset rows="50%, 50%">
            <frame src="03.html" />        
            <frame src="04.html" />
        </frameset>
    </frameset>
    ```

  ⚠️ 框架集不利于 SEO, 不能有自己的内容, 只能引入页面, 引入几个页面就要发送几次请求, 效率低

# 块元素和内联元素

| 属性         | 代表元素           | 说明                                                         |
| ------------ | ------------------ | ------------------------------------------------------------ |
| block        | div、h1～h6、p、ul | 独占一行, 无论它的内容有多少, 都会独占一行;用来对 `网页布局` 的 (网页分区) |
| inline-block | img、input、button | 宽高由内容决定，可以设置宽高; 可以设置 padding、margin       |
| inline       | span、a、b、del    | 只占自身大小的元素，不可以设置宽高;只能设置横向上的 padding、margin |

1. 一般情况下, 只使用块元素包含内联元素, 而不会使用内联元素包含块元素

2. a  元素可以包含 `任意` 元素, 除了它本身

3. p 元素内不能放 `任何` 块级元素
4. inline-block 宽度默认由内容决定
    当子元素是块级元素时，子元素宽度由自身内容决定，不会独占一行
    当有多个块级子元素时，块级子元素的宽度会一样，等于宽度最大的那个

# 实体(特殊字符)

[实体参考](<http://www.w3school.com.cn/html/html_entities.asp>)

- HTML 里一些特殊字符, 不能直接使用, 需要使用一些特殊的符号来表示, 这些特殊符号我们成为实体

  ```html
  <!-- < -->
  &lt;
  
  <!-- > -->
  &gt;
  
  <!-- 空格 -->
  &nbsp;
  
  <!-- 版权符号©️ -->
  &copy;
  ```

# 标签的属性

属性

- 实际上就是一个 `名值对`

  ```html
  <font color="red">红色的字</font>
  ```

- id 属性

  -  每一个元素都可以设置, 该属性可以作为标签的唯一标识
  -  id 属性在同一个页面中不能重复

- title 属性

  - 可以加给任意元素
  - 当鼠标移上去的时候显示 title 的值

# URL 的构成

[URL 参考](http://c.biancheng.net/view/7530.html)

URL 由协议、主机名、域名、端口、路径、以及文件名这六个部分构成，其中端口可以省略

```shell
scheme://host.domain:port/path/filename
```

# 相对路径和绝对路径

## 绝对路径

绝对路径以 `域名` 或 `盘符` 为参考点

绝对路径之所以称为绝对，是指**当不同的页面引用同一个文件时，使用的路径都是一样的**

1. 本地绝对路径

   本地绝对路径一般指从盘符开始，到文件名称结束

2. 网络绝对路径

   网络绝对路径指从网站的域名开始，到文件名结束，在使用时需要加上协议

   ```text
   ①D:/Hbulider/HBuilder/tools/nview/index.js
   ②C:/Users/admin/Desktop/C语言中文网/url/url.html
   ③http://c.biancheng.net/view/7410.html
   ④http://c.biancheng.net:80/view/7410.html
   ⑤http://c.biancheng.net 
   ```

## 相对路径

**相对路径以当前文件位置为参考点**，到文件名称结束

```
<a href=”./html/login.html”>跳转到login.html</a>
<a href=”html/login.html”>跳转到login.html</a>
<a href=”../html/login.html”>跳转到login.html</a>

./  表示同级目录，可以省略不写
../ 表示上级目录 
```

# 开发工具

- IDE  (集成开发工具)
  - DreamWeaver
  - WebStorm
  - Hbuilder  (国产)
  - Visual Studio Code

# 项目中 logo 的固定布局方案

```html
<!-- 利于 SEO -->
<h1>
    <a href="javascript:;">
        <img src="logo.png" alt="">
    </a>
</h1>
```

