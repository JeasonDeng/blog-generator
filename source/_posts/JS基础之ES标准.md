---
title: JS基础之ES标准
date: 2016-06-23 12:20:30
updated: 2016-06-23 12:20:30
tags: JS 基础
---
# JavaScript 教程

[MDN JS 教程](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)
[阮一峰 JS教程](http://javascript.ruanyifeng.com/)

## 什么是语言?

人和计算机交流的工具, 我们通过语言来控制、操作计算机

## JS 是什么

诞生于 1995 年, 前身是 LiveScript, 前期主要用于 **网页验证**

- <font color=#c00>一种运行在客户端的脚本语言(不需编译, 直接运行)</font>
- 作用
  - 解决浏览器和用户之间的交互问题

<!-- more -->

## 特点

1. 解释型语言
   - 不需编译, 直接运行

2. 动态类型语言
   - 变量可以保存任意类型的数据

3. 基于对象的语言

## JS 的三个部分

ECMAScript(标准) —— js 的基本语法 
DOM —— 文档对象模型       =>   如何通过 js 去操作网页
BOM —— 浏览器对象模型    =>   如何通过 js 去操作浏览器

## 几个输出语句

1. alert('哈哈')
   - 弹出消息框
2. prompt()

   - 弹出一个带输入框的提示框
   - 该函数需要一个字符串作为参数, 该字符串为提示框的提示文字
   - 用户输入的内容会作为该函数的返回值, 返回值为 `string 类型`
```js
var score = prompt('请输入小米的成绩:')
alert(score)

// 输入三个数, 按从小到大输出
var num1  = +prompt('请输入第一个数:')
var num2  = +prompt('请输入第二个数:')
var num3  = +prompt('请输入第三个数:')
if (num1 < num2 && num1 < num3) {
    // num1 最小
    if (num2 < num3) {
        alert(num1 + ',' + num2 + ',' + num3)
    } else {
        alert(num1 + ',' + num3 + ',' + num2)
    }
} else if (num2 < num1 && num2 < num3) {
    // num2 最小
    if (num1 < num3) {
        alert(num2 + ',' + num1 + ',' + num3)
    } else {
        alert(num2 + ',' + num3 + ',' + num1)
    }
} else {
    // num3 最小
    if (num1 < num2) {
        alert(num3 + ',' + num1 + ',' + num2)
    } else {
        alert(num3 + ',' + num2 + ',' + num1)
    }
}
```

3. document.write('hello')
   - 向 body 中写入内容

4. console.log('hello')
   - 向控制台输出内容

5. console.time('test')
   - 开启一个定时器

6. console.timeEnd('test')

   - 终止定时器
   - 在一段代码开始地方写上 console.time('test'), 结束地方写上 console.timeEnd('test')
   - 可以测试这一段代码执行所花费的时间 `test: 4.03ms`

## JS 代码写在哪里?

1. 可以写在标签的 onclick 属性中
```js
<button onclick="alert('点我干嘛')">点点</button>
```

2. 可以写在超链接的 href 中, 点击超链接时执行代码
```html
<a href="javascript:alert('点我干嘛?')">点我</a>
<a href="javascript:;">点我</a>  // 不跳转
```
3. 可以写在 script 标签里
4. 可以写在外部, html 里引入
   - 方便在不同的页面中引入, 也可以利用到 浏览器的缓存机制

## JS 代码风格问题

[JavaScript Standard Style](<https://standardjs.com/rules-zhcn.html>)

​   ⚠️ `(、[、` 开头的语句前边记得要加上`;`

[Airbnb](<https://github.com/airbnb/javascript>)

# 几个基本概念

1. 字面量和变量

   - 字面量 —— 不可改变的值 (具体的值 1、2、null、true)

2. 变量

   - 变量可以 <font color=#c00>用来保存字面量</font>, 而且变量的值是可以改变的

   - 更方便我们使用, 所以我们在开发中都是 `使用变量去保存一些字面量`
   ```js
   // 声明变量
   var a = 10
   ```

3. js 注释

   - //      —— 单行注释
   - /**/  —— 多行注释

4. 标识符

   - 变量名、函数名、属性名都叫标识符 (*自己命名的*)
   - 标识符可以包含字母、数字、下划线、$, <font color=#c00>不能以数字开头</font>
   - 一般使用驼峰命名法
   - 标识符不能是关键字或保留字
   - <font color=#c00>js 中严格区分大小写</font>

![](/images/关键字保留字.png)

# JS 的数据类型

就是指字面量的类型，js 中一共 `6 种` 数据类型

## 基本数据类型 (5 种)

1. String

   - 使用字符串需要引号, 推荐使用单引号

   - 可以外边用单引号, 里边用双引号; (也可以外边双, 里边单)

   - 可以使用 \ 作为转义字符

     - `\n` 表示换行

     - `\\`表示斜杠

     - `\双引号` 表示双引号
```js
var str = "我说:\"我没吃饭\""
var a = "\\"  \
```

2. Number

   - JS 中的数值都是 Number 类型 (包括整数和小数)
   - 数值的最大值 —— Number.MAX_VALUE      数值的最小值 —— Number.MIN_VALUE
   - Infinity —— 正无穷      
   - -Infinity —— 负无穷
```js
var a = Infinity // 字面量, 直接使用
typeof Infinity  // 'number'
```
> a. **NaN** —— 也是一个特殊的数字, 表示的是 not a number
> b. 浮点数进行计算, 可能得到一个不正确的结果
> c. 二进制数 以 0b 开头 (0b10) ;   八进制数 以 0 开头 (012) ;   十六进制数 以 0x 开头 (0x123)

3. Boolean

   - 只有两个 true / false
   - 主要用来做 **逻辑判断**

4. Null

   - Null 类型的值只有一个 null
   - null 专门用来表示为空的 **对象**
     ```js
     var a = null
     console.log(typeof a) // 'object'
     ```

5. Undefined

   - Undefined 类型的值只有一个 undefined
   - 声明一个变量, 但不给它赋值, 它的值就是 undefined
     ```js
     var a 
     console.log(typeof a) // 'undefined'
     ```

⚠️  这 5 种属于基本数据类型(**单一的值, 值和值之间没有任何联系**); Object 属于引用数据类型

### 基本数据类型之间的转换

#### 其他类型转换为 String 类型

1. 调用<font color=#c00>.toString()方法</font>

   - 该方法不会影响原变量, 它会返回 **转换的结果**
   - **null 和 undefined 没有 .toString() 方法**
```js
var a = 123
console.log(a.toString()) // '123'
```

2. String() 函数

   - 该方法不会影响原变量, 它会返回 **转换的结果**
   - 可以转换 null 和 undefined
```js
var a = 123
console.log(String(a)) // '123'

console.log(String(null)) // 'null'
```

3. <font color=#c00>**我们可以为任何数据类型 + '', 就可以将其转为 String 类型**</font>
   ```js
   var c = 123
   c = c + ''  // '123'
   ```

#### 其他类型转换为 Number 类型

1. Number() 函数

   - 如果是纯数字的字符串, 则直接转为数字
   - 如果字符串中有非数字的内容, 则转换为 NaN
   - 如果字符串为空, 则转换为 0
   - 布尔类型 true 转换为 1;  false 转换为 0
   - null 转换为 0
   - **undefined 转换为 NaN**  
```js
var a = '543'
console.log(Number(a)) // 543

console.log(Number(undefined)) // NaN
```

2. <font color=#c00>parseInt()</font>
   - 将任意类型数据转为 `整数`
   - **会先将要转换的数据转为字符串**, 再转为整数
   - parseInt(a, 10)  可以使用第二个参数来表示要转换的进制
```js
var a = '123abc456'
console.log(parseInt(a)) // 123

parseInt(10.2)   // 10

console.log(parseInt('abc')) // NaN

console.log(parseInt(true)) // NaN

console.log(parseInt(null)) // NaN

console.log(parseInt(undefined)) // NaN

console.log(parseInt('070', 10)) // 70
```

3. parseFloat()
   - 将任意类型数据转为 `浮点数`
   - **也会先将要转换的数据转为字符串**, 再返回 `字符串` 中的有效部分 
```js
var a = '123.45abc456'
console.log(parseFloat(a)) // 123.45
```

4. <font color=#c00>**可以对其他数据类型 加 +(正号); 或者 * 1; 或者 /1; 或者 -0 将其转为数字类型**</font>
   ```js
   var a = '12'
   a = +a // 12
   
   var b = true
   b = b * 1 // 1
   
   var c = null
   c = c - 0 // 0
   ```

#### 将其他数据类型转换为 Boolean 类型

1. Boolean() 函数
   - **0、NaN、空串、null、undefined 转 boolean 都是 false**
```js
var a = 12
console.log(Boolean(a)) // true
console.log(Boolean(null)) // false
```

2. 对任意类型的数据进行 `两次逻辑非运算`
   ```js
   var a = !!2       // true
   var b = !!'hello' // true
   ```

## 引用数据类型

<font color=#c00>**Object**</font> —— 对象属于一种复合的数据类型, 在对象中可以保存多个不同数据类型的属性

## 基本数据类型 和 引用数据类型 的区别

1. 基本数据类型的<font color=#c00>值直接存储在栈内存里</font>, 值与值之间是独立存在的, 修改一个变量不会影响其他变量
2. 引用数据类型的<font color=#c00>值保存在堆内存里</font>, 而<font color=#c00>变量在栈内存保存的是对象的地址</font>
3. 赋值的区别
   - 基本数据类型赋值  ==>  值传递
   - 引用数据类型赋值  ==>  引用(地址)传递
   - 基本数据类型比较  ==>  比较值
   - 引用数据类型比较  ==>  比较引用(地址)

# 常用运算符 (8 种)

也叫操作符, 可以对一个或多个值进行运算, 并获取运算结果

## typeof

typeof 就是一个运算符, 可以用来获得一个值的类型
- 它将该值的类型 <font color=#c00>**以字符串的形式返回**</font>
```js
var a = 3
console.log(typeof a) // 'number'

console.log(typeof null) // 'object'
console.log(typeof undefined) // 'undefined'
```

## 算数运算符

`+、-、*、/、%`

- 不会改变原变量的值
- 对非 Number 类型的值进行计算, **会将这些值转换为 Number** (字符串 + 除外), 然后再运算
  - 所以, <font color=#c00>**可以对其他数据类型 * 1; 或者 /1; 或者 -0 将其转为数字类型**</font>
  - <font color=#c00>只有字符串 + 的时候特殊; <font color=#c00>**任何值和字符串进行 + 操作, 会进行字符串拼接**</font></font>
  ```js
  var result = true + false
  console.log(result) // 1
  
  var res = 100 - true // 99
  
  var s = 100 - '1'  // 99, 先将字符串 '1' 转为数字 1, 再运算(和 + 不同)
  
  var res = 2 * '8' // 16
   
  var res = 2 * null // 0
  
  var res = '2' * 1 // 2
  ```

## 一元运算符

`++、--、+(正)、-(负)`

- 只需一个操作数
- ++、-- **会立即 **使 `原变量` 的值自增/减 1
  - **a++ 的值等于原值**
  - **++a 的值等于自增后的值**
  - ⚠️ a 是变量; 而 a++ 是一个表达式, 也可以作为一条语句
  ```js
  var a = 1
  console.log(a++) // 1
  console.log(++a) // 3
  
  var d = 20
  d = d++
  console.log(d) // 20
  ```

- `+`  取正数
  - 不会对数字产生影响
  - <font color=#c00>可以把非数字类型转换为数字类型</font>
    ```js
    var s = +'12' // 12
    
    var res = 1 + +'2' + 3 // 6
    ```

- `-`  取反
  - 对非数字会先进行转换为数字类型, 再取反
    ```js
    var a = -true // -1 
    
    var b = -'18' // -18
    ```

## 逻辑运算符 

`&&、||、!`

- 用来做逻辑判断
- 非 Boolean 类型数据会先转为 Boolean 类型

1. !  

   - <font color=#c00>返回布尔值</font>
   - 将操作数转为布尔值再取反
   - 可以对一个非布尔类型数据进行 `两次 ! 操作将其转为布尔值`

2. && 、|| 
   - 将第一个操作数转换为布尔值, 然后再运算

3. &&、|| 还可以用作 **短路运算**

   - fn && fn()
   - a || a = 20
```js
// 非运算
var a = !2  // false
var b = !0  // true
var c = !!2 // true

// 与运算 
// 如果第一个为 true, 则返回后边的值
var ret = 1 && 2
console.log(ret) // 2

// 与运算 
// 如果第一个为 false, 则直接返回第一个值
var ret = 0 && 2
console.log(ret) // 0

// 或运算 
// 如果第一个为 true, 则直接返回第一个值
var ret = 1 || 2
console.log(ret) // 1

// 或运算 
// 如果第一个为 false, 则返回后边的值
var ret = 0 || 2
console.log(ret) // 2
```

## 赋值运算符

`=、+=、-=、*=、/=、%=`

- 将右侧的值赋值给左侧的变量
  ```js
  var a = 2
  a += 1  // 等价于 a = a + 1
  console.log(a) // 3
  ```

## 关系运算符

`> 、 < 、 >= 、 <= 、 == 、 != 、=== 、 !==`

- <font color=#c00>返回布尔值</font>
- 比较两个值之间的大小关系
- **对于非数字进行比较时, 会先将其转为数字, 再进行比较**
  ```js
  var res = 5 > 5
  console.log(res) // false
  
  console.log(1 > true) // false
  ```

1. **任何值和 NaN 比较, 结果都是 false**
```js
console.log(10 > 'hello') // false
```

2. <font color=#c00>如果两侧都是字符串</font>, 会 `比较其字符编码(Unicode 编码)`, 一位一位进行比较, 如果两位一样, 再比较下一位  (可以进行英文名排序)
   - **⚠️ 在比较两个字符串型的数字时, 一定要转型**
```js
console.log('11' > '5') // false
console.log('11' > +'5') // true, 加 + 转型

console.log('a' < 'b') // true

console.log('abc' < 'b') // true
```

3. 使用 == 时, 如果两个值类型不同, 会自动做类型转换, 再进行比较
```js
console.log(10 == 4) // false

console.log('1' == 1) // true

// 特殊情况
console.log(null == 0) // false
console.log(undefined == null) // true

// NaN 不和任何值相等, 包括它本身
console.log(NaN == NaN) // false

// 判断 b 的值是否是 NaN
var b = NaN
console.log(b == NaN) // false, 无法判断 b 是否为 NaN
console.log(isNaN(b)) // true
```

4. === 和 !== 
   - **===不会做类型转换, 类型不同直接返回 false**
   - **!==不会做类型转换, 类型不同直接返回 true**
```js
console.log('1' === 1) // false

console.log(undefined === null) // false
```

## <font color=#c00>条件运算符  (三元运算符)</font>

条件表达式 ? 表达式 1  : 表达式 2

- 如果条件表达式的值为非布尔值, 则先将其转为布尔值
```js
var max = a > b ? a : b

true ? alert('11') : alert('22')

// 求 3 个数中的最大值
var max = a > b ? (a > c ? a : c) : (b > c ? b : c) 

'' ? alert('hehe') : alert('haha') // 'haha'
```

## 逗号运算符

同时声明所个变量时使用
```js
var a, b, c

var a = 1, b = 2, c = 3
```

## 运算符的优先级

>  .  >  new  >  ()  >   ++   >   算数   >   关系   >   逻辑( *! > && > ||*)   >   赋值

```js
var result = 1 || 2 && 3 
console.log(result) // 1
```

# 语句

我们的程序是由一条一条的语句构成的

- 语句是按照 `自上向下` 的顺序执行的

## 流程控制语句

根据一定的条件控制程序的执行流程

### if 语句 (条件判断语句)

1. if (条件表达式) { 语句 }
   - 会先对条件表达式进行求值判断
2. if (条件表达式) { 语句 } else { 语句 }
3. if (条件表达式) { 语句 } else if { 语句 } else { 语句 }
4. ⚠️ if 条件表达式如果是赋值表达式的情况
```js
if (a = 10) { // 赋值表达式是恒成立的, 相当于 if (true)
    console.log('hehe')
}
```

### switch 语句 (条件分支语句)
```js
switch (条件表达式) {

    case 表达式:

        语句......

        break; 

    case 表达式:

        语句......

        break; 

    default:

        语句......

        break; 

}
```

- 执行时, 依次将 case 后的表达式的值与 switch 条件表达式的值进行 `全等(===)比较`
  - 如果比较结果为 true, 则执行 case 后的语句
    - 如果没有 break, 则往后的语句都会执行
    - 所以, **一般都会在语句后加 break**
  - 如果比较结果为 fasle, 则继续向下比较
  - 如果所有的比较结果都为 false, 则执行 default 后的语句
- switch 语句和 if 语句的功能是重复的, 使用 switch 也可以实现 if 的功能
  ```js
  // 60 分以上及格
  var score = 67
  switch(parseInt(score / 10)) {
    case 10:
    case 9:
    case 8:
    case 7:
    case 6:
      console.log('及格')  
      break
    default:
      console.log('不及格')  
      break
  }
  
  // or
  switch(true) {
    case score >= 60:
      console.log('及格')  
      break
    default:
      console.log('不及格')  
      break
  }
  ```

## 循环语句

反复地执行一段代码多次

### while 循环

while (循环条件) { 循环体 }
```js
var money = 1000
var count = 0
while(money < 5000) {
  money *= 1.05
  count++
}
console.log(count)
```

1. <font color=#c00>break 关键字可以立即终止离它最近的循环</font>
```js
var n = 1
while(n < 10) {
  console.log(n++)
    if (n == 3) {
        // 退出循环
        break
    }
}
// 输出: 1 2 
```

2. <font color=#c00>continue 关键字跳过本次循环</font>
```js
for (var i = 0; i < 5; i++) {
  if (i == 2) {
    continue
  }
  console.log(i)
}
// 输出: 0 1 3 4
```

### do...while 循环

do {

​   循环体

} while (条件表达式)

- **循环体至少执行一次**

### for 循环

for (初始化表达式 1 ; 表达式 2 ; 更新表达式 3) { 循环体 }

- 初始化表达式只会执行一次
- 可以把初始表达式放在外边
  ```js
  var i = 0
  for(; i < 10; i++) {
    
  }
  ```

- 可以为循环语句加一个标识, 来标识当前循环
  ```js
  outer:
  for (var i = 0; i < 5; i++){
      for (var j = 0; j < i; j++) {
        break outer // 终止外层循环, 内层也不会再执行
          console.log(j)
      }
  }
  ```
  ```js
  // 输出 1-100 之间的质数
  for (var i = 2; i < 100; i++) {
      var flag = true // 判断 是不是质数
      for (var j = 2; j <= Math.sqrt(i); j++) {
          if (i / j == 0) {
              flag = false // 不是质数
              break // 终止内层循环
          }
      }
      if (flag) {
          console.log(i)
      }
  }
  ```

# 对象(Object)

1. 以后看到的数据如果不是基本类型的都是 Object 类型
2. 基本数据类型都是单一的值
   - 值和值之间没有任何联系, 不能成为一个整体
3. 对象 —— 一种复合的数据类型, 在对象中可以保存多个不同类型的数据

## 对象的分类 (3 种)

1. 内置对象

   - 由 ES 标准中定义的对象, 在任何的 ES 的实现中都可以使用
   - `Math、String、Number、Boolean、Function、Object、etc`

2. 宿主对象

   - 由 `JS 运行环境`提供的对象, 目前来讲主要指 `由浏览器提供` 的对象
   - BOM、DOM
     - document、console

3. 自定义对象
   - 由开发人员自己创建的对象

## 创建对象

1. 构造函数创建
```js
// 构造函数是专门用来创建对象的函数
var obj = new Object()

// 添加属性
obj.name = '章三'
obj.age = 18
obj.sayHello = function () { 
    console.log('hello')
}
// 删除对象的属性 - delete
delete obj.age

// 判断对象中是否有某属性 - in 运算符
console.log('gender' in obj) // false
```

2. 字面量创建对象

   - 属性名可以加引号, 也可以不加
   - 如果使用一些特殊的名字(有空格/下划线等), 必须加引号
```js
var obj = {}

// 添加属性
obj.name = '里斯'
obj.age = 18
```

- 如果 **使用特殊的属性名或者变量**, 不能使用 . 的方式
  - 需要使用另一种方式
  ```js
  // 特殊的属性名
  obj['123'] = 789
  
  // 变量值作为属性名
  var n = '123'
  console.log(obj[n]) // 789
  ```

- js 对象的属性值, 可以是任意数据类型
  - 函数作为对象的属性值, 就称这个函数是这个对象的方法
  ```js
  obj.sayHello = function () {
    console.log('hello')
  }
  obj.sayHello()  // 调用 obj 的 sayHello 方法
  ```

## 枚举对象中的属性

for in 遍历对象
```js
for (var key in obj) {
    console.log(key + '---' + obj[key])
}
```

## JSON

JavaScript Object Notation ——  JS 对象表示法

- <font color=#c00>**JSON 就是特殊格式的字符串**</font>, 
  - 这个字符串可以被任意语言所识别, 并且可以转换为任意语言中的对象
  - JSON 在开发中主要用来做 `数据的交互`
- JSON 和 JS 对象的格式一样, 只不过<font color=#c00> **JSON 字符串中的属性名必须加双引号**</font>

### JSON 分类

1. 对象形 —— '{}' 
```js
var obj = '{"name": "章三", "age": 18}'
```

2. 数组形 —— '[]'
```js
var arr = '[1, 2, 3, "hello"]'
```

### JSON 中允许的值

1. 字符串
2. 数值
3. 布尔值
4. null
5. *对象(普通), 不包含函数对象*
6. 数组

⚠️ 不能有 `undefined` 和 `函数`

### JSON 字符串 转 JS 对象

在 JS 中, 为我们提供来一个工具类 JSON
- 这个对象可以帮助我们将一个 JSON 字符串转为 JS 对象, 也可以将一个  JS 对象转为 JSON 字符串

1. <font color=#c00>**JSON.parse()**</font>
   - 将 JSON 字符串转为 JS 对象
   - `需要一个 JSON 字符串作为参数`, 返回一个 JS 对象

2. <font color=#c00>**JSON.stringify()**</font>
   - 将 JS 对象转换为 JSON字符串
   - `需要一个 JS 对象作为参数`

3. eval() (不常用)
   - 这个函数 `可以直接执行将一段字符串形式的 JS 代码, 并将结果返回`
   - 如果字符串中含有大括号, 它会将大括号当成是代码块; 如果不希望将其当成代码块解析, 则需要给字符串加括号
```js
var str = 'alert("hello")'
eval(str)

var obj = '{"name": "章三"}'
console.log(eval('(' + obj + ')')) // { name: '章三' }
```

# 函数

函数也是一个对象, 里边保存的是可执行代码

- 函数中可以 **封装一些功能(代码段), 在需要时可以调用这些代码**
- 使用 typeof 检查一个函数对象时, 结果是 `function`
- 封装到函数中的代码 <font color=#c00>不会立即执行, 只有在调用时执行</font>

## 函数的创建

```js
// 1.构造函数创建
var fun = new Function('console.log("hello world")')
fun() // 调用函数

// 2.函数声明方式创建
function fun([形参1, 形参2...]) {
    console.log('hello world')
}
console.log(fun) // 'function fun() {...}'

// 3.使用函数表达式的方式来创建
var 函数名 = function ([形参1, 形参2...]) {
    
}
```

## 函数的参数

形参 —— 在函数<font color=#c00>声明时</font>, 形参就相当于在函数内部声明了对应的变量, 但是并没有赋值

实参 —— 在函数<font color=#c00>调用时</font>, () 中指定的参数;  实参将会赋值给对应的形参

- 调用函数时, 如果实参的数量少于形参, 则没有对应实参的形参将会是 `undefined`
  ```js
  function sum(x, y) {
      console.log(x + y)
  }
  sum(1) // NaN, y 默认是 undefined
  ```

## 函数的返回值

使用 return 关键字来设置函数的返回值

- return 后的值将会作为函数的执行结果返回
- return 之后的语句都不会执行
- 如果 return 后不跟任何值或者没有 return , 函数将返回 **undefined**
  ```js
  function sum(x, y, z) {
      return x + y + z
  }
  
  var ret = sum(1, 2, 3)
  
  console.log (ret) // 6
  ```

- 返回值可以是 **任意数据类型**, 也可以是一个函数
  ```js
  function f3 () {
    function f4 () {
      console.log('hehe')
    }
    return f4
  }
  
  f3()() // 'hehe'
  // 相当于
  var a = f3()
  a()
  ```

- return 可以结束整个函数

## 立即执行函数 (IFEE)
```js
// 只会执行一次
// 相当于 函数对象()
(function () {
    alert('匿名函数')
})()
```

# 作用域

指一个变量的作用范围

## 全局作用域

1. <font color=#c00>直接写在 script 标签中的代码, 都在全局作用域</font>

2. 全局作用域在页面打开时创建, 在页面关闭时销毁

3. 全局作用域中有一个**全局对象 window**, 可以直接使用, 它代表的是浏览器窗口

4. **在全局作用域中创建的变量都会作为 window 对象的属性保存**

5. 在全局作用域中创建的函数都会作为 window 的方法保存
6. 全局作用域中的变量都是全局变量
   - 在页面的任意部分都可以访问到

```js
var a = 10
console.log(a) // 10
console.log(window.a) // 10
```

```js
function fun(a) {
    console.log(a)
}
fun('hello') // 'hello'
window.fun('hello') // 'hello'
```

## 函数(局部)作用域

在 `函数内部` 创建的变量

1. 调用函数时创建函数作用域, 函数执行完毕, 函数作用域销毁
2. **在函数作用域中可以访问全局变量, 在全局作用域中, 无法访问到函数作用域的变量**

```js
var a = 10
function fun () {
  var b = 20
  console.log(a)
}
fun() // 10
console.log(b) // b is not defined
```

3. 在函数作用域中操作一个变量时, 会先在自身作用域中查找

   - 如果自身作用域中有, 则直接使用
   - 如果自身作用域中没有, 则在上一级作用域中查找
     - 直到全局作用域, 如果全局作用域还是没有
     - 则报错: `变量 is not defined`

4. 在函数中要使用全局的变量可以使用 `window.`
```js
var a = 10

function fun () {
  var a = 20
  console.log(a) // 20
  console.log(window.a) // 10
}
```

5. 在函数作用域中也有 `声明提前`
```js
var a = 10

function fun () {
  console.log(a) // undefined
  var a = 20
}
```

# 声明提前

## 变量的声明提前

1. 使用 var 声明的变量, 会 `在所有的代码执行之前被声明, 不会被赋值`
2. 如果声明变量时不使用 var , 则变量不会被声明提前, 会成为 window 的属性
```js
console.log(a) // undefined
var a = 12

// 相当于
var a
console.log(a)
a = 12

console.log(a) // error: a is not defined
a = 12

function fun () {
  console.log(d) 
  d = 100 // 不会提前, 相当于 window.d = 100
}
fun() // d is not defined
console.log(d) // 100
```

## 函数的声明提前

1. `使用函数声明方式创建的函数`, 它会在所有的代码执行之前就被创建, 可以在声明前调用
2. 使用函数表达式创建的函数, 不会被声明提前, 不能在声明前调用
```js
fun() // 我是 fun 函数
fun2() // error: undefined is not a function
console.log(fun2) // undefined

// 会被提前创建
function fun() {
    console.log('我是 fun 函数')
}

// 不会被提前创建
var fun2 = function () {
    console.log('我是 fun2 函数')
}
```

# this 对象

解析器在调用函数时, 每次都会向函数内部传递两个隐含的参数 —— this & arguments

- this 指向的是一个对象 (函数执行的上下文对象)
- <font color=#c00>**根据函数调用方式** 的不同, this 会指向不同的对象</font>
  - **以函数的形式调用时, this 永远都是 window**
  - **以方法的形式调用时, this 就是调用方法的对象**
    ```js
    function fun () {
        console.log(this)
    }
    
    var obj = {
        name: '章三',
        sayHello: fun
    }
    
    // 以函数的形式调用
    fun()  // window
    
    // 以方法的形式调用时
    obj.sayHello() // obj
    ```

## <font color=#c00>**总结 this**</font>

1. 以函数形式调用时, this 是 window

2. 以对象方法的形式调用时, this 是调用方法的对象

3. 以 new 构造函数形式调用时, this 是新创建的那个对象
   - ⚠️ `Foo()  和 new Foo() 的区别 —— 前者 this - window, 而后者 this - 实例对象`

4. 使用 call() 和 apply() 时, this 是指定的那个对象

5. 在事件响应函数中, 事件给谁绑定, this 就是谁

# arguments 对象

在 `调用函数时`, 浏览器每次都会向函数传递两个隐藏参数 —— this & arguments

- arguments 是一个 **类数组对象**(不是数组), 用来 **保存实参**
  - 可以通过索引操作数据, 也可以获取长度
  - arguments.length —— 实参的个数
  - arguments[0] —— 第一个实参
  - arguments[1] —— 第二个实参......
  ```js
  function fun() {
      console.log(arguments.length)
      console.log(arguments[0])
  }
  fun(1, 2, 3) // 3  1
  ```

- arguments.callee 属性
  - 对应一个函数对象, 就是 <font color=#c40>**当前正在执行的函数对象**</font>
  - callee关键字的定义
    - 在函数内部使用，代表当前函数的引用(名字)
    - callee关键字的作用 —— 降低代码的耦合度(*一处代码修改尽量少地引起其他代码的变化*)
      ```js
      function jiecheng(n){
        if(n==1){
          return 1;
        }
      
        //return n * jiecheng(n-1);
      
        //callee 可以保证外部名称的变化，不会引起内部代码的修改，代码耦合度降低
        return n * arguments.callee(n-1);
      }
      
      var jc = jiecheng 
      jiecheng = null
      jc(4) // 24
      ```

# 工厂模式创建对象

可以大批量生成对象

- 这种方式生成的对象的类型都是 'object', 无法区分出不同类型的对象(因为它们都是通过 new Object() 创建的)
```js
function createPerson (name, age, gender) {
  // 创建一个新的对象
  var obj = new Object()
  // 添加属性
  obj.name = name
  obj.age = age
  obj.gender = gender
  obj.sayName = function () {
    alert(this.name)
  }
  // 返回新的对象
  return obj
}

var obj1 = createPerson('zs', 18, '男')
var obj2 = createPerson('ls', 16, '女')
var obj3 = createPerson('ww', 24, '男')
```

# 构造函数

专门用来创建对象的, **构造函数名首字母要大写**

- 构造函数和普通函数的区别就是 `调用方式的不同`

  - 普通函数是直接调用
  - <font color=#c00>构造函数需要使用 new 关键字来调用</font>

- 构造函数的执行流程

  > 1. 立刻创建一个新的对象 (在内存中开辟一块空间)
  > 2. 将函数中的 this 指向新建的对象
  > 3. 逐行执行构造函数中的代码
  > 4. 将新建的对象作为返回值返回

  ```js
  function Person(name, age, gender) {
        this.name = name
      this.age = age
      this.gender = gender
      this.sayHello = function () {
          console.log(this.name)
      }
  }
  
  var per1 = new Person('章三', 18, '男')
  var per2 = new Person('里斯', 28, '女')
  
  // 检查 per1 是不是 Person 的实例
  console.log(per1 instanceof Person) // true
  ```

- 使用同一个构造函数创建的对象, 我们称为同一类对象

  - 通过该构造函数创建的对象, 叫做这个构造函数的一个实例

- 构造函数创建的对象类型为 对应的构造函数

  ```js
  console.log(per1) // Person {}
  
  console.log(per1 instanceof Person) // true
  ```

## 原型 prototype

我们所创建的每一个函数, 解析器都会向函数中添加一个属性 prototype

- 这个属性对应着一个对象, 就是<font color=#c00>**原型对象**</font>

- 如果函数作为普通函数调用, prototype 没有任何作用;

  当函数以构造函数的形式调用时, 它所创建的对象中都会有一个隐含的属性, 指向该构造函数的原型对象;

  我们可以通过 `__proto__` 来访问该属性
  ```js
  function MyClass() {
      
  }
  var mc = new MyClass()
  console.log(mc.__proto__ == MyClass.prototype)
  ```

- 原型对象就相当于一个<font color=#c00>公共的区域</font>, 所有同一个类的实例都可以访问到这个原型对象

- 我们可以将对象中共有的内容, 统一设置到原型对象中

  ```js
  MyClass.prototype.a = 123
  
  MyClass.prototype.sayHello = function () {
      console.log('hello')
  }
  ```

- 当我们访问一个对象的属性或方法时, 会先在对象自身中寻找

  - 如果有则直接使用
  - 如果没有, 则会去原型对象中寻找

  ![](/images/原型对象.png)

- 创建构造函数时, 可以将 **共有的属性和方法统一添加到原型对象中**

  - 避免污染全局作用域

  ```js
  function MyClass() {
      
  }
  MyClass.prototype.name = '我是原型中的name'
  
  var mc = new MyClass()
  mc.age = 18
  
  // 使用 in 检查对象是否含有某个属性时, 如果对象中没有, 但原型上有, 也会返回 true
  console.log('name' in mc) // true
  
  // 可以使用对象的 hasOwnProperty() 检查自身是否含有某属性
  console.log(mc.hasOwnProperty('name')) // false
  ```

- ⚠️ 原型对象也是对象, 它也有原型

  - 当我们使用一个对象的属性或方法时, 会先在自身中寻找
    - 自身如果有, 则直接使用
    - 如果没有, 则去原型对象中寻找
      - 如果原型对象中有, 则使用之
      - 如果没有, 则去原型的原型对象中寻找 `mc.__proto__.__proto__`
      - 直到找到 Object 构造函数的原型 `null`; 如果依然没有, 则返回 `undefined`
        ```js
        console.log(Object.prototype) // null
        ```

- 原型链最多只有两级
  ```js
  console.log(mc.__proto__.__proto__) // [object Object]
  
  console.log(mc.__proto__.__proto__.__proto__) // null
  ```

- 当我们直接在页面中打印一个对象时, 默认输出的是对象的 toString() 方法的返回值

  - 如果我们不希望输出 '[object Object]', 可以为对象添加一个 toString() 方法

  ```js
  function Person(name, age, gender) {
      this.name = name
      this.age = age
      this.gender = gender
  }
  
  var per = new Person('章三', 18, '男')
  
  console.log(per) // [object Object]
  
  // 覆写 toString() 方法
  Person.prototype.toString = function () {
      return JOSN.stringify(this)
  }
  console.log(per) // { 'name': '章三', 'age': '18', 'gender': '男' }, 
  //相当于 console.log(per.toString())
  ```

# call() 和 apply()

函数对象的方法, 需要 **函数对象 **来调用

- 当函数调用 call() 和 apply() 时, 函数都会立即执行
- 在调用 call() 和 apply() 时, 可以将一个对象指定为第一个参数;

  `此时这个对象将会成为函数执行时的 this`
```js
var obj1 = {
    name: '章三',
    sayHello: function () {
        console.log(this.name)
    }
}
var obj2 = {
    name: '里斯',
    sayHello: function () {
        console.log(this.name)
    }
}

obj1.sayHello()  // '章三'
obj1.sayHello.apply(obj2)  // '里斯'
```

- call() 可以将实参在对象之后依次传递  **fun.call(obj, 2, 3)**
- apply() 需要将实参封装到一个数组里  **fun.apply(obj, [2, 3])**

# 数组(Array)

<font color=#c00>**数组也是一个对象**</font>, 和普通对象功能类似, 也是用来存储一些值的

1. 不同的是, 普通对象是使用字符串作为属性名的
   - 而数组是 **使用数字作为索引** 来操作元素的

2. 数组的储存性能比普通对象好

3. <font color=#c00>数组中的元素可以是任意数据类型</font>

4. 如果读取不存在的索引, 返回 `undefined`

5. typeof(arr) 结果是 `object` 

## 创建数组对象

```js
// 1.构造函数创建
var arr = new Array()
console.log(typeof arr) // 'object'
// 添加元素
arr[0] = 10
arr[1] = 20

// 2.字面量创建数组
var arr = [1, 2, 3]
```

## 数组(常用)属性

.length

- 对于 **连续** 的数组, arr.length 获取的是数组的长度 (元素的个数)

- 对于 **不连续** 的数组, arr.length 获取的是最大索引值 + 1

  ```js
  var arr = new Array()
  arr[0] = 1
  arr[1] = 2
  arr[2] = 3
  arr[3] = 4
  arr[10] = 11
  console.log(arr.length) // 11
  ```

- 设置数组的长度

  - 数组名.length = 10
  - 如果设置的长度大于原数组的长度, 多出的会空出来 (2, 3, 4, , , , )
  - 如果设置的长度小于原数组的长度, 多出的会被舍弃

## 数组的常用方法

见 10-JS基础之数组的常用方法

# 几个内置对象

## Date 对象

在 JS 中使用 Date 对象来表示一个时间

- 创建一个 Date 对象

  ```js
  var d = new Date()
  console.log(d) // 当前代码执行时的时间 'Sat Dec 03 2019 10:12:14 GMT+0800(中国标准时间)'
  
  // 创建指定的时间对象, 传递一个时间字符串
  var d2 = new Date('12/03/2019 11:10:02')
  ```

- 日期的格式

  - 月 / 日 / 年  时:分:秒

- 常用方法

  | API           | 说明                                                         |
  | ------------- | ------------------------------------------------------------ |
  | getDate()     | 当前日期对象的日                                             |
  | getDay()      | 当前日期对象的星期几 (**返回 0-6 的值, 0 表示周日**)         |
  | getMonth()    | 当前日期对象的月 (**返回 0-11 的值, 0 表示一月**)            |
  | getFullYear() | 当前日期对象的年                                             |
  | getHour()     | 当前日期对象的时                                             |
  | getTime()     | 指定日期对象的时间戳(从 1970 年 1 月 1 日 到当前日期的毫秒数) |
  | Date.now()    | 获取 `当前` 的时间戳 (毫秒)                                  |

  ```js
  var d = new Date()
  
  var date = d.getDate() // 3
  
  var time = Date.now()  // 1480649189517
  ```

## Math 对象

**`Math 和其他对象不同, 不是构造函数`**

- 它是一个工具类, 不需要创建对象, <font color=#c40>直接使用</font>
- 常用属性
  - Math.PI
- 常用方法

  | API                   | 说明                         |
  | --------------------- | ---------------------------- |
  | Math.abs(a)           | 绝对值                       |
  | Math.ceil(x)          | 向上取整(*小数位有值就进 1*) |
  | Math.floor(x)         | 向下取整                     |
  | Math.round(x)         | 四舍五入取整                 |
  | **Math.Random()**     | 返回 0 ~ 1 之间的随机数      |
  | Math.max(x, y,......) | 最大值                       |
  | Math.min(x, y,......) | 最小值                       |
  | Math.pow(x, y)        | x 的 y 次幂                  |
  | Math.sqrt(x)          | x 的 开方                    |

  ```js
  console.log(Math.ceil(1.2))  // 2
  
  console.log(Math.floor(1.9))  // 1
  
  console.log(Math.round(1.5))  // 2
  
  // 生成 x ~ y 之间的随机数
  Math.round(Math.Random() * (y - x)) + x
  ```

## String 对象

在底层, 字符串是以 **字符数组** 的形式保存的

常用属性和方法见 10-JS基础之字符串常用方法

# 包装类

JS 中为我们提供了三个包装类, 通过这些包装类可以 `将基本数据类型转为对象`

1. String()
   - 将一个基本数据类型的字符串转为 string 对象

2. Number()
   - 将一个基本数据类型的数值转为 number 对象

3. Boolean()
   - 将一个基本数据类型的布尔值转为 boolean 对象

```js
var num = new Number(3)
console.log(num) // 3
console.log(typeof num) // 'object'
```

# 正则表达式 RegExp 对象

[参考手册](<http://www.w3school.com.cn/jsref/jsref_obj_regexp.asp>)

<font color=#c00>用于定义一些字符串的规则</font>

- 计算机可以根据正则表达式, 来*检查一个字符串是否符合规则*

## 创建正则表达式对象

```js
// 1.构造函数创建
var reg = new RegExp('a', 'i')  // 检查一个字符串是否含有 a
// 第一个参数 正则表达式     第二个参数 匹配模式 i(忽略大小写) / g(全局匹配)
console.log(typeof reg) // 'object'
console.log(reg) // '/a/'
reg.test('ba') // true  

// 2.字面量方式创建
// 语法: var 变量名 = /正则表达式/匹配模式
var reg = /a/i
```

## 正则表达式语法

- |  ——  或者

  ```js
  var reg = /a|b/
  reg.test('bc') // true, 测试一个字符串是否有 a 或者 b
  ```

- 方括号

  - [ab]  ——  查找中括号内的字符

    ```js
    var reg = /a[bde]c/
    // 判断一个字符串是否含有 abc 或 adc 或 aec
    ```

  - [a-z]  ——  任意一个小写字母

  - [A-Z]  ——  任意一个大写字母

  - [A-z]  ——  任意一个字母

  - [^ab ] —— 查找中括号之外的自负

    ```js
    var reg = /[^ab]/ // 查找 ab 之外的字符
    reg.test('a') // false
    reg.test('b') // false
    reg.test('c') // true
    reg.test('abc') // true
    ```

  - [0-9] —— 任意一个数字

- 量词 (只对它前边的一个内容起作用)

  - {n}  —— 正好出现 n 次

    ```js
    var reg = /a{3}/ // aaa
    
    reg.test('aaac') // true
    
    var reg = /(ab){3}/ // ababab
    ```

  - {1, 3} —— 出现 1-3 次

    ```js
    var reg = /a{1, 3}/ // 1-3 个 a
    
    reg.test('aac') // true
    ```

  - {3,} —— 3次以上

  - a+—— 至少一个a

    ```js
    var reg = /ab+c/ // b 至少得有一个
    
    reg.test('abbbcs') // true
    ```

  - a* —— 0 或多个a

  - a? —— 0 或 1 个a

  - ^a —— 以 a 开头

    ```js
    var reg = /^a/
    
    reg.test('abs') // true
    ```

  - a$ —— 以 a 结尾

    ```js
    var reg = /^a|a$/ // 以 a 开头或以 a 结尾
    
    reg.test('ab') 
    ```

- 元字符

  - . —— 表示任意字符

    ```js
    var reg = /./
    reg.test('abc') // true
    ```

  - \w —— 表示任意字母 / 数字 / 下划线

    ```js
    var reg = /\w/
    reg.test('_') // true
    ```

  - \W —— 除了字母 / 数字 / 下划线

    ```js
    var reg = /\W/
    reg.test('@@@@') // true
    ```

  - \d —— 表示任意数字

  - \D —— 除了数字

  - \s —— 空格

    ```js
    var reg = /\s/
    reg.test('12 3') // true
    ```

  - \S —— 除了空格

  - \b —— 单词边界

    ```js
    var reg = /\bchild\b/
    reg.test('hello children') // false
    ```

- ⚠️**如果在正则表达式中同时使用 ^ 和 $ , 则要求字符串必须完全符合正则规则**

- 在正则表达式中使用 \ 作为转义字符

  ```js
  var reg = /\./
  reg.test('a.b') // true
  ```

### 正则表达式的方法

1. test()

   - **检查** 一个字符串是否符合正则规则

   - 返回 true / false

```js
reg.test(str)

reg.test('bbc')
```

2. <font color=#c40>字符串和正则相关的方法</font>
   - 见 10-JS 基础之字符串常用方法

## 常用正则表达式

```js
// 手机号
var phoneReg = /^1[3-9][0-9]{9}$/

// 电子邮件的正则 (只是参考,不是标准)
var mailReg = /^\w{3,}(\.\w+)*@[A-z0-9]+(\.[A-z]{2,5}){1,2}$/

// 去除字符串前面和后面的多个空格(保留中间的空格)
// var str = '   he  llo   '
str = str.replace(/^\s*|\s*$/g, '')
```

# 定时器

JS 的程序执行速度是非常快的

如果希望一段程序, 每隔一段时间执行一次, 可以使用`定时调用`

## setInterval

setInterval(function() {}, 1000)

- *可以将一个函数每隔一段时间执行一次*
- 第一个参数 —— 回调函数, 该函数每隔一段时间被调用一次
- 第二个参数 —— 时间间隔, 单位是 ms
- 返回一个数字, 作为定时器的唯一标识
  ```js
  var num = 1
  var timer = null
  
  clearInterval(timer)
  timer = setInterval(function () {
    p.innerHTML = num++
    if (num > 10) {
      clearInterval(timer)
    }
  }, 1000)
  ```

## clearInterval

clearInterval()

- 关闭一个定时器
- 需要 `一个定时器的标识` 作为参数
- 可以接收任意参数
  - 如果接收的是有效的定时器标识, 则停止对应的定时器
  - 如果是无效的值 (undefined / null), 不做任何操作
- <font color=#c00>在开启一个定时器之前应先关闭定时器, 避免多次点击造成执行速度变快的问题</font>

  ```js
  var btn01 = document.getElementById('btn01')
  var btn02 = document.getElementById('btn02')
  var img = document.getElementById('img')
  
  var imgs = ['01.jpg', '02.jpg', '03.jpg', '04.jpg']
  var index = 0
  var timer = null
  btn01.onclick = function () {
    // 先清除定时器
    clearInterval(timer)
    // 开启定时器
    timer = setInterval(function () {
      index++
        index %= imgs.length
        img.src = imgs[index]
    }, 1000)
  }
  
  btn02.onclick = function () {
    // 关闭定时器
    clearInterval(timer)
  }
  ```

## setTimeout 延时调用

不马上执行, 隔多久之后再执行, <font color=#c00>只会执行一次</font>

- setTimeout(function() {}, 1000)
  - 开启延时调用
- clearTimeout()
  - 关闭延时调用

# 垃圾回收(GC)

程序运行过程中也会产生垃圾, 所以我们需要一个 `垃圾回收的机制`, 来处理程序运行过程中的垃圾

- 当一个对象没有任何的变量或属性对它进行引用, 此时我们将永远无法操作该对象

  - 此时这种对象就是垃圾, 这种对象过多会占用大量的内存空间, 导致程序运行变慢

- JS 有 `自动` 的垃圾回收机制, 会自动将这些垃圾销毁

  - 我们要做的只是 **将不再使用的对象设置为 null 即可**
    ```js
    var obj = new Object()
    ......
    obj = null 
    ```

# 使用 Unicode 字符

在 JS 中使用

- \u2620  (\u 后跟 十六进制数)
  ```js
  console.log('\u0031') // 1
  ```

在 HTML 中使用

- &#编码; (&# 后跟 十进制数)
  ```html
  <h1>&#9760;</h1> ☠️
  ```

> \u 开头和 &#x 开头是一样的 都是16进制 unicode 字符的写法；&# 则是 unicode 字符的10进制的写法