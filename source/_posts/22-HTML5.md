---
title: HTML5
date: 2016-07-21 11:12:16
updated: 2016-07-21 11:12:16
tags: HTML
---
#  html5
      
- HTML5 是定义 HTML 标准的最新版本, 该术语表示两个不同的概念
  + 它是一个新的 HTML 语言, 具有新的元素、属性和行为
  + 它有更强大的技术集, 允许更多样化和强大的网站和应用程序, 这个技术集就是 HTML5
  + *约等于 HTML + CSS + JS*
<!--more-->

- HTML5 的优势

  - 跨平台
    - 通吃 PC、Mac、iPhone、 Android 等平台
  - 快速迭代
  - 导流入口多
  - 分发效率高

- IE8 不支持 html5 标签

  - 解决 —— 引入 `html5shiv.min.js`
    ```html
    <!--[if lte IE 8]>
        <script src="html5shiv.min.js"></script>
    <![endif]-->
    </head>
    ```

# html5 和 html4 的区别

- DOCTYPE
  - 文档类型声明
    - 决定浏览器解析需要采用的渲染模式
    - document.compatMode 可以返回渲染模式
- 根元素
  - html4 根元素含有很多说明
  - html5 根元素很干净
- content
  - html4 在 meta 标签中显式定义
  - html5 不需要显式定义, content-type 在请求头中

# 语义化标签

- [常用语义化标签](<https://www.w3school.com.cn/tags/index.asp>)
  - hgroup
    - 可以在里边包含多个 h1-h6 标签
  - header
  - nav
    - 主导航
  - section
    - 页面上的一块区域, 相当于 div
  - footer
  - aside
    - 侧边栏
  - ⚠️ 这些语义化标签没有默认样式, 可以直接使用
- 语义化好处
  - 代替无意义的 div 标签
  - 提升了网页的质量和语义
  - 对搜索引擎更友好

# h5 新增其他标签

- 状态标签 (进度条)
  - meter —— 用来表示已知范围的标量值或者分数值
    - value : 当前数值
    - min : 最小值 (默认为0)
    - max : 最大值 (默认为1)
    - low : 低值区间的上限值
    - high : 高值区间的下限值
    - optimum : 最优取值
  - progress —— 用来表示一项任务的完成进度
    - max : 一共需要完成多少工作
    - value : 指定进度条已完成的工作量 (没有表示不确定)

- 列表标签
  - datalist —— 包含一组 option 元素, 这些元素表示其表单控件的可选值
    - <font color=#c40>它的 id 必须要和 input 中的 list 一致</font>
  - datails —— 一个 ui 小部件, 用户可以从其中检索附加信息
    - open 属性来控制附加信息的显示与隐藏
    - summary —— 用做一个 <details> 元素的一个内容摘要 (标题)

- 注释标签
  - ruby —— 展示文字注音或注释
    - rt 包裹拼音或注释内容

- 标记标签
  - mark —— 着重
  ```html
  <meter value="55" min="20" max="100" low="40" high="80" optimum="50"></meter>
    
  <progress value="10" max="100"></progress>
  
  <input type="text" placeholder="你最喜欢的是" list="fav">
  
  <datalist id="fav">
    <option value="1">栗子</option>
    <option value="2">香蕉</option>
    <option value="3">苹果</option>
    <option value="4">西瓜</option>
  </datalist>
  
  <details open>
    <summary>李白</summary>
    <p>诗仙, 生于...,老婆...</p>
  </details>
  
  <span><ruby>邓<rt>deng</rt></ruby></span>
  
  <span>我是<mark>湖北</mark>人</span>
  ```

# h5 新增表单相关

## 新增表单格式

- type="email"
  - 校验是否含有 @
- type="tel"
  - 移动端点击 type 为 `tel` 的表单会弹出数字键盘
- type="url"
- type="search"
  - 右边带 ❌ , 可以清空表单
- type="range"
  - 滑块条
  - 可指定 min、max、step
- type="number"
  - 右边带 `上/下`
- type="color"
  - 颜色选择器
- type="datetime"
  - 完整日期 (PC 端无作用)
- type="datetime-local"
  - 不含时区
- type="time"
  - 显示时间
- type="date"
  - 显示日期
- type="week"
  - 显示周
- type="month"
  - 显示月

## h5 新增表单属性

- placeholder
  - 提示信息
  - 怎么选中 placeholder? —— input :: -webkit-input-placeholder

- autofocus
  - 自动获得焦点

- required
  - 必填项

- pattern
  - 正则校验
    ```html
    <input type="text" pattern="\d{1, 5}"/>
    ```

- formaction
  - 在 submit 里定义提交地址
    ```html
    <form>
      <input type="submit" value="提交" formaction="http://www.baidu.com"/>
    </form>
    ```

- list 
  - 为输入框构造一个选择列表
  - 表单的 **list 值为 datalist 标签的 id**

## 表单验证

- validity 对象
  - 通过下面的 valid 可以查看验证是否通过, 如果八种验证都通过, 则返回 `true`
    否则返回 `false`
  - valueMissing
  - typeMismatch
  - patternMismatch
  - toolong
  - rangeUnderflow
  - rangeOverflow
  - stepMismatch
  - customError
    - 不符合自定义规则
    - setCustomValidity
    ```js
    var input = document.querySelector('input[type=text]')
    var submit = document.querySelector('input[type=submit]')
    submit.onclick = function () {
        var val = input.value
        if (val === 'deng') {
            input.setCustomValidity('请不要输入敏感词') // 自定义规则，验证失败
        } else {
            input.setCustomValidity('') // 验证通过
        }
    }
    ```

- node.addEventListener('invalid', fn1, false)
  - *验证不通过时触发 'invalid'*
    ```js
    var input = document.querySelector('input[type=text]')
    input.addEventListener('invalid', function () { // 点击 submit 按钮时, 如果验证不通过, 就触发
        console.log('验证失败')
        console.log(this.validity) // { valueMissing: true, typeMismatch: false, ... }
    })
    ```

- 关闭验证
  - **formnovalidate 属性**

# attribute 和 property

- html 标签的预定义和自定义属性我们统称为 attribute
- js 原生对象的直接属性, 我们统称为 property
- 布尔值属性 —— property 数值为布尔类型的
- <font color=#c40>一句话, 操作布尔值属性用 prop() 方法</font>

# 新增实用小功能

## classList 操作类

- **classList**
- div.classList.add('hello')
  - 添加类(在原有基础上)
- div.classList.remove('hello')
  - 删除类(在原有基础上)
- div.classList.toggle('hello')
  - 切换类 (有就移除, 没有就加上)

## dataset 操作自定义属性

- **dataset**
- div.dataset.index
  - 获取 / 设置自定义的属性 data-index 的值
  - ⚠️ 如果自定义属性带 - , 获取时应使用 `驼峰命名`

## 元素内容可编辑属性

- contenteditable
  - contenteditable[="true"]
    ```html
    <!-- 可以在 div 中输入内容 -->
    <div contenteditable>
        
    </div>
    ```

## 修改 input 标签的 placeholder 的属性

```css
input::-webkit-input-placeholder {
    color: red;
}
```

# 音视频标签

[参考手册](<https://www.w3school.com.cn/tags/html_ref_audio_video_dom.asp>)

## 容器

- 不管是音频文件或视频文件, 实际上都只是一个容器文件, 这点类似于压缩了一组文件的 zip 文件
- 视频文件 (视频容器) 包含了音频轨道、视频轨道和其他一些元数据
- 视频播放的时候, 音频轨道和视频轨道是绑在一起的
- 元数据包含了视频的封面、标题、子标题、字幕等相关信息

## 主流的音视频文件格式

- 主流视频文件格式 (容器格式)
  - MPEG-4 —— 以 .mp4 为扩展名
  - Flash —— 以 .flv 为扩展名
  - Ogg —— 以 .ogv 为扩展名
  - WebM —— 以 .webm 为扩展名
  - 音视频交错 —— 以 .avi 为扩展名
- 主流音频文件格式 (容器格式)
  - MPEG-3 —— 以 .mp3 为扩展名
  - Aac —— 以 .aac 为扩展名
  - Ogg —— 以 .ogg 为扩展名

## 音视频编解码器

- 视频编解码器
  - H.264
  - VP8
  - Ogg Theora
- 音频编解码器
  - AAC
  - MPEG-3
  - Ogg-Vorbis

## 浏览器支持情况

| Browser   | MP4  | WebM | Ogg  |
| :-------- | :--: | :--: | :--: |
| IE 9      | YES  |  NO  |  NO  |
| FireFox 4 |  NO  | YES  | YES  |
| Chrome    | YES  | YES  | YES  |
| Safari 5  | YES  |  NO  |  NO  |
| Opera 10  |  NO  | YES  | YES  |

## 格式转换 ffmpeg

- window10 下环境变量的入口
  > 控制面板-->选择大图标-->系统-->高级系统设置-->高级-->环境变量

- 制作MP4 视频
  ```shell
  ffmpeg -i test.mp4 -c:v libx264 -s 1280x720 -b:v 1500k -profile:v high -level 3.1 -c:a aac -ac 2 -b:a 160k -movflags faststart OUTPUT.mp4
  ```

- 制作 WebM 视频

  ```she
  ffmpeg -i test.mp4 -c:v libvpx -s 1280x720 -b:v 1500k -c:a libvorbis -ac 2 -b:a 160k OUTPUT.webm
  ```

- 制作 Ogg 视频

  ```shell
  ffmpeg -i test.mp4 -c:v libtheora -s 1280x720 -b:v 1500k -c:a libvorbis -ac 2 -b:a 160k OUTPUT.ogv
  ```

- 制作Mp3音频

  ```shell
  ffmpeg -i test.mp3 -c:a libmp3lame -ac 2 -b:a 160k OUTPUT.mp3
  ```

- 制作Ogg音频

  ```shell
  ffmpeg -i test.mp3 -c:a libvorbis -ac 2 -b:a 160k OUTPUT.ogg
  ```

- 制作ACC音频   

  ```shell
  ffmpeg -i test.mp3 -c:a aac -ac 2 -b:a 160k OUTPUT.aac
  ```

## 兼容模式

```html
<video width="800" height="600">
    <source src="myvideo.mp4" type="video/mp4"></source>
    <source src="myvideo.webm" type="video/webm"></source>
    <source src="myvideo.ogv" type="video/ogg"></source>
    当前浏览器不支持 video 直接播放, 点这里下载视频 <a href="myvideo.mp4">下载视频</a>
</video>
```

```html
<audio controls>
    <source src="myaudio.mp3" type="audio/mpeg"></source>
  <source src="myaudio.acc" type="audio/aac; codecs='aac'"></source>
  <source src="myaudio.ogg" type="audio/ogg; codecs='vorbis'"></source>
    当前浏览器不支持 audio 直接播放, 点这里下载视频 <a href="myvideo.mp3">下载音频</a>
</audio>
```

## 音视频标签的 attribute 属性

- video 标签的属性
  - width
  - height
  - poster
    - 一个海报帧的 URL, 用于在用户播放或跳帧之前展示的图片
  - src
    - 要嵌到页面的视频的 URL
  - controls
    - 显示或隐藏用户控制界面
  - autoplay
    - 媒体是否自动播放
  - loop
    - 媒体事件循环播放
  - muted
    - 是否静音
  - preload
    - 告诉浏览器是否预先加载视频
    - none
      - 提示浏览器视频不需要缓存
    - metadata (默认值)
      - 作者认为不需要查看该视频, 但是元数据 (视频基本属性) 会抓取
    - auto
      - 用户需要这个视频优先加载
    - 空串
      - 代指 auto
    
- audio 标签的属性
  - src
  - controls
  - autoplay
  - loop
  - muted
  - perload

## 音视频 js 相关属性

- currentTime —— 开始播放到现在所用时间 (可读写)
- duration —— 媒体总时间 (只读，单位: s)

  ```js
  console.log(video.duration)
  console.log(audio.duration)
  ```

- muted —— 是否静音 (可读写, `比 volume 优先级要高`)
  - muted 和 volume 之间不会同步
  - <font color=#c40>设置静音时, 需要手动设置 volome = 0</font>
    ```js
    video.muted = true
    video.volume = 0
    ```

- volume —— 0.0 - 1.0 的音量相对值 (可读写)
- paused —— 是否暂停 (只读)
- ended —— 是否播放完毕 (只读)
- error —— 发生错误的时候, 返回错误代码 (只读)
- currentSrc —— 以字符串的形式返回媒体地址 (只读)

- 视频对象多的部分
  - poster —— 视频播放前的预览图片 (读写)
  - width、height —— 设置视频的尺寸 (读写)
  - videoWidth、videoHeight —— 视频的实际尺寸 (只读)

## 音视频相关js事件

[参考手册](<https://www.w3school.com.cn/tags/html_ref_audio_video_dom.asp>)

- 视频常用事件

  | 事件名                             | 说明                                                         |
  | ---------------------------------- | ------------------------------------------------------------ |
  | abort                              | 在播放被终止时触发,例如, 当播放中的视频重新开始播放时会触发这个事件 |
  | <font color=#c40>canplay</font>    | 在媒体数据已经有足够的数据（至少播放数帧）可供播放时触发     |
  | <font color=#c40>ended</font>      | 播放结束时触发                                               |
  | loadeddata                         | 媒体的第一帧已经加载完毕                                     |
  | loadedmetadata                     | 媒体的元数据已经加载完毕，现在所有的属性包含了它们应有的有效信息 |
  | <font color=#c40>pause</font>      | 播放暂停时触发                                               |
  | <font color=#c40>play</font>       | 在媒体回放被暂停后再次开始时触发。即，在一次暂停事件后恢复媒体回放 |
  | playing                            | 在媒体开始播放时触发（不论是初次播放、在暂停后恢复、或是在结束后重新开始） |
  | <font color=#c40>timeupdate</font> | 元素的 currentTime 属性表示的时间已经改变                    |
  | volumechange                       | 在音频音量改变时触发（既可以是volume属性改变，也可以是muted属性改变) |

## 音视频 js 相关函数

- play () ——   媒体播放
- pause() —— 媒体暂停
- load() ——    重新加载媒体
  - 通过 source 重置 src 属性, 必须 load() 重新加载后才能播放
    ```js
    var video = document.querySelector('video')
    var source = document.querySelectorAll('source')
    
    source[0].src = '123.mp4'
    video.load()
    ```

- 解决 Uncaught (in promise) DOMException 问题
  ```js
  var playPromise = video.play()
  if (playPromise !== undefined) {
      playPromise.then(() => {
          video.play()
      }).catch(() => {
      })
  }
  ```

## canvas 和 video 结合

```html
<video src="https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_AnimatedShot_Soldier76_Hero.mp4"
     autoplay preload="metadata" width="0" height="0"></video>
<canvas width="400" height="400"></canvas>

<script>
    var video = document.querySelector('video')
    var canvas = document.querySelector('canvas')
    if (canvas.getContext) {
        var ctx = canvas.getContext('2d')

        video.addEventListener('loadeddata', function () {
            setInterval(function () {
                ctx.drawImage(video, 0, 0, canvas.width, canvas.height)
            })
        })
    }
</script>
```

# 新增 API

## 网络状态

以前我们可以通过 `window.navigator.onLine` 来检测用户当前的网络状况，返回一个布尔值，*现在已不用*

HTML5 给我们提供了 2 个事件

- online 
  - 用户连接网络时调用
- offline
  - 用户网络断开时调用

⚠️ 他们监听的都是 window 对象

```js
window.addEventListener('online', function () {
  $('span').html('网络已连接').fadeIn(500).delay(1000).fadeOut()
})

window.addEventListener('offline', function () {
  $('span').html('网络断开。。。').fadeIn(500).delay(1000).fadeOut()
})
```

## 全屏

HTML5 允许用户自定义网页上任一元素全屏显示

### DOM 元素的 API

- Node.webkitRequestFullScreen()
  - 开启全屏显示
  - ⚠️ **不同浏览器需要添加前缀**
  - webkitRequestFullScreen()   mozRequestFullScreen() 

### document 的属性和API

- document.webkitCancelFullScreen()
  - 关闭全屏显示
  - **取消全屏要通过 document 调用，不是通过 node 调用**
- *要加浏览器前缀*
- document.webkitIsFullScreen
  - 检测当前是否处于全屏状态

### 全屏伪类选择器

- :full-screen .box {}    :-webkit-full-screen .box {}    :-moz:-full-screen .box {}

### 示例

```js
$('.btn').onclick = function () {
  if ($('img').requestFullScreen) {
    $('img').requestFullScreen()
  } else if ($('img').webkitRequestFullScreen) {
    $('img').webkitRequestFullScreen()
  } else if ($('img').mozRequestFullScreen) {
    $('img').mozRequestFullScreen()
  } else if ($('img').msRequestFullscreen) { // 微软里边 s 是小写
    $('img').msRequestFullscreen()
  } else {
    $('img').oRequestFullScreen()
  }
}

$('.btn').onclick = function () {
  document.webkitCancelFullScreen() // 一定是 document 来调用
}

div:-webkit-full-screen { // 全屏状态下 div 背景设置为粉色
  background: pink;
}
```

### 全屏实现

- 全屏实现不需要我们写, 直接调用 API 就行

  ```js
  var isFullScreen =  false
  
  full.onclick=function() {
    if(isFullScreen) {
        isFullScreen = false
        if (document.exitFullscreen) {  
            document.exitFullscreen();  
        }  
        else if (document.mozCancelFullScreen) {  
            document.mozCancelFullScreen();  
        }  
        else if (document.webkitCancelFullScreen) {  
            document.webkitCancelFullScreen();  
        }
        else if (document.msExitFullscreen) {
              document.msExitFullscreen();
        }
    } else {
        isFullScreen = true
        var docElm = document.documentElement;
        //W3C  
        if (docElm.requestFullscreen) {  
            docElm.requestFullscreen();  
        }
        //FireFox  
        else if (docElm.mozRequestFullScreen) {  
            docElm.mozRequestFullScreen();  
        }
        //Chrome等  
        else if (docElm.webkitRequestFullScreen) {  
            docElm.webkitRequestFullScreen();  
        }
        //IE11
        else if (docElm.msRequestFullscreen) {
          docElm.msRequestFullscreen();
        }
    }
  }
  ```

### 全屏带来的问题

我们将一个元素全屏后，该元素的会覆盖页面上的所有元素，设置层级也不管效果

- 如何解决？

  1. 用一个父元素包裹需要进行全屏的元素和不想被覆盖的其他元素

  2. **把全屏操作加给 父元素**

  3. 根据需求来设置全屏和取消全屏下的元素宽高

     ```html
     <style>
     .box1 {
         width: 300px;
         height: 300px;
         background-color: pink;
         position: relative;
       }
     
       .box2 {
         height: 100%;
         background: #000;
       }
       button {
         position: absolute;
         left: 0;
         bottom: 0;
         width: 100%;
         height: 20px;
         background-color: yellow;
       }
     </style>
     <div class="box1">
         <div class="box2"></div>
         <button>全屏/取消全屏</button>
     </div>
     
     <script>
     var box1 = document.querySelector('.box1')
     var btn = document.querySelector('button')
     btn.onclick = function () {
       if (document.webkitIsFullScreen) {
         document.webkitCancelFullScreen()
         ......
       } else {
         box1.webkitRequestFullScreen()    
         ......
       }
     
     }
     </script>
     ```

## 文件读取

通过 FileReader 对象我们可以读取本地存储的文件，使用 [File ](https://developer.mozilla.org/zh-CN/docs/DOM/File)对象来指定所要读取的文件或数据。其中File对象可以是来自用户在一个元素上选择文件后返回的[FileList ](https://developer.mozilla.org/zh-CN/docs/DOM/FileList)对象，也可以来自由拖放操作生成的  [DataTransfer](https://developer.mozilla.org/zh-CN/DragDrop/DataTransfer)

- files 对象

- 我们可以通过为表单元素添加 multiple 属性，通过<input>上传文件后得到的是一个 Files 对象（伪数组形式）

- [FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader) 对象
  - HTML5新增内建对象，可以读取本地文件内容
  - `var reader = new FileReader()`  可以实例化一个对象
  - 实例方法
    - readAsText()          读取文件内容（*多用于读取文本文件*）
    - readAsDataURL()  以 DataURL 形式读取文件 (*读取图片文件*)

  - 事件
    - [onload](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader/onload)

    ```js
    var file = document.querySelector('[type=file]')
    
    file.onchange = function () {
      console.log(this.files) // 上传文件的集合
      
      var reader = new FileReader()
      // reader.readAsText(this.files[0]) // 选取上传的第一个文件
      reader.readAsDataURL(this.files[0])
      
      reader.onload = function () { // 当读取完成时
        console.log(this.result) // 获取文件内容, 这里得到的并不是路径字符串，而是图片编码后的内容
        $('img').attr('src', this.result) // 实现图片预览
      }
    }
    ```

## 地理定位

在HTML规范中，增加了获取用户地理信息的API，这样使得我们可以基于用户位置开发互联网应用，即基于位置服务 (Location Base Service)

[API 详解](http://www.w3school.com.cn/html5/html_5_geolocation.asp)

  - navigator.geolocation.getCurrentPosition(successCallback, errorCallback) 
    - 获取当前地理信息

  - navigator. geolocation.watchPosition(successCallback, errorCallback)
    - 重复获取当前地理信息

  - 当成功获取地理信息后，会调用 succssCallback，并向其传递一个包含位置信息的对象 position
    - position.coords.latitude              纬度

      position.coords.longitude           经度

      position.coords.accuracy            精度

      position.coords.altitude              海拔高度

  - 当获取地理信息失败后，会调用errorCallback，并向其传递错误信息对象 error

### 第三方 API 来获取位置

HTML5 默认采用谷歌地图来获取位置，可能获取不到；

在现实开发中，通过调用第三方API（如百度地图）来实现地理定位信息，这些API都是基于用户当前位置的，并将用位置（经/纬度）当做参数传递，就可以实现相应的功能

- [百度地图API](https://lbsyun.baidu.com/index.php?title=jspopularGL)

使用步骤
  1. 拷贝对应的 demo 代码到本地
  2. 申请密钥
  3. 密钥填入 `<script type="text/javascript" src="//api.map.baidu.com/api?type=webgl&v=1.0&ak=您的密钥"></script>`
  4. [坐标拾取器](http://api.map.baidu.com/lbsapi/getpoint/index.html) 获取要展示的中心点坐标
  5. 将获取到的坐标替换掉代码中的默认坐标

### 百度地图名片

- [地图名片](http://api.map.baidu.com/mapCard/setInformation.html)
  - 要先登录

## 拖拽

在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放

- Drag —— 拖拽
- Drop —— 释放
  - 拖拽指的是鼠标点击源对象后一直移动对象不松手，一但松手即释放了
- ⚠️ 首先，要设置元素为可拖拽
  - 就是标签元素要设置 `draggable=true`，否则不会有效果 
    - **一定是 draggable="true", 只添加 draggable 是没有效果的**
  - *链接和图片默认是可拖动的，不需要 draggable 属性*

### 拖拽事件

**被拖动的源对象可以触发的事件：**

(1) ondragstart：源对象开始被拖动
(2) ondrag：源对象被拖动过程中(鼠标可能在移动也可能未移动)
(3) ondragend：源对象被拖动结束

**目标对象可以触发的事件：**

⚠️ <font color=#c40>参考的是鼠标点的位置</font>

(1) ondragenter：源对象进入目标对象时
(2) ondragover：源对象悬停在目标对象上方时
(3) ondragleave：源对象离开目标对象时
(4) ondrop：源对象在目标对象上松手时 （*默认是被浏览器阻止的，要执行 ondrop 需要在 ondragover 中阻止默认行为*）

拖拽API总共就是7个函数！！

### DataTransfer

在进行拖放操作时，`DataTransfer` 对象用来保存被拖动的数据。它可以保存一项或多项数据、一种或者多种数据类型

- [常用 API](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer/setData)
  - setData(type, data)
    - type —— 要存储的数据类型，可取值为 text/plain 和 text/uri-list
    - data —— 要存储的数据
  - getData(type)
    - 获取存储的数据

### 实现如下拖拽效果

![](/images/拖拽.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        div {
            float: left;
            width: 300px;
            height: 300px;
            border: 2px solid pink;
            margin: 20px;
        }
        p {
            background-color: skyblue;
        }
        .div2 {
            border-color: yellow;
        }
        .div3 {
            border-color: yellowgreen;
        }
    </style>
</head>
<body>
    <div class="div1">
        <p id="p1" draggable="true">我是 p1</p>
        <p id="p2" draggable="true">我是 p2</p>
        <p id="p3" draggable="true">我是 p3</p>
    </div>
    <div class="div2"></div>
    <div class="div3"></div>

    <script>
        // 我们可以将拖拽事件委托给 document, 这样页面上所有带有 draggable="true" 属性的元素都可以拖拽
        // 不必一个一个得添加监听
        document.ondragstart = function (e) {
            // console.log(e.target) // 拖拽源对象
            e.dataTransfer.setData('text/plain', e.target.id)  // 存储源对象的信息
        }

        document.ondragover = function (e) {
            //console.log(e.target) // 进入的目标对象
            return false
        }

        document.ondrop = function (e) { // 默认被浏览器阻止，要触发此事件，需要在 ondragover 中阻止默认行为
            console.log(e.target)  // 目标对象

            var id = e.dataTransfer.getData('text/plain') // 获取存储的源对象的信息

            // 实现拖拽到别的 div 中放手就添加到对应的 div 中
            if (!['div1', 'div2', 'div3'].includes(e.target.className)) return 
            e.target.appendChild(document.getElementById(id))
        }
    </script>
</body>
</html>
```

## Web存储

在本地存储大量的数据，HTML5规范提出了相关解决方案

### 特性

1. 设置、读取方便、页面刷新不丢失数据
2. 容量较大，sessionStorage 约 5M、localStorage 约 20M
3. <font color=#c40>只能存储字符串</font>，可以将对象 JSON.stringify() 编码后存储
4. 可以通过 检查 ==> 控制台 == > Application 查看

### window.sessionStorage对象

1. 生命周期为关闭浏览器窗口
2. 在同一个窗口(页面)下数据可以共享

### window.localStorage对象

1. <font color=#c40>永久生效</font>，*除非手动删除 关闭页面也会存在*
2. 可以多窗口（页面）共享（同一浏览器可以共享）

### 方法详解

⚠️ sessionStorage 和 localStorage 用法完全一样

- setItem(key, value) 
  - 设置存储内容

- getItem(key) 
  - 读取存储内容

- removeItem(key) 
  - 删除键值为key的存储内容

- clear() 
  - 清空所有存储内容

### 其他

WebSQL、IndexDB

## 应用缓存

HTML5 中我们可以轻松的构建一个离线（无网络状态）应用，只需要创建一个 cache manifest 文件

### 优势

1. 可配置需要缓存的资源
2. 网络无连接应用仍可用
3. 本地读取缓存资源，提升访问速度，增强用户体验
4. 减少请求，缓解服务器负担

### 缓存清单

一个普通文本文件，其中列出了 **浏览器应缓存以供离线访问的资源**，推荐使用.appcache为后缀名

- 例如我们创建了一个名为 demo.appcache 的文件，然后在需要应用缓存的页面的根元素(html)添加属性manifest="demo.appcache"，路径要保证正确

### manifest文件格式

1. 顶行写 CACHE MANIFEST
2. CACHE: 换行 指定我们需要缓存的静态资源，如 css、image、js 等
3. NETWORK: 换行 指定需要在线访问的资源，可使用通配符
4. FALLBACK: 换行 当被缓存的文件找不到时的备用资源

   ```shell
   CACHE MANIFEST
   # 上面必须是第一行 
   CACHE:
   
   # 此部分写需要缓存的资源 （#是注释的意思）
   
   ./images/img1.jpg
   ./images/img2.jpg
   ./images/img3.jpg
   ./images/img4.jpg
   
   
   NETWORK:
   
   # 此部分要写需要有网络才可访问的资源，无网络刚不访问
   
   ./images/img1.jpg
   ./images/img2.jpg
   
   # * 表示所有
   
   FALLBACK:
   
   # 当访问不到某个资源的情况下，自动由另一个资源替换
   
   ./images/img4.jpg  ./images/img5.jpg
   
   
   ```

### 其它

1. CACHE: 可以省略，这种情况下将需要缓存的资源写在CACHE MANIFEST
2. 可以指定多个 CACHE: NETWORK: FALLBACK:，无顺序限制
3. `#` 表示注释，只有当 demo.appcache 文件内容发生改变时或者手动清除缓存后，才会重新缓存
4. chrome 可以通过 chrome://appcache-internals/ 工具和离线（offline）模式来调试管理应用缓存

## 多媒体

方法：load()、play()、pause()

属性：currentSrc、currentTime、duration

事件：oncanplay，  ontimeupdate，onended 等

⚠️ 以上属性和 API 只能通过原生 JS 来操作

[**参考文档**](http://www.w3school.com.cn/tags/html_ref_audio_video_dom.asp)

http://www.w3school.com.cn/tags/html_ref_audio_video_dom.asp