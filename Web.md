# Web

## Web 开发介绍

**Web**：全球广域网，也称为**万维网**(www **W**orld **W**ide **W**eb)，能够通过浏览器访问的**网站**。

### 网站的工作流程

1. 首先通过**浏览器**访问发布到**前端服务器**中的**前端程序**，这时候前端程序会将前端代码返回给浏览器。如下图所示：

![浏览器请求前端资源](img/Web_1.png)

2. 浏览器得到前端代码，此时浏览器会将前端代码进行解析，然后展示到浏览器的窗口中，这时候就看到了**网站**的**页面**，如下图所示：

![浏览器解析展示前端资源](img/Web_2.png)

1. 但是此时这个页面是没有数据的，因为数据在数据库中，所以浏览器需要根据**前端代码中指定的后台服务器的地址** 向**后台服务器**（内部有 java 程序）发起**请求**，后台服务器再去从**数据库**中获取数据，然后返回给浏览器。

![浏览器请求后台服务器资源](img/Web_3.png)

1. 浏览器拿到后台返回的数据后，然后将数据展示在前端资源也就是**网页**上，然后就看到了网页的完整的内容

![浏览器展示完整资源](img/Web_4.png)

**整个流程如下：**

1. 浏览器先向前端服务器请求**前端资源**，也就是我们所说的**网页**

2. 浏览器再向**后台服务器**发起请求，获取**数据**

3. 浏览器将得到的后台**数据**填充到**网页**上，然后展示给用户去看

### 网站的开发模式

主要有 2 种：前后端分离开发模式和混合开发模式。

#### 前后端分离开发模式

（**目前企业开发的主流**市场占有率 70%以上）这种开发模式的特点如下：

1. 前端人员开发前端程序，前端程序单独部署到前端服务器上

2. 后端人员开发后端程序，后端程序部署到后端服务器上

![前后端分离开发模式](img/Web_5.png)

#### 混合开发模式

这种开发模式的特点是：前端人员开发的代码和后端人员开发的代码在同一个项目中，一起打包部署。

![混合开发模式](img/Web_6.png)

### 网站开发的技术栈

![网站开发的技术栈](img/Web_7.png)

前端 web 开发：

| 技术       | 描述                                            |
| ---------- | ----------------------------------------------- |
| HTML       | 用于构建网站的基础结构的                        |
| css        | 用于美化页面的，作用和化妆或者整容作用一样      |
| JavaScript | 实现网页和用户的交互                            |
| Vue        | 主要用于将数据填充到 html 页面上的              |
| Element    | 主要提供了一些非常美观的组件                    |
| Nginx      | 一款 web 服务器软件，可以用于部署我们的前端工程 |

后端 web 开发：

| 技术       | 描述                                            |
| ---------- | ----------------------------------------------- |
| Maven      | 一款 java 中用于管理项目的软件                  |
| Mysql      | 最常用的一款数据库软件之一                      |
| SpringBoot | spring 家族的产品，当前最为主流的项目开发技术。 |
| Mybatis    | 用于操作数据库的框架                            |

### Web 标准

**Web 标准**也称为**网页标准**，由一系列的标准组成，大部分由 W3C（ World Wide Web Consortium，万维网联盟）负责制定。由三个组成部分：

- HTML：负责网页的结构（页面元素和内容）。

- CSS：负责网页的表现（页面元素的外观、位置等页面样式，如：颜色、大小等）。

- JavaScript：负责网页的行为（交互效果）。

## HTML&CSS

### HTML

**HTML**:HyperText Markup Language，超文本标记语言。

- 超文本：超越了文本的限制，比普通文本更强大。除了文字信息，还可以定义图片、音频、视频等内容。

- 标记语言：由标签构成的语言
  - HTML 标签都是预定义好的。例如：使用`<h1>`标签展示标题，使用`<a>`展示超链接，使用`<img>`展示图片，`<video>`展示视频。
  - HTML 代码直接在浏览器中运行，HTML 标签由浏览器解析。

#### HTML 页面的基础结构标签

```
<html>
	<head>
    	<title> </title>
    </head>
    <body>

    </body>
</html>
```

`<title>`中定义标题显示在浏览器的标题位置，`<body>`中定义的内容会呈现在浏览器的内容区域

#### HTML 中的标签特点

- HTML 标签不区分大小写
- HTML 标签的属性值，采用单引号、双引号都可以
- HTML 语法相对比较松散（建议大家编写 HTML 标签的时候尽量严谨一些）

#### 图片标签 img

1. 图片标签:`<img>`

2. 常见属性:
   src: 指定图像的 url (可以指定 绝对路径 , 也可以指定 相对路径)
   width: 图像的宽度 (像素 / 百分比 , 相对于父元素的百分比)
   height: 图像的高度 (像素 / 百分比 , 相对于父元素的百分比)

   备注: 一般 width 和 height 我们只会指定一个，另外一个会自动的等比例缩放。

3. 路径书写方式:
   绝对路径（赋值给 src 属性）: 1. 绝对磁盘路径 2. 绝对网络路径
   相对路径:
   ./ : 当前目录，可省略
   ../ : 上一级目录

#### 标题标签 h 系列

1. 标题标签: `<h1>` - `<h6>`

```
	<h1>111111111111</h1>
	<h2>111111111111</h2>
	<h3>111111111111</h3>
	<h4>111111111111</h4>
	<h5>111111111111</h5>
	<h6>111111111111</h6>
```

2. 效果 : h1 为一级标题，字体也是最大的 ； h6 为六级标题，字体是最小的。

#### 水平分页线标签

水平分页线标签: `<hr>`

#### 超链接

- 标签: `<a>href="..." target="...">...</a>`
- 属性:
  - href: 指定资源访问的 url
  - target: 指定在何处打开资源链接
    - \_self: 默认值，在当前页面打开
    - \_blank: 在空白页面打开

#### 视频、音频标签

- 视频标签:`<video>`

  - 属性:
    - src: 规定视频的 url
    - controls: 显示播放控件
    - width: 播放器的宽度
    - height: 播放器的高度

- 音频标签:`<audio>`
  - 属性:
    - src: 规定音频的 url
    - controls: 显示播放控件

#### 段落标签

- 换行标签: `<br>`

  - 注意: 在 HTML 页面中,我们在编辑器中通过回车实现的换行, 仅仅在文本编辑器中会看到换行效果, 浏览器是不会解析的, HTML 中换行需要通过 br 标签

- 段落标签: `<p>`

#### 文本格式标签

| 效果   | 标签 | 标签(强调) |
| ------ | ---- | ---------- |
| 加粗   | b    | strong     |
| 倾斜   | i    | em         |
| 下划线 | u    | ins        |
| 删除线 | s    | del        |

#### 占位符

- 在 HTML 页面中无论输入了多少个空格, 最多只会显示一个。 可以使用空格占位符（&nbsp；）来生成空格，如果需要多个空格，就使用多次占位符。

- 那在 HTML 中，除了空格占位符以外，还有一些其他的占位符(了解, 只需要知道空格的占位符写法即可)，如下：

| 显示结果 | 描述   | 占位符  |
| :------- | :----- | :------ |
| &nbsp;   | 空格   | \&nbsp; |
| <        | 小于号 | \&lt;   |
| >        | 大于号 | \&gt;   |
| &        | 和号   | \&amp;  |
| "        | 引号   | \&quot; |
| '        | 撇号   | \&apos; |

#### 布局标签

- 布局标签：实际开发网页中，会大量频繁的使用 div 和 span 这两个没有语义的布局标签。

- 标签：`<div>` `<span>`

- 特点：

  - div 标签：

    - 一行只显示一个（独占一行）

    - 宽度默认是父元素的宽度，高度默认由内容撑开

    - 可以设置宽高（width、height）

  - span 标签：

    - 一行可以显示多个

    - 宽度和高度默认由内容撑开

    - 不可以设置宽高（width、height）

#### 表格标签

- `<table>` : 用于定义整个表格, 可以包裹多个 `<tr>`， 常用属性如下：

  - border：规定表格边框的宽度
  - width：规定表格的宽度
  - cellspacing: 规定单元之间的空间

- `<tr>` : 表格的行，可以包裹多个 `<td>`

- `<td>` : 表格单元格(普通)，可以包裹内容 , 如果是表头单元格，可以替换为 `<th>`

#### 表单标签

- 表单标签: `<form>`

- 表单属性:

  - action: 规定表单提交时，向何处发送表单数据，表单提交的 URL。
  - method: 规定用于发送表单数据的方式，常见为： GET、POST。
    - GET：表单数据是拼接在 url 后面的， 如： xxxxxxxxxxx?username=Tom&age=12，url 中能携带的表单数据大小是有限制的。
    - POST： 表单数据是在请求体（消息体）中携带的，大小没有限制。

- 表单项标签: 不同类型的 input 元素、下拉列表、文本域等。

  - input: 定义表单项，通过 type 属性控制输入形式

  - select: 定义下拉列表

  - textarea: 定义文本域

分别是：

- `<input>`: 表单项 , 通过 type 属性控制输入形式。

  | type 取值                | **描述**                             |
  | ------------------------ | ------------------------------------ |
  | text                     | 默认值，定义单行的输入字段           |
  | password                 | 定义密码字段                         |
  | radio                    | 定义单选按钮                         |
  | checkbox                 | 定义复选框                           |
  | file                     | 定义文件上传按钮                     |
  | date/time/datetime-local | 定义日期/时间/日期时间               |
  | number                   | 定义数字输入框                       |
  | email                    | 定义邮件输入框                       |
  | hidden                   | 定义隐藏域                           |
  | submit / reset / button  | 定义提交按钮 / 重置按钮 / 可点击按钮 |

- `<select>`: 定义下拉列表, `<option>` 定义列表项

- `<textarea>`: 文本域

#### HTTP 请求

##### 请求组成：

1. 请求行：请求方式、请求地址、协议版本

2. 请求头：头名:头值

3. 请求体（可选）：携带数据

##### 请求方式与数据格式

**get 请求示例**

```
GET /test2?name=张&age=20 HTTP/1.1
//请求行

Host: localhost
//请求头
```

**post 请求示例**

（url 编码）

```
POST /test2 HTTP/1.1
//请求行

Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 21
//请求头

name=张
//请求体
```

- application/x-www-form-urlencoded：参数用键值对存储，用 `=` 分隔；参数之间用 `&` 分割

**json 请求示例**

（utf-8 编码）

```
POST /test3 HTTP/1.1
//请求行

Host: localhost
Content-Type: application/json
Content-Length: 25
//请求头

{"name":"zhang","age":18}
//请求体
```

- application/json：参数以 JSON 格式存储，参数之间用 `,` 分割；参数的键值对用 `:` 分割

##### session 原理

Http 无状态，有会画：

- 无状态是指，请求之间相互独立，第一次请求的数据，第二次请求不能重用

- 有会话是指，客户端和服务端都有相应的技术，可以暂存数据，让数据在请求间共享

服务端使用了 session 技术来暂存数据

- 存

```
GET /s1?name=zhang HTTP/1.1
Host: localhost
```

- 取

```
GET /s2 HTTP/1.1
Host: localhost
Cookie: JSESSIONID=560FA845D02AE09B176E1BC5D9816A5D
```

### CSS

**CSS**:Cascading Style Sheet，层叠样式表，用于控制页面的样式（表现）。

#### CSS 引入方式

具体有 3 种引入方式，语法如下表格所示：

| 名称     | 语法描述                                          |
| -------- | ------------------------------------------------- |
| 行内样式 | 在标签内使用`style`属性，属性值是 CSS 属性键值对  |
| 内嵌样式 | 定义`<style>`标签，在标签内部定义 CSS 样式        |
| 外联样式 | 定义`<link>`标签，通过`href`属性引入外部 CSS 文件 |

企业开发的使用情况如下：

1. 内联样式会出现大量的代码冗余，不方便后期的维护，所以不常用。
2. 内部样式，通过定义 CSS 选择器，让样式作用于当前页面的指定的标签上。
3. 外部样式，HTML 和 CSS 实现了完全的分离，企业开发常用方式。

#### 颜色表示

| **表示方式**   | **表示含义**                      | **取值**                                         |
| -------------- | --------------------------------- | ------------------------------------------------ |
| 关键字         | 预定义的颜色名                    | red, green, blue...                              |
| rgb 表示法     | 红绿蓝三原色，每项取值范围：0-255 | rgb(0, 0, 0), rgb(255, 255, 255), rgb(255, 0, 0) |
| 十六进制表示法 | #开头，将数字转换成十六进制表示   | #000000, #ff0000, #cccccc，简写：#000, #ccc      |

#### CSS 选择器

选择器是选取需设置样式的元素（标签），根据业务场景不同，选择的标签的需求也是多种多样的，有多种选择器

1. **元素（标签）选择器**

- 选择器的名字必须是标签的名字
- 作用：选择器中的样式会作用于所有同名的标签上

```
元素名称 {
    css样式名:css样式值；
}
```

2. **id 选择器**

- 选择器的名字前面需要加上#
- 作用：选择器中的样式会作用于指定 id 的标签上，而且有且只有一个标签（由于 id 是唯一的）

```
#id属性值 {
    css样式名:css样式值；
}
```

3. **类选择器**

- 选择器的名字前面需要加上 .
- 作用：选择器中的样式会作用于所有 class 的属性值和该名字一样的标签上，可以是多个

```
.class属性值 {
    css样式名:css样式值；
}
```

#### CSS 属性

- text-indent: 设置段落的首行缩进
- line-height: 设置行高
- text-align: 设置对齐方式, 可取值为 left / center / right

#### 盒子模型

- 盒子：页面中所有的元素（标签），都可以看做是一个 盒子，由盒子将页面中的元素包含在一个矩形区域内，通过盒子的视角更方便的进行页面布局

- 盒子模型组成：内容区域（content）、内边距区域（padding）、边框区域（border）、外边距区域（margin）

![盒子模型](img/Web_8.png)

## JavaScript

JavaScript 是一门脚本语言，用于实现网页的动态效果，让页面和用户进行交互。

### 引入方式

**第一种方式**：内部脚本，将 JS 代码定义在 HTML 页面中

- JavaScript 代码必须位于`<script> </script>`标签之间
- 在 HTML 文档中，可以在任意地方，放置任意数量的`<script>`
- 一般会把脚本置于`<body>`元素的底部，可改善显示速度

**第二种方式**：外部脚本，将 JS 代码定义在外部 JS 文件中，然后引入到 HTML 页面中

- 外部 JS 文件以`.js`为后缀名
- 外部 JS 文件只包含 JS 代码，不包含`<script>`标签
- 引入外部 JS 的`<script>`必须是双标签，且放在`<head>`标签中

### 基本语法

#### 语法规则：

- 区分大小写：与 Java 一样，变量名、函数名以及其他一切东西都是区分大小写的

- 每行结尾的分号可有可无

- 大括号表示代码块

- 注释：

  - 单行注释：// 注释内容

  - 多行注释：/_ 注释内容 _/

#### JS 的三种输出语句

| API              | 描述             |
| ---------------- | ---------------- |
| window.alert()   | 警告框           |
| document.write() | 在 HTML 输出内容 |
| console.log()    | 写入浏览器控制台 |

#### 变量

| 关键字 | 解释                                                                       |
| ------ | -------------------------------------------------------------------------- |
| var    | 早期 ECMAScript5 中用于变量声明的关键字                                    |
| let    | ECMAScript6 中新增的用于变量声明的关键字，相比较 var，let 只在代码块内生效 |
| const  | 声明常量的，常量一旦声明，不能修改                                         |

- JavaScript 是一门弱类型语言，变量可以存放不同类型的值 。
- 变量名需要遵循如下规则：
  - 组成字符可以是任何字母、数字、下划线（\_）或美元符号（$）
  - 数字不能开头
  - 建议使用驼峰命名

#### 数据类型和运算符

虽然 JS 是弱数据类型的语言，但是 JS 中也存在数据类型，JS 中的数据类型分为 ：原始类型 和 引用类型，具体有如下类型

| 数据类型  | 描述                                               |
| --------- | -------------------------------------------------- |
| number    | 数字（整数、小数、NaN(Not a Number)）              |
| string    | 字符串，单双引皆可                                 |
| boolean   | 布尔。true，false                                  |
| null      | 对象为空                                           |
| undefined | 当声明的变量未初始化时，该变量的默认值是 undefined |

使用 typeof 函数可以返回变量的数据类型

| 运算规则   | 运算符                                                                     |
| ---------- | -------------------------------------------------------------------------- |
| 算术运算符 | + , - , \* , / , % , ++ , --                                               |
| 赋值运算符 | = , += , -= , \*= , /= , %=                                                |
| 比较运算符 | >, < , >= , <= , != , == , === 注意`==`会进行类型转换`===`不会进行类型转换 |
| 逻辑运算符 | && , \|\| , !                                                              |
| 三元运算符 | 条件表达式 ? true_value: false_value                                       |

#### 函数

- 第一种定义格式：

```
function 函数名(参数1,参数2..){
    要执行的代码
}
```

因为 JavaScript 是弱数据类型的语言，所以有如下几点需要注意：

- 形式参数不需要声明类型，并且 JavaScript 中不管什么类型都是 let 或者 var 去声明，加上也没有意义。

- 返回值也不需要声明类型，直接 return 即可

- 第二种定义格式：

```
var functionName = function (参数1,参数2..){
	//要执行的代码
}
```

- 在 JavaScript 中，函数的调用只需要名称正确即可，参数列表不管的。如上述案例，10 传递给了变量 a，20 传递给了变量 b,而 30 和 40 没有变量接受，但是不影响函数的正常调用。

### JavScript 对象

#### 基本对象

- Array
- Boolean
- Classe
- Date
- Error
- Global
- JSON
- Math
- Number
- RegExp
- String
- 运算符
- 语句

##### Array

Array 对象是用来定义数组

**构造方法**

方法一：`var 变量名 = new Array(元素1,元素2,元素3..);`

方法二：`var 变量名 = [元素1,元素2,元素3..];`

JavaScript 中数组相当于 Java 中的集合，数组的长度是可以变化的。而且 JavaScript 是弱数据类型的语言，所以数组中可以存储任意数据类型的值。接下来我们通过代码来演示上述特点。

**属性和方法**

属性：

| 属性   | 描述                         |
| :----- | :--------------------------- |
| length | 设置或返回数组中元素的数量。 |

方法：

| 方法方法  | 描述                                                                       |
| :-------- | :------------------------------------------------------------------------- |
| forEach() | 遍历数组中的每个有值的元素，并调用一次传入的函数                           |
| push()    | 将新元素添加到数组的末尾，并返回新的长度                                   |
| splice()  | 从数组中删除元素，参数 1 表示从哪个索引位置删除，参数 2 表示删除的元素个数 |

使用方法都是使用`.`来后接方法名，并传入相应的参数。

```
arr.forEach(function(e){
  console.log(e);
})

arr.forEach((e)=>console.log(e))
```

##### String

String 对象是用来定义字符串

**构造方法**

方法一：`var 变量名 = new String(字符串);`

方法二：`var 变量名 = "字符串";`

**属性和方法**

属性：

| 属性   | 描述           |
| ------ | -------------- |
| length | 字符串的长度。 |

方法：

| 方法        | 描述                                     |
| ----------- | ---------------------------------------- |
| charAt()    | 返回在指定位置的字符。                   |
| indexOf()   | 检索字符串。                             |
| trim()      | 去除字符串两边的空格                     |
| substring() | 提取字符串中两个指定的索引号之间的字符。 |

##### JSON

###### 自定义对象

**构造方法**

```
var 对象名 = {
    属性名1: 属性值1,
    属性名2: 属性值2,
    属性名3: 属性值3,
    函数名称: function(形参列表){}
};
```

通过`对象名.属性名`调用属性或者`对象名.函数名称(参数)`调用函数。

###### JSON 对象

JSON 对象：**J**ava**S**cript **O**bject **N**otation，JavaScript 对象标记法。是通过 JavaScript 标记法书写的文本。

**格式**：

```
{
  "key":value,
  "key":value,
  "key":value
}
```

**key 必须使用引号并且是双引号标记，value 可以是任意数据类型。**

JSON 这种数据格式的对象经常用来作为前后台交互的数据载体。

在前后台交互的时候，我们需要传输数据，可以使用下图的 XML 格式，可以清晰地描述 Java 中需要传递给前端的 Java 对象。

![XML格式传输数据](img/Web_9.png)

但是 XML 格式存在如下问题：

- 冗余：标签需要编写双份，占用带宽、浪费资源

- 繁琐：解析繁琐

JSON 格式的对象可以解决这些问题

![JSON格式传输数据](img/Web_10.png)

但是上述在浏览器中还不能直接使用，**因为上述是一个 JSON 字符串，不是 JSON 对象**

使用`JSON.parse()`方法，传入一个 JSON 字符串，可以将其转换为 JSON 对象。

使用`JSON.stringify()`方法，传入一个 JSON 对象，可以将其转换为 JSON 字符串。

#### BOM 对象

BOM 的全称是 Browser Object Model,翻译过来是浏览器对象模型

也就是 JavaScript 将浏览器的各个组成部分封装成了对象。操作浏览器的部分功能，可以通过操作 BOM 对象的相关属性或者函数来完成。

| 对象名称  | 描述           |
| :-------- | :------------- |
| Window    | 浏览器窗口对象 |
| Navigator | 浏览器对象     |
| Screen    | 屏幕对象       |
| History   | 历史记录对象   |
| Location  | 地址栏对象     |

BOM 对象与浏览器各组成对应关系：

![BOM对象与浏览器各组成对应关系](img/Web_11.png)

##### Window 对象

Window 对象指的是浏览器窗口对象，是 JavaScript 的全部对象，对于 Window 对象，我们可以直接使用，并且对于其属性和方法，我们可以省略 Window 对象。

window 对象提供了获取其他 BOM 对象的属性：

| 属性      | 描述                    |
| --------- | ----------------------- |
| history   | 用于获取 history 对象   |
| location  | 用于获取 location 对象  |
| navigator | 用于获取 navigator 对象 |
| screen    | 用于获取 screen 对象    |

Window 也提供了一些常用的函数，如下表格所示：

| 函数          | 描述                                               |
| ------------- | -------------------------------------------------- |
| alert()       | 显示带有一段消息和一个确认按钮的警告框。           |
| comfirm()     | 显示带有一段消息以及确认按钮和取消按钮的对话框。   |
| setInterval() | 按照指定的周期（以毫秒计）来调用函数或计算表达式。 |
| setTimeout()  | 在指定的毫秒数后调用函数或计算表达式。             |

- alert()函数：弹出警告框，函数的内容就是警告框的内容

- confirm()函数：弹出确认框，并且提供用户 2 个按钮，分别是确认和取消。该函数有个布尔类型的返回值

- setInterval(function,毫秒值)：定时器，用于周期性的执行某个功能，并且是**循环执行**。该函数需要传递 2 个参数：第一个参数是要执行的函数，第二个参数是执行的周期，单位是毫秒。

- setTimeout(function,毫秒值) ：定时器，只会在一段时间后**执行一次功能**。参数同`setInterval()`

##### Location 对象

location 是指浏览器的地址栏对象，常用属性为`href`属性，用于获取或者设置浏览器的地址信息

#### DOM 对象

DOM：Document Object Model 文档对象模型。也就是 JavaScript 将 HTML 文档的各个组成部分封装为对象。

封装的对象分为：

- Document：整个文档对象
- Element：元素对象
- Attribute：属性对象
- Text：文本对象
- Comment：注释对象

![JavaScript转换HTML文档](img/Web_12.png)

DOM 技术的特点：

- 改变 HTML 元素的内容
- 改变 HTML 元素的样式（CSS）
- 对 HTML DOM 事件作出反应
- 添加和删除 HTML 元素

DOM 技术核心：

- 如何获取 DOM 中的元素对象（Element 对象 ，也就是标签）
- 如何操作 Element 对象的属性,也就是标签的属性。

##### 获取 DOM 对象

HTML 中的 Element 对象可以通过 Document 对象获取，而 Document 对象是通过 Window 对象获取的。Document 对象提供的用于获取 Element 元素对象的 API 如下表所示：

| 函数                              | 描述                                         |
| --------------------------------- | -------------------------------------------- |
| document.getElementById()         | 根据 id 属性值获取，返回单个 Element 对象    |
| document.getElementsByTagName()   | 根据标签名称获取，返回 Element 对象数组      |
| document.getElementsByName()      | 根据 name 属性值获取，返回 Element 对象数组  |
| document.getElementsByClassName() | 根据 class 属性值获取，返回 Element 对象数组 |

- document.getElementById()：根据标签的 id 属性获取标签对象，id 是唯一的，所以获取到是单个标签对象。

- document.getElementsByTagName() : 根据标签的名字获取标签对象，同名的标签有很多，所以返回值是数组。

- document.getElementsByName()：根据标签的 name 的属性值获取标签对象，name 属性值可以重复，所以返回值是一个数组。

- document.getElementsByClassName(): 根据标签的 class 属性值获取标签对象，class 属性值也可以重复，返回值是数组。

##### 操作 DOM 对象

`innerHTML`属性：设置或获取元素的内容，可以设置 HTML 代码。

`style`属性：设置或获取元素的样式，可以设置 CSS 样式。

[JavaScript 参考手册](https://www.w3school.com.cn/jsref/index.asp)

### JavaScript 事件

HTML 事件是发生在 HTML 元素上的 “事情”，例如：

- 按钮被点击
- 鼠标移到元素上
- 输入框失去焦点

可以给这些事件绑定函数，当事件触发时，可以自动的完成对应的功能。这就是事件监听。

#### 事件绑定

JavaScript 对于事件的绑定提供了 2 种方式：

- 方式 1：通过 html 标签中的事件属性进行绑定，然后指向一个函数

- 方式 2：通过 DOM 中 Element 元素的事件属性进行绑定，然后操作对象属性绑定函数事件

### 常见事件

上面案例中使用到了 `onclick` 事件属性，那都有哪些事件属性供我们使用呢？下面就给大家列举一些比较常用的事件属性

| 事件属性名  | 说明                     |
| ----------- | ------------------------ |
| onclick     | 鼠标单击事件             |
| onblur      | 元素失去焦点             |
| onfocus     | 元素获得焦点             |
| onload      | 某个页面或图像被完成加载 |
| onsubmit    | 当表单提交时触发该事件   |
| onmouseover | 鼠标被移到某元素之上     |
| onmouseout  | 鼠标从某元素移开         |

## TypeScript

动态类型意味着

- 运行代码时才知道发生什么 (running the code to see what happens)

静态类型意味着

- 在代码运行前，就对它的行为做出预测 (make predications about what code is expected before it runs)

typescript 属于编译时实施类型检查（静态类型）的技术

### 类型

| 类型         | 例                                    | 备注                         |
| ------------ | ------------------------------------- | ---------------------------- |
| 字符串类型   | string                                |                              |
| 数字类型     | number                                |                              |
| 布尔类型     | boolean                               |                              |
| 数组类型     | number[],string[], boolean[] 依此类推 |                              |
| 任意类型     | any                                   | 相当于又回到了没有类型的时代 |
| 复杂类型     | type 与 interface                     |                              |
| 函数类型     | () => void                            | 对函数的参数和返回值进行说明 |
| 字面量类型   | "a"\|"b"\|"c"                         | 限制变量或参数的取值         |
| nullish 类型 | null 与 undefined                     |                              |
| 泛型         | `<T>`，`<T extends 父类型>`           |                              |

### 标注位置

##### 标注变量

```
let message: string = 'hello,world'
```

- 一般可以省略，因为可以根据后面的字面量推断出前面变量类型

##### 标注参数

```
function greet(name: string): void {
  //...
}
```

##### 标注返回值

```
function add(a: number, b: number) : number {
    return a + b
}
```

- 一般也可以省略，因为可以根据返回值做类型推断

##### 复杂类型

**type**

```
type Cat = {
  name: string,
  age: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 错误: 缺少 age 属性
const c3: Cat = { name: '小黑', age: 1, sex: '公' } // 错误: 多出 sex 属性
```

**interface**

```
interface Cat {
  name: string,
  age: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 错误: 缺少 age 属性
const c3: Cat = { name: '小黑', age: 1, sex: '公' } // 错误: 多出 sex 属性
```

**可选属性**

```
interface Cat {
  name: string,
  age?: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 正确: age 属性可选
```

可选属性需要注意处理 `undefined` 值，否则会报错。

##### 方法类型

```
interface Api {
  foo(): void,
  bar(str: string): string
}

function test(api: Api) {
  api.foo()
  console.log(api.bar('hello'))
}

test({
  foo() { console.log('ok') },
  bar(str: string) { return str.toUpperCase() }
})
```

##### 字面量类型

```
function printText(s: string, alignment: "left" | "right" | "center") {
  console.log(s, alignment)
}

printText('hello', 'left')
printText('hello', 'aaa') // 错误: 取值只能是 left | right | center
```

##### nullish 类型

```
function test(x?: string | null) {
  console.log(x?.toUpperCase())
}

test('aaa')
test(null)
test()
```

- x?: string | null 表示可能是 undefined 或者是 string 或者是 null

##### 泛型

```
interface Ref<T> {
  value: T
}

const r1: Ref<string> = { value: 'hello' }
const r2: Ref<number> = { value: 123 }
const r3: Ref<boolean> = { value: true }
```

- 泛型的要点就是 `<类型参数>`，把【类型】也当作一个变化的要素，像参数一样传递过来，这样就可以派生出结构相似的新类型

函数定义也支持泛型

```
function ref<T>(n: T): Ref<T> {
  return { value: n }
}

const v1 = ref("hello"); 	// Ref<string>
const v2 = ref(123.3333);	// Ref<number>

v1.value.toLocaleLowerCase()
v2.value.toFixed(2)
```

## Vue

Vue.js 是一套构建用户界面的 **渐进式框架**。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合。Vue.js 的目标是通过尽可能简单的 API 实现**响应的数据绑定**和**组合的视图组件**。

框架即是一个半成品软件，是一套可重用的、通用的、软件基础代码模型。基于框架进行开发，更加快捷、更加高效。

为了提高开发的效率，引入**MVVM(Model-View-ViewModel)的前端开发思想**，即让开发者更加关注数据，而非数据绑定到视图这种机械化的操作。

MVVM:其实是 Model-View-ViewModel 的缩写，有 3 个单词，具体释义如下：

- Model: 数据模型，特指前端中通过请求从后台获取的数据

- View: 视图，用于展示数据的页面，可以理解成我们的 html+css 搭建的页面，但是没有数据

- ViewModel: 数据绑定到视图，负责将数据（Model）通过 JavaScript 的 DOM 技术，将数据展示到视图（View）上

![MVVM架构](img/Web_13.png)

基于 MVVM 思想，其中的 Model 我们可以通过 Ajax 来发起请求从后台获取

对于 View 部分，我们将来会学习一款 ElementUI 框架来替代 HTML+CSS 来更加方便的搭建 View

而侧重于 ViewModel 部分开发的 vue 前端框架，用来替代 JavaScript 的 DOM 操作，让数据展示到视图的代码开发变得更加的简单。

### 基础介绍

核心功能：

- **声明式渲染**：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系

- **响应性**：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM

使用类 HTML 格式的文件来书写 Vue 组件，也称为**单文件组件**（`.vue`文件）顾名思义，Vue 的单文件组件会将一个组件的逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里。

### 环境准备

使用 `vite` 作为前端项目的打包、构建工具

```
npm init vite@latest

cd <my-project>
npm install
npm run dev
```

#### 修改端口

```
// vite.config.ts

export default defineConfig({
  //...

  server: {
    port: 3000,
    host: "0.0.0.0",
  }
})
```

#### 配置代理

为了避免前后端服务器联调时，相关请求产生跨域问题，需要配置代理

```
// vite.config.ts

export default defineConfig({
  //...

  server: {
    //...

    proxy: {
      '/api':{
        target: 'http://localhost:4000',
        changeOrigin: true,
      }
    }
  }
})
```

#### 项目结构

```
index.html
package.json
tsconfig.json
vite.config.ts
├─public
└─src
    ├─assets
    ├─components
    ├─model
    ├─router
    ├─store
    └─views
```

- index.html 为主页面
- package.json npm 配置文件
- tsconfig.json typescript 配置文件
- vite.config.ts vite 配置文件
- public 静态资源
- src/components 可重用组件
- src/model 模型定义
- src/router 路由
- src/store 共享存储
- src/views 视图组件

### 创建一个 Vue 实例

每个 Vue 应用都是通过`createApp`函数创建一个新的应用实例

```
import { createApp } from 'vue'
const app = createApp({
  /* 根组件选项 */
})
```

**根组件**`src/App.vue`：传入的`createApp`对象实际上是一个组件，每个应用都需要一个“根组件”，其他组件将作为其子组件嵌套在根组件下。

#### Vue 组件

Vue 组件文件是以`.vue`结尾，每个组件由三个部分组成

```
<script setup lang="ts"></script>

<template></template>

<style scoped></style>
```

- script 代码部分，控制模板的数据来源和行为

- template 模板部分，由它生成 html 代码

#### main.ts

```
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

- createApp 就是创建一个 Vue 应用实例，它接受的参数 App 即根组件

- mount 就是把根组件生成的 html 代码片段挂载到 index.html 中的 id 为 `app` 的元素上

#### ref 和 reactive

- ref 能将任意类型的数据变为【响应式】的

- reactive 只能将对象类型变为【响应式】，对基本类型无效（例如 string，number，boolean）

```
<script setup lang="ts">
import { ref, reactive } from 'vue'
const u1 = ref({ name: '张三' })
const u2 = reactive({ name: '张三' })

function test() {
  console.log(u1.value)
  console.log(u2)
}

test()
</script>

<template>
  <h2>{{u1.name}}</h2>
  <h2>{{u2.name}}</h2>
</template>
```

- 在 template 模板中使用 ref 包装的数据，直接写【变量名】就可以了

- 但在代码中要使用 ref 包装的数据，必须用【变量名.value】才能访问到

- reactive 包装的数据，在模板中和代码中都是一致的

#### 属性绑定

```
<script setup lang="ts">
import { ref } from 'vue'
const path = ref('/src/assets/vue.svg')

</script>

<template>
  <img :src="path" alt="">
</template>
```

- 【:属性名】用来将标签属性与【响应式】变量绑定（`v-bind`简写）

#### 事件绑定

```
<script setup lang="ts">
import { ref } from 'vue'
const count = ref(0)
function dec() {
  count.value--
}
function inc() {
  count.value++
}
</script>

<template>
  <input type="button" value="-" @click="dec">
  <h2>{{count}}</h2>
  <input type="button" value="+" @click="inc">
</template>
```

- 【@事件名】用来将标签属性与函数绑定，事件发生后执行函数内代码（`v-on`简写）

#### 表单绑定

- 用 v-model 实现双向绑定，即

  - javascript 数据可以同步到表单标签

  - 反过来用户在表单标签输入的新值也会同步到 javascript 这边

- 双向绑定只适用于表单这种带【输入】功能的标签，其它标签的数据绑定，单向就足够了

- 复选框这种标签，双向绑定的 javascript 数据类型一般用数组

#### 计算属性

```
computed(()=>{

})
```

- 计算属性具备缓存功能，即传入参数的值发生了变化，才会重新计算

- 如果用函数实现相同功能，则没有缓存功能，不会再当参数改变时重新计算

#### xhr 请求

```
const xhr = new XMLHttpRequest() // 创建 XMLHttpRequest 对象

// 当响应返回时，会触发 onload 事件
xhr.onload = function() {
  console.log(xhr.response) // 响应数据
}

xhr.open('Get', url) // 打开请求
xhr.responseType = 'json' // 设置响应类型
xhr.send() // 发送请求
```

### 模板语法

Vue 使用一种基于 HTML 的模板语法，能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。

在底层机制中，Vue 会将模板编译成高度优化的 JavaScript 代码。结合响应式系统，当应用状态变更时，Vue 能够智能地推导出需要重新渲染的组件的最少数量，并应用最少的 DOM 操作。

#### 文本插值

最基础的数据绑定形式是文本插值，使用的是 `{{ }}` 语法，该语法会被替换为相应组件实例中 `msg` 属性的值，每次 `msg` 属性更改时也会同步更新

```
<span>Message: {{ message }}</span>
```

#### Attribute 绑定

`{{}}` 不能在 HTML attribute 中使用，想要响应式地绑定一个 attribute ，应该使用 `v-bind` 指令：

```
<div v-bind:id="dynamicId"></div>
```

`v-bind` 指令指示 Vue 将元素的 `id` attribute 与组件的 `dynamicId` 属性保持一致，如果 `dynamicId` 发生变化，则会同步更新 `id` attribute；如果绑定的值是 `null` 或者 `undefined` 那么该 attribute 会被移除。

##### 简写

将常用的 `v-bind` 指令缩写为 `: `：

```
<div :id="dynamicId"></div>
```

##### 同名简写

如果 attribute 的名称与绑定的 JavaScript 值的名称相同，那么可以进一步简化语法，省略 attribute 值：

```
<!-- 与 :id="id" 等效 -->
<div :id="id"></div>
```

##### 布尔型 Attribute

**布尔型 attribute** 依据 true/false 值来决定 attribute 是否应该存在于该元素上。 `disabled` 就是最常见的例子

`v-bind` 在这种场景下有所不同：

```
<button :disabled="isButtonDisabled">Button</button>
```

1. 如果 `isButtonDisabled` 为 `true` 或者一个空字符串，则 `<button>` 元素将会获得 `disabled` attribute。

2. 如果 `isButtonDisabled` 为 `false`，则 `<button>` 元素将会失去 `disabled` attribute。

##### 动态绑定多个值

```
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper',
  style: 'background-color:green'
}

<div v-bind="objectOfAttrs"></div>
```

#### JavaScript 表达式

在 Vue 模板内，JavaScript 表达式可以被使用在如下场景上：

- 在文本插值中 (双大括号)

- 在任何 Vue 指令 (以 v- 开头的特殊 attribute) attribute 的值中

仅支持单一表达式，判断标准是否可以合法地写在 `return` 后面

#### 指令 Directives

指令是指带有 `v-` 前缀的特殊 attribute

Vue 提供了很多内置指令， 指令 attribute 的期望值为一个 JavaScript 表达式（除了 `v-for` 、 `v-on` 、 `v-slot`这几个指令之外），一个指令的任务是在其表达式的值变化时响应式地更新 DOM。

![完整指令语法](img/Web_14.png)

##### 参数 Arguments

某些指令需要一个参数，在指令后通过一个冒号 `:` 来指定参数

```
<a :href="url">...</a>
```

此处的 `href` 就是一个参数，它告诉 `v-bind` 指令将表达式 `url` 的值绑定到元素的 `href` attribute 上。

###### 动态参数

在指令参数上也可以使用一个 JavaScript 表达式，需要包含在 `[ ]` 内：

```
<a :[key]="url">...</a>
```

此处的 `key` 会作为一个 JavaScript 表达式被动态执行，计算得到的值会被用作最汇总的参数。

动态参数中表达式的值应当是一个字符串或者 `null` ，特殊值 `null` 意为显式移除该绑定。其他非字符串的值会触发警告。

##### 修饰符 Modifiers

修饰符是以点开头的特殊后缀，表明指令需要以一些特定的方式被绑定，后续 `v-on` 和 `v-model` 会详细说明

### 响应式基础

#### 声明响应式状态

##### `ref()`

```
import { ref } from 'vue'

const count = ref(0)
```

`ref()` 接收参数，并将其包裹在一个带有 `.value` 属性的 ref 对象中返回

```
const count = ref(0)

console.log(count.value) // {{ value: 0 }}
console.log(count) // 0

count.value++

console.log(count.value) // 1
```

##### `<script setup>`

在 `setup()` 函数中手动暴露大量的状态和方法十分繁琐，可以通过单文件组件来避免，使用 `<script setup>` 来大幅度简化代码

```
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">{{ count }}</button>
</template>
```

`<script setup>` 中的顶层的导入、声明的变量和函数刻在同一组件的模板中直接使用

##### 响应式原理

当在模板中使用了一个 `ref`，然后改变了这个 `ref` 的值时，Vue 会自动检测到这个变化，并且相应地更新 DOM。这是通过一个基于依赖追踪的响应式系统实现的。当一个组件首次渲染时，Vue 会追踪在渲染过程中使用的每一个 `ref`。然后，当一个 `ref` 被修改时，它会触发追踪它的组件的一次重新渲染。

另一个 `ref` 的好处是，与普通变量不同，你可以将 `ref` 传递给函数，同时保留对最新值和响应式连接的访问。当将复杂的逻辑重构为可重用的代码时，这将非常有用。

##### 深层响应性

Ref 可以持有任何类型的值，包括深层嵌套的对象、数组或者 JavaScript 内置的数据结构，比如 `Map`。这意味着即使改变嵌套对象或数组时，变化也会被检测到.

非原始值将通过 `reactive()` 转换为响应式代理

也可以通过 `shallow ref` 来放弃深层响应性。对于浅层 `ref`，只有 `.value` 的访问会被追踪。浅层 `ref` 可以用于避免对大型数据的响应性开销来优化性能、或者有外部库管理其内部状态的情况。

##### DOM 更新时机

修改了响应式状态时，DOM 会被自动更新。但是需要注意的是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。

要等待 DOM 更新完成后再执行额外的代码，可以使用 `nextTick()` 全局 API：

```
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // 现在 DOM 已经更新了
}
```

### 计算属性

使用**计算属性**来描述依赖响应式状态的复杂逻辑

```
<script setup>
import { ref, computed } from 'vue'

const authorName = ref('John Doe')
const books = ref([
  'Vue 2 - Advanced Guide',
  'Vue 3 - Basic Guide',
  'Vue 4 - The Mystery'
])

const publishedBooksMessage = computed(() => {
  return books.value.length > 0 ? "yes" : "no"
})
</script>

<template>
  <p>Has published books: {{ publishedBooksMessage }}</p>
</template>
```

计算属性的返回值是一个 `computed ref`，它会根据依赖的响应式状态自动更新。

Vue 的计算属性会自动追踪响应式依赖。它会检测到 `publishedBooksMessage` 依赖于 `books`，所以当 `books` 改变时，任何依赖于 `publishedBooksMessage` 的绑定都会同时更新。

若使用

```
function caculateBooksMessage() {
  return books.value.length > 0 ? "yes" : "no"
}
```

定义为一个方法而不是计算属性，则不会自动追踪依赖，需要手动调用 `caculateBooksMessage()` 来更新。

### Class 与 Style 绑定

数据绑定的一个常见需求场景是操纵元素的 CSS class 列表和内联样式。因为 class 和 style 都是 attribute，可以和其他 attribute 一样使用 v-bind 将它们和动态的字符串绑定。

但是，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。

因此，Vue 专门为 class 和 style 的 v-bind 用法提供了特殊的功能增强。除了字符串外，表达式的值也可以是对象或数组。

#### 绑定 Class

使用 `:class` （ `v-bind:class` ）来传递一个对象来动态切换 class

#### 绑定 Style

使用 `:style` 来绑定 JavaScript 表达式对象，动态更新元素的样式，对应的是 `style` attribute。

### 条件渲染

#### `v-if`

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回真值时才被渲染。

#### `v-else`

`v-else` 指令表示 `v-if` 的“else 块”。

#### `v-else-if`

`v-else-if` 指令表示 `v-if` 的一个分支，可以连续多次重复使用，一个使用 `v-else-if` 的元素必须紧跟在一个 `v-if` 或一个 `v-else-if` 元素后面。

#### `v-show`

`v-show` 指令根据表达式的真假值，切换元素的 `display` CSS 属性。不同于 `v-if`，`v-show` 不渲染元素，只是简单地切换元素的 `display` 值。

### 列表渲染

#### `v-for`

我们可以使用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令的值需要使用 `item in items` 、 `item of items` 形式的特殊语法，其中 items 是源数据的数组，而 item 是迭代项的别名：

`v-for` 也支持使用可选的第二个参数表示当前项的位置索引

#### `v-for` 与 对象

可以使用 `v-for` 来遍历一个对象的所有属性。遍历的顺序会基于对该对象调用 `Object.values()` 的返回值来决定。

提供第二个参数表示属性名（`key`），第三个参数表示位置索引（`index`）

#### `v-for` 与 范围值

可以使用 `v-for` 来渲染一个数字范围，会基于 `1 ... n`的取值范围来渲染。

```
<span v-for="n in 10">{{ n }}</span>
```

**初值为 1**

### 事件处理

#### 监听事件

`v-on` 指令用来监听 DOM 事件（简写为 `@` ），并在触发时调用相应的 JavaScript 回调函数。

事件处理器的值可以是：

1. **内联事件处理器**：事件被触发时执行的内联 JavaScript 代码

2. **方法事件处理器**：一个指向组件上定义的方法的属性名或是路径

#### 内联事件处理器

常用于简单场景

```
const count = ref(0)

<button @click="count++">{{ count }}</button>
```

在内联事件处理器中访问时间参数：

1. `$event`：原生 DOM 事件对象

```
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

2. 使用内联箭头函数

```
<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```

#### 方法事件处理器

`v-on` 可以接受一个方法名或对某个方法的调用

```
const name = ref('Vue.js')

function greet(event) {
  alert(`Hello ${name.value}!`)
  // `event` 是 DOM 原生事件
  if (event) {
    alert(event.target.tagName)
  }
}

<button @click="greet">Greet</button>
```

方法事件处理器会自动接收原生 DOM 事件并触发执行。在上面的例子中，我们能够通过被触发事件的 `event.target` 访问到该 DOM 元素。

#### 事件修饰符

在处理事件常常要调用 `event.preventDefault()` 或 `event.stopPropagation()` ，为了让方法更专注于数据逻辑而不用去处理 DOM 细节，Vue 为 `v-on` 提供了事件修饰符。

修饰符是以 `.` 表示的指令后缀，包含

- `.stop`
- `.prevent`
- `.self`
- `.capture`
- `.once`
- `.passive`

```
<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>

<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>

<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>
```

#### 按键修饰符

可以使用 `v-on` 监听按键事件时添加按键修饰符

```
<!-- 仅在 `key` 为 `Enter` 时调用 `submit` -->
<input @keyup.enter="submit" />
```

**按键别名**：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

**系统按键修饰符**：

- `.ctrl`
- `.alt`
- `.shift`
- `.meta` (Mac 上的 “command” 键)

**`.exact`修饰符**：

`.exact` 修饰符允许精确控制触发事件所需的系统修饰符的组合

```
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```

**鼠标按键修饰符**：

- `.left`
- `.right`
- `.middle`

### 表单输入绑定

`v-model` 指令简化了将表单输入框的内容同步给 JavaScript 变量的过程，手动连接值绑定和更改事件监听器相对麻烦，`v-model` 指令会自动将元素的值和变量绑定起来。

```
<input v-model="text">
```

v-model 还可以用于各种不同类型的输入，`<textarea>`、`<select>` 元素。它会根据所使用的元素自动使用对应的 DOM 属性和事件组合：

文本类型的 `<input>` 和 `<textarea>` 元素会绑定 value property 并侦听 input 事件；

`<input type="checkbox">` 和 `<input type="radio">` 会绑定 checked property 并侦听 change 事件；

`<select>` 会绑定 value property 并侦听 change 事件。

### 生命周期

#### 生命周期图示

![生命周期图示](img/Web_15.png)

#### 生命周期钩子

##### 注册周期钩子

`onMounted` 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码

```
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

`onUpdated` 钩子可以用来在组件完成重新渲染后运行代码

```
<script setup>
import { onUpdated } from 'vue'

onUpdated(() => {
  console.log(`the component is now updated.`)
})
</script>
```

`onBeforeMount` 钩子可以用来在组件开始渲染前运行代码

```
<script setup>
import { onBeforeMount } from 'vue'

onBeforeMount(() => {
  console.log(`the component is about to be mounted.`)
})
</script>
```

`onBeforeUpdate` 钩子可以用来在组件重新渲染前运行代码

```
<script setup>
import { onBeforeUpdate } from 'vue'

onBeforeUpdate(() => {
  console.log(`the component is about to be updated.`)
})
</script>
```

## Ajax

Ajax: 全称 Asynchronous JavaScript And XML，异步的 JavaScript 和 XML

作用如下：

- 与服务器进行数据交换：通过 Ajax 可以给服务器发送请求，并获取服务器响应的数据。

  - 如下图所示前端资源被浏览器解析，但是前端页面上缺少数据，前端可以通过 Ajax 技术，向后台服务器发起请求，后台服务器接受到前端的请求，从数据库中获取前端需要的资源，然后响应给前端，前端在通过我们学习的 vue 技术，可以将数据展示到页面上，这样用户就能看到完整的页面了。此处可以对比 JavaSE 中的网络编程技术来理解。

![Ajax流程图](img/Web_16.png)

- 异步交互：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术，如：搜索联想、用户名是否可用的校验等等。

  - Ajax 请求不会导致整个页面的重新加载，并且只针对局部模块的数据进行了数据的更新，不会对整个页面的其他地方进行数据的更新，这样就大大提升了页面的加载速度，用户体验高。

**同步异步**

- 同步请求发送过程如下：

  浏览器页面在发送请求给服务器，在服务器处理请求的过程中，浏览器页面不能做其他的操作，只能等到服务器响应结束后才能，浏览器页面才能继续做其他的操作。

![同步请求发送过程](img/Web_17.png)

- 异步请求发送过程如下：

  浏览器页面发送请求给服务器，在服务器处理请求的过程中，浏览器页面还可以做其他的操作

![异步请求发送过程](img/Web_18.png)

### Axios
