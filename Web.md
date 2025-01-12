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

```html
<html>
  <head>
    <title> </title>
  </head>
  <body></body>
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

```html
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

```css
元素名称 {
  css样式名: css样式值；;
}
```

2. **id 选择器**

- 选择器的名字前面需要加上#
- 作用：选择器中的样式会作用于指定 id 的标签上，而且有且只有一个标签（由于 id 是唯一的）

```css
#id属性值 {
  css样式名: css样式值；;
}
```

3. **类选择器**

- 选择器的名字前面需要加上 .
- 作用：选择器中的样式会作用于所有 class 的属性值和该名字一样的标签上，可以是多个

```css
.class属性值 {
  css样式名: css样式值；;
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

```js
function 函数名(参数1,参数2..){
    要执行的代码
}
```

因为 JavaScript 是弱数据类型的语言，所以有如下几点需要注意：

- 形式参数不需要声明类型，并且 JavaScript 中不管什么类型都是 let 或者 var 去声明，加上也没有意义。

- 返回值也不需要声明类型，直接 return 即可

- 第二种定义格式：

```js
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

```js
arr.forEach(function (e) {
  console.log(e);
});

arr.forEach((e) => console.log(e));
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

```js
var 对象名 = {
  属性名1: 属性值1,
  属性名2: 属性值2,
  属性名3: 属性值3,
  函数名称: function (形参列表) {},
};
```

通过`对象名.属性名`调用属性或者`对象名.函数名称(参数)`调用函数。

###### JSON 对象

JSON 对象：**J**ava**S**cript **O**bject **N**otation，JavaScript 对象标记法。是通过 JavaScript 标记法书写的文本。

**格式**：

```js
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

```js
let message: string = "hello,world";
```

- 一般可以省略，因为可以根据后面的字面量推断出前面变量类型

##### 标注参数

```js
function greet(name: string): void {
  //...
}
```

##### 标注返回值

```js
function add(a: number, b: number): number {
  return a + b;
}
```

- 一般也可以省略，因为可以根据返回值做类型推断

##### 复杂类型

**type**

```js
type Cat = {
  name: string,
  age: number,
};

const c1: Cat = { name: "小白", age: 1 };
const c2: Cat = { name: "小花" }; // 错误: 缺少 age 属性
const c3: Cat = { name: "小黑", age: 1, sex: "公" }; // 错误: 多出 sex 属性
```

**interface**

```js
interface Cat {
  name: string;
  age: number;
}

const c1: Cat = { name: "小白", age: 1 };
const c2: Cat = { name: "小花" }; // 错误: 缺少 age 属性
const c3: Cat = { name: "小黑", age: 1, sex: "公" }; // 错误: 多出 sex 属性
```

**可选属性**

```js
interface Cat {
  name: string;
  age?: number;
}

const c1: Cat = { name: "小白", age: 1 };
const c2: Cat = { name: "小花" }; // 正确: age 属性可选
```

可选属性需要注意处理 `undefined` 值，否则会报错。

##### 方法类型

```js
interface Api {
  foo(): void;
  bar(str: string): string;
}

function test(api: Api) {
  api.foo();
  console.log(api.bar("hello"));
}

test({
  foo() {
    console.log("ok");
  },
  bar(str: string) {
    return str.toUpperCase();
  },
});
```

##### 字面量类型

```js
function printText(s: string, alignment: "left" | "right" | "center") {
  console.log(s, alignment);
}

printText("hello", "left");
printText("hello", "aaa"); // 错误: 取值只能是 left | right | center
```

##### nullish 类型

```js
function test(x?: string | null) {
  console.log(x?.toUpperCase());
}

test("aaa");
test(null);
test();
```

- x?: string | null 表示可能是 undefined 或者是 string 或者是 null

##### 泛型

```js
interface Ref<T> {
  value: T;
}

const r1: Ref<string> = { value: "hello" };
const r2: Ref<number> = { value: 123 };
const r3: Ref<boolean> = { value: true };
```

- 泛型的要点就是 `<类型参数>`，把【类型】也当作一个变化的要素，像参数一样传递过来，这样就可以派生出结构相似的新类型

函数定义也支持泛型

```js
function ref<T>(n: T): Ref<T> {
  return { value: n };
}

const v1 = ref("hello"); // Ref<string>
const v2 = ref(123.3333); // Ref<number>

v1.value.toLocaleLowerCase();
v2.value.toFixed(2);
```

## Vue-文档自学（废）

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

```shell
npm init vite@latest

cd <my-project>
npm install
npm run dev
```

#### 修改端口

```ts
// vite.config.ts

export default defineConfig({
  //...

  server: {
    port: 3000,
    host: "0.0.0.0",
  },
});
```

#### 配置代理

为了避免前后端服务器联调时，相关请求产生跨域问题，需要配置代理

```ts
// vite.config.ts

export default defineConfig({
  //...

  server: {
    //...

    proxy: {
      "/api": {
        target: "http://localhost:4000",
        changeOrigin: true,
      },
    },
  },
});
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

```js
import { createApp } from "vue";
const app = createApp({
  /* 根组件选项 */
});
```

**根组件**`src/App.vue`：传入的`createApp`对象实际上是一个组件，每个应用都需要一个“根组件”，其他组件将作为其子组件嵌套在根组件下。

#### Vue 组件

Vue 组件文件是以`.vue`结尾，每个组件由三个部分组成

```vue
<script setup lang="ts"></script>

<template></template>

<style scoped></style>
```

- script 代码部分，控制模板的数据来源和行为

- template 模板部分，由它生成 html 代码

#### main.ts

```ts
import { createApp } from "vue";
import App from "./App.vue";

createApp(App).mount("#app");
```

- createApp 就是创建一个 Vue 应用实例，它接受的参数 App 即根组件

- mount 就是把根组件生成的 html 代码片段挂载到 index.html 中的 id 为 `app` 的元素上

#### ref 和 reactive

- ref 能将任意类型的数据变为【响应式】的

- reactive 只能将对象类型变为【响应式】，对基本类型无效（例如 string，number，boolean）

```ts
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

```ts
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

```ts
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

```ts
computed(() => {});
```

- 计算属性具备缓存功能，即传入参数的值发生了变化，才会重新计算

- 如果用函数实现相同功能，则没有缓存功能，不会再当参数改变时重新计算

#### xhr 请求

```ts
const xhr = new XMLHttpRequest(); // 创建 XMLHttpRequest 对象

// 当响应返回时，会触发 onload 事件
xhr.onload = function () {
  console.log(xhr.response); // 响应数据
};

xhr.open("Get", url); // 打开请求
xhr.responseType = "json"; // 设置响应类型
xhr.send(); // 发送请求
```

### 模板语法

Vue 使用一种基于 HTML 的模板语法，能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。

在底层机制中，Vue 会将模板编译成高度优化的 JavaScript 代码。结合响应式系统，当应用状态变更时，Vue 能够智能地推导出需要重新渲染的组件的最少数量，并应用最少的 DOM 操作。

#### 文本插值

最基础的数据绑定形式是文本插值，使用的是 `{{ }}` 语法，该语法会被替换为相应组件实例中 `msg` 属性的值，每次 `msg` 属性更改时也会同步更新

```vue
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

```vue
<div :id="dynamicId"></div>
```

##### 同名简写

如果 attribute 的名称与绑定的 JavaScript 值的名称相同，那么可以进一步简化语法，省略 attribute 值：

```vue
<!-- 与 :id="id" 等效 -->
<div :id="id"></div>
```

##### 布尔型 Attribute

**布尔型 attribute** 依据 true/false 值来决定 attribute 是否应该存在于该元素上。 `disabled` 就是最常见的例子

`v-bind` 在这种场景下有所不同：

```vue
<button :disabled="isButtonDisabled">Button</button>
```

1. 如果 `isButtonDisabled` 为 `true` 或者一个空字符串，则 `<button>` 元素将会获得 `disabled` attribute。

2. 如果 `isButtonDisabled` 为 `false`，则 `<button>` 元素将会失去 `disabled` attribute。

##### 动态绑定多个值

```ts
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

```vue
<a :href="url">...</a>
```

此处的 `href` 就是一个参数，它告诉 `v-bind` 指令将表达式 `url` 的值绑定到元素的 `href` attribute 上。

###### 动态参数

在指令参数上也可以使用一个 JavaScript 表达式，需要包含在 `[ ]` 内：

```vue
<a :[key]="url">...</a>
```

此处的 `key` 会作为一个 JavaScript 表达式被动态执行，计算得到的值会被用作最汇总的参数。

动态参数中表达式的值应当是一个字符串或者 `null` ，特殊值 `null` 意为显式移除该绑定。其他非字符串的值会触发警告。

##### 修饰符 Modifiers

修饰符是以点开头的特殊后缀，表明指令需要以一些特定的方式被绑定，后续 `v-on` 和 `v-model` 会详细说明

### 响应式基础

#### 声明响应式状态

##### `ref()`

```ts
import { ref } from "vue";

const count = ref(0);
```

`ref()` 接收参数，并将其包裹在一个带有 `.value` 属性的 ref 对象中返回

```ts
const count = ref(0);

console.log(count.value); // {{ value: 0 }}
console.log(count); // 0

count.value++;

console.log(count.value); // 1
```

##### `<script setup>`

在 `setup()` 函数中手动暴露大量的状态和方法十分繁琐，可以通过单文件组件来避免，使用 `<script setup>` 来大幅度简化代码

```ts
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

```ts
import { nextTick } from "vue";

async function increment() {
  count.value++;
  await nextTick();
  // 现在 DOM 已经更新了
}
```

### 计算属性

使用**计算属性**来描述依赖响应式状态的复杂逻辑

```ts
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

```ts
function caculateBooksMessage() {
  return books.value.length > 0 ? "yes" : "no";
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

```vue
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

```vue
const count = ref(0)

<button @click="count++">{{ count }}</button>
```

在内联事件处理器中访问时间参数：

1. `$event`：原生 DOM 事件对象

```vue
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

2. 使用内联箭头函数

```vue
<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```

#### 方法事件处理器

`v-on` 可以接受一个方法名或对某个方法的调用

```ts
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

```vue
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

```vue
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

```vue
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

```vue
<script setup>
import { onMounted } from "vue";

onMounted(() => {
  console.log(`the component is now mounted.`);
});
</script>
```

`onUpdated` 钩子可以用来在组件完成重新渲染后运行代码

```vue
<script setup>
import { onUpdated } from "vue";

onUpdated(() => {
  console.log(`the component is now updated.`);
});
</script>
```

`onBeforeMount` 钩子可以用来在组件开始渲染前运行代码

```vue
<script setup>
import { onBeforeMount } from "vue";

onBeforeMount(() => {
  console.log(`the component is about to be mounted.`);
});
</script>
```

`onBeforeUpdate` 钩子可以用来在组件重新渲染前运行代码

```vue
<script setup>
import { onBeforeUpdate } from "vue";

onBeforeUpdate(() => {
  console.log(`the component is about to be updated.`);
});
</script>
```

## Ajax-文档自学（废）

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

## Tomcat

Web 服务器通常由硬件和软件共同构成

- 硬件：电脑，提供服务供其他客户电脑访问

- 软件：电脑上安装的服务器软件，安装后能提供服务给网络中的其他计算机，将本地文件映射成一个虚拟的 url 地址供网络中的其他人访问

![Tomcat架构图](img/Web_19.png)

### Tomcat 目录

- bin：该目录下存放的是二进制可执行文件，如果是安装版，那么这个目录下会有两个 exe 文件：tomcat10.exe、tomcat10w.exe，前者是在控制台下启动 Tomcat，后者是弹出 GUI 窗口启动 Tomcat

- conf：这是一个非常非常重要的目录，这个目录下有四个最为重要的文件：

  - **server.xml：配置整个服务器信息。例如修改端口号。默认 HTTP 请求的端口号是：8080**

  - tomcat-users.xml：存储 tomcat 用户的文件，这里保存的是 tomcat 的用户名及密码，以及用户的角色信息。可以按着该文件中的注释信息添加 tomcat 用户，然后就可以在 Tomcat 主页中进入 Tomcat Manager 页面了；

  - - web.xml：部署描述符文件，这个文件中注册了很多 MIME 类型，即文档类型。这些 MIME 类型是客户端与服务器之间说明文档类型的，如用户请求一个 html 网页，那么服务器还会告诉客户端浏览器响应的文档是 text/html 类型的，这就是一个 MIME 类型。客户端浏览器通过这个 MIME 类型就知道如何处理它了。当然是在浏览器中显示这个 html 文件了。但如果服务器响应的是一个 exe 文件，那么浏览器就不可能显示它，而是应该弹出下载窗口才对。MIME 就是用来说明文档的内容是什么类型的！

- context.xml：对所有应用的统一配置，通常我们不会去配置它。

- lib：Tomcat 的类库，里面是一大堆 jar 文件。如果需要添加 Tomcat 依赖的 jar 文件，可以把它放到这个目录中，当然也可以把应用依赖的 jar 文件放到这个目录中，这个目录中的 jar 所有项目都可以共享之，但这样你的应用放到其他 Tomcat 下时就不能再共享这个目录下的 jar 包了，所以建议只把 Tomcat 需要的 jar 包放到这个目录下；

- logs：这个目录中都是日志文件，记录了 Tomcat 启动和关闭的信息，如果启动 Tomcat 时有错误，那么异常也会记录在日志文件中。

- temp：存放 Tomcat 的临时文件，这个目录下的东西可以在停止 Tomcat 后删除！

- **webapps：存放 web 项目的目录，其中每个文件夹都是一个项目**；如果这个目录下已经存在了目录，那么都是 tomcat 自带的项目。其中 ROOT 是一个特殊的项目，在地址栏中访问：http://127.0.0.1:8080，没有给出项目目录时，对应的就是ROOT项目.http://localhost:8080/examples，进入示例项目。其中examples"就是项目名，即文件夹的名字。

- work：运行时生成的文件，最终运行的文件都在这里。通过 webapps 中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成 work 目录。当客户端用户访问一个 JSP 文件时，Tomcat 会通过 JSP 生成 Java 文件，然后再编译 Java 文件生成 class 文件，生成的 java 和 class 文件都会存放到这个目录下。

- LICENSE：许可证。

- NOTICE：说明文件。

### WEB 项目的标准结构

在 Tomcat 的 webapps 目录下创建一个文件夹，这个文件夹就是你的 WEB 项目的名字

app 本应用根目录：

- static 非必要目录,约定俗成的名字,一般在此处放静态资源 (css js img)

- WEB-INF 必要目录,必须叫 WEB-INF,受保护的资源目录,浏览器通过 url 不可以直接访问的目录

  - classes 必要目录,src 下源代码,配置文件,编译后会在该目录下,web 项目中如果没有源码,则该目录不会出现

  - lib 必要目录,项目依赖的 jar 编译后会出现在该目录下,web 项目要是没有依赖任何 jar,则该目录不会出现

  - web.xml 必要文件,web 项目的基本配置文件. 较新的版本中可以没有该文件,但是学习过程中还是需要该文件

- index.html 非必要文件,index.html/index.htm/index.jsp 为默认的欢迎页

> url 的组成部分和项目中资源的对应关系：

![url的组成部分和项目中资源的对应关系](img/Web_20.png)

### WEB 项目部署的方式

> 方式 1 直接将编译好的项目放在 webapps 目录下 (已经演示)

> 方式 2 将编译好的项目打成 war 包放在 webapps 目录下,tomcat 启动后会自动解压 war 包(其实和第一种一样)

> 方式 3 可以将项目放在非 webapps 的其他目录下,在 tomcat 中通过配置文件指向 app 的实际磁盘路径

- 在磁盘的自定义目录上准备一个 app

![磁盘的自定义目录准备app](img/Web_21.png)

- 在 tomcat 的 conf 下创建 Catalina/localhost 目录,并在该目录下准备一个 app.xml 文件

```
<!--
	path: 项目的访问路径,也是项目的上下文路径,就是在浏览器中,输入的项目名称
    docBase: 项目在磁盘中的实际路径
 -->
<Context path="/app" docBase="D:\mywebapps\app" />
```

- 启动 tomcat 访问测试即可

### IDEA 中开发、部署、运行 WEB 项目

#### IDEA 关联 Tomcat

在 IDEA 设置中的“构建、执行、部署”中的“应用程序服务器”添加`Tomcat Server`

链接配置到对应的 Tomcat 主目录

#### IDEA 创建 WEB 项目

先创建一个空项目，然后在一个空项目下同时存在多个 modules，不用后续来回切换

然后给 modules 添加 Tomcat 依赖（设置中项目结构，选择对应模块添加 Tomcat 库依赖）

单击 modules 然后选择模块设置，选择“facet”，添加 Web Facet，点击“添加应用程序服务器特定的描述符”，选择 Tomcat 服务器（version 5.0），然后就应用确定

#### IDEA Web 项目目录结构

```
App-1
  ├─resources
  ├─src
  └─web
      ├─META-INF
      ├─static
      │  ├─css
      │  ├─img
      │  └─js
      └─WEB-INF
          └─lib
```

![WEB 项目目录结构](img/Web_22.png)

- resources 目录用于存储项目的配置文件，如 properties 文件、xml 文件等，**需要标记为资源根目录（resources root）**。

- src 目录用于存储 Web 项目后端代码，如 Java 类

- web 目录用于存储 Web 项目前端代码

  - static 目录用于存储静态资源

    - css 目录用于存储样式表文件

    - img 目录用于存储图片文件

    - js 目录用于存储 JavaScript 文件

  - WEB-INF 目录用于存储 Web 项目的配置文件

    - lib 目录用于存储依赖的 jar 包（src 文件夹所需要的 jar 包必须放在该目录下），**需要添加为模块库**

    - web.xml 用于配置 Web 项目的基本信息。

  - html 文件用于存储 Web 项目的页面文件

最后可以通过项目结构来查看目前项目有什么环境依赖

#### IDEA 部署、运行 WEB 项目

> 检查 idea 是否识别 modules 为 web 项目并存在将项目构建成发布结构的配置

- 检查工程目录下，Web 目录有无特殊标记（蓝圈）

- 项目设置中，工件（artifacts） 下，有没有对应的输出目录

- 编辑运行/调试配置，添加本地 Tomcat 服务器，然后在部署（development）中选择刚才创建的 Web Module，回到服务器（Server）选择中，默认打开浏览器并且输入对应地址访问项目，配置 JRE 环境（如果没有配置，则会提示配置），然后应用运行配置，运行项目

> 工程结构和可以发布的项目结构之间的目录对应关系

![工程结构和可以发布的项目结构之间的目录对应关系](img/Web_23.png)

> IDEA 部署并运行项目的原理

- idea 并没有直接进将编译好的项目放入 tomcat 的 webapps 中

- idea 根据关联的 tomcat,创建了一个 tomcat 副本,将项目部署到了这个副本中

- idea 的 tomcat 副本在 C:\用户\当前用户\AppData\Local\JetBrains\IntelliJIdea2022.2\tomcat\中

- idea 的 tomcat 副本并不是一个完整的 tomcat,副本里只是准备了和当前项目相关的配置文件而已

- idea 启动 tomcat 时,是让本地 tomcat 程序按照 tomcat 副本里的配置文件运行

- idea 的 tomcat 副本部署项目的模式是通过 conf/Catalina/localhost/\*.xml 配置文件的形式实现项目部署的

![IDEA 部署并运行项目的原理](img/Web_24.png)

## HTTP 简介

![HTTP 协议](img/Web_25.png)

> **HTTP 超文本传输协议** (HTTP-Hyper Text transfer protocol)，是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于 1990 年提出，经过十几年的使用与发展，得到不断地完善和扩展。**它是一种详细规定了浏览器和万维网服务器之间互相通信的规则**，通过因特网传送万维网文档的数据传送协议。客户端与服务端通信时传输的内容我们称之为**报文**。**HTTP 协议就是规定报文的格式。**HTTP 就是一个通信规则，这个规则规定了客户端发送给服务器的报文格式，也规定了服务器发送给客户端的报文格式。实际我们要学习的就是这两种报文。**客户端发送给服务器的称为"请求报文"**，**服务器发送给客户端的称为"响应报文"**。

### HTTP 协议的会话方式

浏览器与服务器之间的通信过程要经历四个步骤：

![浏览器与服务器之间的通信过程](img/Web_26.png)

- 浏览器和 Web 服务器的连接过程是短暂的，每次连接只处理一个请求和响应。对每一个页面的访问，浏览器与 Web 服务器都要建立一次单独的连接

- 浏览器到 Web 服务器之间的所有通讯都是完全独立分开的请求和响应对

### 请求和相应报文

#### 报文格式

> 主体上氛围报文首部和报文主体，中间空行隔开

![报文格式](img/Web_27.png)

> 报文部首可以继续细分为“行”和“头”

![报文部首](img/Web_28.png)

#### 请求报文

> 客户端发送给服务端的报文

- 请求格式报文

  - 请求首行（**请求行**）；GET/POST 资源路径?参数 HTTP/1.1

  - 请求头信息（**请求头**）；

  - 空行；

  - 请求体；POST 请求才有请求体

##### GET 请求

> form 表单发送 GET 请求特点

1. 由于请求参数在请求首行中已经携带了，所以没有请求体，也没有请求空行

2. 请求参数拼接在 url 地址中，地址栏可见（url?name1=value1\&name2=value2），不安全

3. 由于参数在地址栏中携带，所以由大小限制（地址栏数据大小一般限制为 4k），只能携带纯文本

4. get 请求参数只能上传文本数据

5. 没有请求体。所以封装和解析都快，效率高， 浏览器默认提交的请求都是 get 请求比如：地址栏输入回车,超链接,表单默认的提交方式

> 查看 GET 请求行，请求头，请求体

- 请求行组成部分

  - 请求方式 GET

  - 访问服务器的资源路径?参数一=值一&参数二=值二

  - 协议及版本 HTTP/1.1

`GET /index.html?name1=value1&name2=value2 HTTP/1.1`

- 请求头

- 请求空行

- 请求体

  - GET 请求数据不放在请求体

##### POST 请求

> form 表单发送 POST 请求特点

1. POST 请求有请求体，而 GET 请求没有请求体。

2. post 请求数据在请求体中携带，请求体数据大小没有限制，可以用来上传所有内容（文件、文本）

3. 只能使用 post 请求上传文件

4. post 请求报文多了和请求体相关的配置\[请求头]

5. 地址栏参数不可见，相对安全

6. post 效率比 get 低

> 查看 post 的请求行 请求头 请求体

- 请求行组成部分

  - 请求方式 POST

  - 访问服务器的资源路径

  - 协议及版本 HTTP/1.1

`POST /index.html HTTP/1.1`

- 请求头

- 请求空行

- 请求体：路蓝旗提交给服务器的数据

`username=admin&password=123456`

#### 响应报文

> 响应报文格式

- 响应首行（**响应行**）； 协议/版本 状态码 状态码描述

```
HTTP/1.1 200 OK
说明：响应协议为HTTP1.1，响应状态码为200，表示请求成功；
```

- 响应头信息（**响应头**）；

- 空行；

- 响应体；

> 响应状态码：响应码对浏览器来说很重要，它告诉浏览器响应的结果。比较有代表性的响应码如下

- **200：** 请求成功，浏览器会把响应体内容（通常是 html）显示在浏览器中；

- **302：** 重定向，当响应码为 302 时，表示服务器要求浏览器重新再发一个请求，服务器会发送一个响应头 Location 指定新请求的 URL 地址；

- **304：** 使用了本地缓存

- **404：** 请求的资源没有找到，说明客户端错误的请求了不存在的资源；

- **405：** 请求的方式不允许

- **500：** 请求资源找到了，但服务器内部出现了错误；

## Servlet

### 动态资源和静态资源

- 静态资源

  - 无需在程序运行时，通过代码运行生成的资源，在程序运行之前就写好的资源，如：html css js img，音频文件和视频文件

- 动态资源

  - 需要在程序运行时通过代码运行生成的资源，在程序运行之前无法确定的数据，运行时动态生成，如 Servlet、Thymeleaf

  - 动态资源指的不是试图上的动画效果或是简单的人机互动效果

### Servlet 简介

> Servlet (server applet) 是运行在服务端(tomcat)的 Java 小程序，是 sun 公司提供一套定义动态资源规范; 从代码层面上来讲 Servlet 就是一个接口

- 用来接收、处理客户端请求、响应给浏览器的动态资源。在整个 Web 应用中，Servlet 主要负责接收处理请求、协同调度功能以及响应数据。我们可以把 Servlet 称为 Web 应用中的**控制器**

![Web 应用过程](img/Web_29.png)

- 不是所有 Java 类都可以用于处理客户端请求，能处理客户端请求并做出响应的一套技术标准就是 Servlet

- Sercket 试运行在服务端的，所以 Servlet 必须在 Web 项目中开发，且在 Tomcat 这样的服务器中运行

> 请求响应与 HttpServletRequest 和 HttpServletResponse 之间的对应关系

![请求响应与 HttpServletRequest 和 HttpServletResponse 之间的对应关系](img/Web_30.png)

1. Tomcat 接收到请求后，会将请求报文的信息转换成一个 HttpServletRequest 对象，该对象中包含了请求报文中的所有信息

2. Tomcat 同时创建了一个 HttpServletResponse 对象，该对象用于承装要响应给客户端的信息，该对象会被转换成响应报文

3. Tomcat 根据请求中的资源路径找到对应的 servlet，将 servlet 实例化，调用 service 方法，同时将 HttpServletRequest 和 HttpServletResponse 对象作为参数传入

#### 开发 Servlet 过程

- 步骤一：开发一个 web 类型的 module

- 步骤二：开发一个 UserServlet 类，继承 HttpServlet 类

```java
public class UserServlet extends HttpServlet {
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 1. 从request对象中获取请求的所有信息（参数）
      String username = request.getParameter("username");// 根据参数名获取参数值
    // 2. 根据参数生成要响应给客户端的数据
      String info = "YES";
      if(username.equals("admin"))
        info = "NO";
    // 3. 将相应的数据放入response对象
      PrintWriter writer = response.getWriter();
      writer.write(info);
  }
}
```

- 步骤三：在 web.xml 中为 UserServlet 配置请求的映射路径

```xml
<servlet>
  <!--给UserServlet起一个别名-->
  <servlet-name>UserServlet</servlet-name>
  <servlet-class>com.example.UserServlet</servlet-class>
</servlet>

<servlet-mapping>
  <!--关联别名和映射路径-->
  <servlet-name>userServlet</servlet-name>
  <!--可以为一个Servlet匹配多个不同的映射路径,但是不同的Servlet不能使用相同的url-pattern-->
  <url-pattern>/userServlet</url-pattern>
  <!-- <url-pattern>/userServlet2</url-pattern>-->
  <!--
      /        表示通配所有资源,不包括jsp文件
      /*       表示通配所有资源,包括jsp文件
      /a/*     匹配所有以a前缀的映射路径
      *.action 匹配所有以action为后缀的映射路径
  -->
  <!-- <url-pattern>/*</url-pattern>-->
</servlet-mapping>
```

- 步骤四：开发一个 form 表单，向 servlet 发送一个 get 请求并携带 username 参数

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <form action="userServlet">
      请输入用户名:<input type="text" name="username" /> <br />
      <input type="submit" value="校验" />
    </form>
  </body>
</html>
```

1. servlet-api.jar 导入：

- servlet-api 在编码时候需要，在运行的时候，在服务器的环境中，由服务软件（Tomcat）提供，因此我们的 JavaWeb 项目中，在打包/构建的时候，是无需携带 servlet-api.jar 的。

2. Content-Type 响应头：

- MINE 类型响应头 媒体类型，文件类型，响应的数据类型

- MINE 类型用于告诉客户端相应的数据是什么类型的数据，客户端以此类型决定用什么方式解析响应体。

- 设置 Content-Type 响应头

```java
response.setHeader("Content-Type", "text/html;charset=UTF-8");

response.setContentType("text/html;charset=UTF-8");
```

- 一个 servlet-name 可以同时对应多个 servlet-mapping, url-pattern

- 不同的 servlet 不能使用相同的 url-pattern

- url-pattern 匹配规则：

  - url-pattern 精确匹配：/userServlet

  - url-pattern 通配符匹配：

    - `/*`， \*代表模糊

    - `/`，匹配全部，不包含 jsp 文件

    - `/*`，匹配全部，包含 jsp 文件

    - `/a/*`，匹配以 a 开头的路径，不包含 jsp 文件

    - `*.action`，匹配以 action 结尾的路径，不包含 jsp 文件

- `@WebServlet(urlPatterns = {"/userServlet"})`/`WebServlet("/userServlet")`使用注解配置可以省略 xml 配置，但是两种方式不能同时使用

> @WebServlet 注解的源码解析

```java
package jakarta.servlet.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * @since Servlet 3.0
 */
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {

    /**
     * The name of the servlet
     * 相当于 servlet-name
     * @return the name of the servlet
     */
    String name() default "";

    /**
     * The URL patterns of the servlet
     * 如果只配置一个url-pattern ,则通过该属性即可,和urlPatterns属性互斥
     * @return the URL patterns of the servlet
     */
    String[] value() default {};

    /**
     * The URL patterns of the servlet
     * 如果要配置多个url-pattern ,需要通过该属性,和value属性互斥
     * @return the URL patterns of the servlet
     */
    String[] urlPatterns() default {};

    /**
     * The load-on-startup order of the servlet
     * 配置Servlet是否在项目加载时实例化
     * @return the load-on-startup order of the servlet
     */
    int loadOnStartup() default -1;

    /**
     * The init parameters of the servlet
     * 配置初始化参数
     * @return the init parameters of the servlet
     */
    WebInitParam[] initParams() default {};

    /**
     * Declares whether the servlet supports asynchronous operation mode.
     *
     * @return {@code true} if the servlet supports asynchronous operation mode
     * @see jakarta.servlet.ServletRequest#startAsync
     * @see jakarta.servlet.ServletRequest#startAsync( jakarta.servlet.ServletRequest,jakarta.servlet.ServletResponse)
     */
    boolean asyncSupported() default false;

    /**
     * The small-icon of the servlet
     *
     * @return the small-icon of the servlet
     */
    String smallIcon() default "";

    /**
     * The large-icon of the servlet
     *
     * @return the large-icon of the servlet
     */
    String largeIcon() default "";

    /**
     * The description of the servlet
     *
     * @return the description of the servlet
     */
    String description() default "";

    /**
     * The display name of the servlet
     *
     * @return the display name of the servlet
     */
    String displayName() default "";
}
```

- `<load-on-startup>`标签（在`<servlet>`标签中）：默认值-1 代表 tomcat 启动时不会实例化该 servlet，设置值大于 0 时，代表在启动时就实例化该 servlet，设置值越小，启动时实例化的优先级越高。

- `default-servlet`：默认 servlet 是指当服务器找不到请求的资源时，会默认调用的 servlet，可以配置多个 default-servlet，但是只能有一个 default-servlet 被激活。

#### Servlet 生命周期

##### 生命周期简介

> 什么是 Servlet 生命周期？

- 应用程序中的对象不仅在空间上有层次结构的关系，在时间上也会因为处于程序运行中的不同阶段而表现出不同状态和不同行为——这就是对象的生命周期

- 简单的叙述生命周期，就是对象在容器中从开始创建到销毁的过程

> Servlet 容器

- Servlet 对象是 Servlet 容器创建的，生命周期方法都是由容器（目前我们使用的是 Tomcat）调用的。

> Servlet 主要的生命周期执行特点

| 生命周期 | 对应方法                                                 | 执行时机               | 执行次数 |
| -------- | -------------------------------------------------------- | ---------------------- | -------- |
| 构造对象 | 构造器                                                   | 第一次请求或者容器启动 | 1        |
| 初始化   | init()                                                   | 构造完毕后             | 1        |
| 处理服务 | service(HttpServletRequest req,HttpServletResponse resp) | 每次请求               | 多次     |
| 销毁     | destory()                                                | 容器关闭               | 1        |

#### Servlet 继承结构

##### Servlet 接口

Servlet 规范接口,所有的 Servlet 必须实现

- public void init(ServletConfig config) throws ServletException;

  - 初始化方法,容器在构造 servlet 对象后,自动调用的方法,容器负责实例化一个 ServletConfig 对象,并在调用该方法时传入

  - ServletConfig 对象可以为 Servlet 提供初始化参数

- public ServletConfig getServletConfig();

  - 获取 ServletConfig 对象的方法,后续可以通过该对象获取 Servlet 初始化参数

- public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException;

  - 处理请求并做出响应的服务方法,每次请求产生时由容器调用

  - 容器创建一个 ServletRequest 对象和 ServletResponse 对象,容器在调用 service 方法时,传入这两个对象

- public String getServletInfo();

  - 获取 ServletInfo 信息的方法

- public void destroy();

  - Servlet 实例在销毁之前调用的方法

##### GenericServlet 抽象类

GenericServlet 抽象类是对 Servlet 接口一些固定功能的粗糙实现,以及对 service 方法的再次抽象声明,并定义了一些其他相关功能方法，侧重于除了 service 方法外的实现

- private transient ServletConfig config;

  - 初始化配置对象作为属性

- public GenericServlet() { }

  - 构造器,为了满足继承而准备

- public void destroy() { }

  - 销毁方法的平庸实现

- public String getInitParameter(String name)

  - 获取初始参数的快捷方法

- public Enumeration<String> getInitParameterNames()

  - 返回所有初始化参数名的方法

- public ServletConfig getServletConfig()

  - 获取初始 Servlet 初始配置对象 ServletConfig 的方法

- public ServletContext getServletContext()

  - 获取上下文对象 ServletContext 的方法

- public String getServletInfo()

  - 获取 Servlet 信息的平庸实现

- public void init(ServletConfig config) throws ServletException()

  - 初始化方法的实现,并在此调用了 init 的重载方法

- public void init() throws ServletException

  - 重载 init 方法,为了让我们自己定义初始化功能的方法

- public void log(String msg)

- public void log(String message, Throwable t)

  - 打印日志的方法及重载

- public abstract void service(ServletRequest req, ServletResponse res) throws ServletException, IOException;

  - 服务方法再次声明

- public String getServletName()

  - 获取 ServletName 的方法

##### HttpServlet 抽象类

abstract class HttpServlet extends GenericServlet HttpServlet 抽象类,除了基本的实现以外,增加了更多的基础功能，侧重于 service 方法的具体实现

- private static final String METHOD_DELETE = "DELETE";

- private static final String METHOD_HEAD = "HEAD";

- private static final String METHOD_GET = "GET";

- private static final String METHOD_OPTIONS = "OPTIONS";

- private static final String METHOD_POST = "POST";

- private static final String METHOD_PUT = "PUT";

- private static final String METHOD_TRACE = "TRACE";

  - 上述属性用于定义常见请求方式名常量值

- public HttpServlet() {}

  - 构造器,用于处理继承

- public void service(ServletRequest req, ServletResponse res) throws
  ServletException, IOException

  - 对服务方法的实现

  - 在该方法中,将请求和响应对象转换成对应 HTTP 协议的 HttpServletRequest HttpServletResponse 对象

  - 调用重载的 service 方法

- public void service(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException

  - 重载的 service 方法,被重写的 service 方法所调用

  - 在该方法中,通过请求方式判断,调用具体的 do\*\*\*方法完成请求的处理

- protected void doGet(HttpServletRequest req, HttpServletResponse
  resp) throws ServletException, IOException

- protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

- protected void doHead(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

- protected void doPut(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

- protected void doDelete(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

- protected void doOptions(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

- protected void doTrace(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException

  - 对应不同请求方式的处理方法

  - 除了 doOptions 和 doTrace 方法,其他的 do\*\*\* 方法都在故意响应错误信息

##### 继承结构图解

![Servlet 继承结构图解](img/Web_31.png)

#### ServletConfig 和 ServletContext

> ServletConfig 是什么

- 为 Servlet 提供初始配置参数的一种对象，每个 Servlet 都有自己独立唯一的 ServletConfig 对象

- 容器会为每个 Servlet 实例化一个 ServletConfig 对象，并通过 Servlet 生命周期的 init 方法传入 Servlet 作为属性

![ServletConfig 对象作用过程](img/Web_32.png)

> ServletConfig 是一个接口，定义了如下 API

```java
package jakarta.servlet;
import java.util.Enumeration;
public interface ServletConfig {
    String getServletName();
    ServletContext getServletContext();
    String getInitParameter(String var1);
    Enumeration<String> getInitParameterNames();
}
```

| 方法名                  | 作用                                                               |
| ----------------------- | ------------------------------------------------------------------ |
| getServletName()        | 获取\<servlet-name>HelloServlet\</servlet-name>定义的 Servlet 名称 |
| getServletContext()     | 获取 ServletContext 对象                                           |
| getInitParameter()      | 获取配置 Servlet 时设置的『初始化参数』，根据名字获取值            |
| getInitParameterNames() | 获取所有初始化参数名组成的 Enumeration 对象                        |

> ServletContext 是什么

- ServletContext 对象又称呼为上下文对象，或者叫应用域对象

- 容器会为每个 app 创建一个独立的唯一的 ServletContext 对象

- ServletContext 对象为所有的 Servlet 所共享

- ServletContext 可以为所有的 Servlet 提供初始配置参数

![ServletContext 对象作用过程](img/Web_33.png)

> ServletContext 用法

- 配置 ServletContext 参数

```xml
    <context-param>
        <param-name>paramA</param-name>
        <param-value>valueA</param-value>
    </context-param>
    <context-param>
        <param-name>paramB</param-name>
        <param-value>valueB</param-value>
    </context-param>
```

- 获取 ServletContext 对象

```java
ServletContext servletContext1 = servletConfig.etServletContext();
ServletContext servletContext2=req.getServletContext();ServletContext servletContext3=getServletContext();
```

- 获取 ServletContext 参数

```java
String paramA = servletContext1.getInitParameter("paramA");
String paramB = servletContext1.getInitParameter("paramB");

Enumeration<String> paramNames = servletContext1.getInitParameterNames();
while(paramNames.hasMoreElements())
  String paramName = paramNames.nextElement();
```

##### ServletContext 的重要 API

> 获取资源的真实路径

```java
String realPath = getServletContext().getRealPath("资源在web目录中的路径");
```

- 如果目标是需要获取项目中某个静态资源的路径，不是工程目录中的路径，而是**部署目录中的路径**；如果直接拷贝其在电脑中的完整路径的话其实是有问题的，因为如果该项目以后部署到公司服务器上的话，路径肯定是会发生改变的，所以需要使用代码动态获取资源的真实路径. 只要使用了 servletContext 动态获取资源的真实路径，**那么无论项目的部署路径发生什么变化，都会动态获取项目运行时候的实际路径**，所以就不会发生由于写死真实路径而导致项目部署位置改变引发的路径错误问题

> 获取请求的上下文路径

```java
String contextPath = getServletContext().getContextPath();
```

- 项目的部署名称,也叫项目的上下文路径,在部署进入 tomcat 时所使用的路径,该路径是可能发生变化的,通过该 API 动态获取项目真实的上下文路径,可以**帮助解决一些后端页面渲染技术或者请求转发和响应重定向中的路径问题**

> 域对象的相关 API

- 域对象: 一些用于存储数据和传递数据的对象,传递数据不同的范围,称之为不同的域,不同的域对象代表不同的域,共享数据的范围也不同

- ServletContext 代表应用,所以 ServletContext 域也叫作应用域,是 webapp 中最大的域,可以在本应用内实现数据的共享和传递

- webapp 中的三大域对象,分别是应用域,会话域,请求域

- 三大域对象都具有的 API 如下

| API                                         | 功能解释            |
| ------------------------------------------- | ------------------- |
| void setAttribute(String key,Object value); | 向域中存储/修改数据 |
| Object getAttribute(String key);            | 获得域中的数据      |
| void removeAttribute(String key);           | 移除域中的数据      |

#### HttpServletRequest

##### HttpServletRequest 简介

> HttpServletRequest 是什么

- HttpServletRequest 是一个接口,其父接口是 ServletRequest

- HttpServletRequest 是 Tomcat 将请求报文转换封装而来的对象,在 Tomcat 调用 service 方法时传入

- HttpServletRequest 代表客户端发来的请求,所有请求中的信息都可以通过该对象获得

##### HttpServletRequest 常见 API

- 获取请求行信息相关(方式,请求的 url,协议及版本)

| API                           | 功能解释                       |
| ----------------------------- | ------------------------------ |
| StringBuffer getRequestURL(); | 获取客户端请求的 url           |
| String getRequestURI();       | 获取客户端请求项目中的具体资源 |
| int getServerPort();          | 获取客户端发送请求时的端口     |
| int getLocalPort();           | 获取本应用在所在容器的端口     |
| int getRemotePort();          | 获取客户端程序的端口           |
| String getScheme();           | 获取请求协议                   |
| String getProtocol();         | 获取请求协议及版本号           |
| String getMethod();           | 获取请求方式                   |

- 获得请求头信息相关

| API                                   | 功能解释                 |
| ------------------------------------- | ------------------------ |
| String getHeader(String headerName);  | 根据头名称获取请求头     |
| Enumeration<String> getHeaderNames(); | 获取所有的请求头名字     |
| String getContentType();              | 获取 content-type 请求头 |

- 获得请求参数相关

| API                                                     | 功能解释                             |
| ------------------------------------------------------- | ------------------------------------ |
| String getParameter(String parameterName);              | 根据请求参数名获取请求单个参数值     |
| String[] getParameterValues(String parameterName);      | 根据请求参数名获取请求多个参数值数组 |
| Enumeration<String> getParameterNames();                | 获取所有请求参数名                   |
| Map<String, String[]> getParameterMap();                | 获取所有请求参数的键值对集合         |
| BufferedReader getReader() throws IOException;          | 获取读取请求体的字符输入流           |
| ServletInputStream getInputStream() throws IOException; | 获取读取请求体的字节输入流           |
| int getContentLength();                                 | 获得请求体长度的字节数               |

- 其他 API

| API                                          | 功能解释                      |
| -------------------------------------------- | ----------------------------- |
| String getServletPath();                     | 获取请求的 Servlet 的映射路径 |
| ServletContext getServletContext();          | 获取 ServletContext 对象      |
| Cookie[] getCookies();                       | 获取请求中的所有 cookie       |
| HttpSession getSession();                    | 获取 Session 对象             |
| void setCharacterEncoding(String encoding) ; | 设置请求体字符集              |

#### HttpServletResponse

##### HttpServletResponse 简介

> HttpServletResponse 是什么

- HttpServletResponse 是一个接口,其父接口是 ServletResponse

- HttpServletResponse 是 Tomcat 预先创建的,在 Tomcat 调用 service 方法时传入

- HttpServletResponse 代表对客户端的响应,该对象会被转换成响应的报文发送给客户端,通过该对象我们可以设置响应信息

##### HttpServletResponse 常见 API

- 设置响应行相关

| API                       | 功能解释       |
| ------------------------- | -------------- |
| void setStatus(int code); | 设置响应状态码 |

- 设置响应头相关

| API                                                    | 功能解释                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| void setHeader(String headerName, String headerValue); | 设置/修改响应头键值对                                |
| void setContentType(String contentType);               | 设置 content-type 响应头及响应字符集(设置 MIME 类型) |

- 设置响应体相关

| API                                                       | 功能解释                                                  |
| --------------------------------------------------------- | --------------------------------------------------------- |
| PrintWriter getWriter() throws IOException;               | 获得向响应体放入信息的字符输出流                          |
| ServletOutputStream getOutputStream() throws IOException; | 获得向响应体放入信息的字节输出流                          |
| void setContentLength(int length);                        | 设置响应体的字节长度,其实就是在设置 content-length 响应头 |

- 其他 API

| API                                                          | 功能解释                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| void sendError(int code, String message) throws IOException; | 向客户端响应错误信息的方法,需要指定响应码和响应信息 |
| void addCookie(Cookie cookie);                               | 向响应体中增加 cookie                               |
| void setCharacterEncoding(String encoding);                  | 设置响应体字符集                                    |

> MIME 类型

- MIME 类型,可以理解为文档类型,用户表示传递的数据是属于什么类型的文档

- 浏览器可以根据 MIME 类型决定该用什么样的方式解析接收到的响应体数据

- 可以这样理解: 前后端交互数据时,告诉对方发给对方的是 html/css/js/图片/声音/视频/... ...

- tomcat/conf/web.xml 中配置了常见文件的拓展名和 MIMIE 类型的对应关系

- 常见的 MIME 类型举例如下

| 文件拓展名                  | MIME 类型              |
| --------------------------- | ---------------------- |
| .html                       | text/html              |
| .css                        | text/css               |
| .js                         | application/javascript |
| .png /.jpeg/.jpg/... ...    | image/jpeg             |
| .mp3/.mpe/.mpeg/ ... ...    | audio/mpeg             |
| .mp4                        | video/mp4              |
| .m1v/.m1v/.m2v/.mpe/... ... | video/mpeg             |

#### 请求转发和响应重定向

- 请求转发和响应重定向是 web 应用中间接访问项目资源的两种手段,也是 Servlet 控制页面跳转的两种手段

- 请求转发通过 HttpServletRequest 实现,响应重定向通过 HttpServletResponse 实现

##### 请求转发

![请求转发逻辑图](img/Web_34.png)

- 请求转发通过 HttpServletRequest 对象获取请求转发器实现

- 请求转发是服务器内部的行为,对客户端是屏蔽的

- 客户端只发送了一次请求,客户端地址栏不变

- 服务端只产生了一对请求和响应对象,这一对请求和响应对象会继续传递给下一个资源

- 因为全程只有一个 HttpServletRequset 对象,所以请求参数可以传递,请求域中的数据也可以传递

- 请求转发可以转发给其他 Servlet 动态资源,也可以转发给一些静态资源以实现页面跳
  转

- 请求转发可以转发给 WEB-INF 下受保护的资源，该方式也是 WEB-INF 下的资源唯一访问方式

- 请求转发不能转发到本项目以外的外部资源

##### 响应重定向

![响应重定向逻辑图](img/Web_35.png)

- 响应重定向通过 HttpServletResponse 对象的 sendRedirect 方法实现

- 响应重定向是服务端通过 302 响应码和路径,告诉客户端自己去找其他资源,是在服务端提示下的,客户端的行为

- 客户端至少发送了两次请求,客户端地址栏是要变化的

- 服务端产生了多对请求和响应对象,且请求和响应对象不会传递给下一个资源

- 因为全程产生了多个 HttpServletRequset 对象,所以请求参数不可以传递,请求域中的数据也不可以传递

- 重定向可以是其他 Servlet 动态资源,也可以是一些静态资源以实现页面跳转

- 重定向不可以到给 WEB-INF 下受保护的资源

- 重定向可以到本项目以外的外部资源

##### 总结

同样能实现页面跳转，优先使用响应重定向

#### web 乱码和路径问题总结

##### 乱码问题

> 乱码问题产生的根本原因

- 数据的编码和解码使用的不是同一个字符集

- 使用了不支持某个语言文字的字符集

> 各个字符集的兼容性

![各个字符集的兼容性](img/Web_36.png)

##### 路径问题

> 相对路径和绝对路径

- 相对路径

  - 相对路径的规则是: 以当前资源所在的路径为出发点去寻找目标资源

  - 相对路径不以 / 开头

  - 在 file 协议下,使用的是磁盘路径

  - 在 http 协议下,使用的是 url 路径

  - 相对路径中可以使用 ./表示当前资源所在路径,可以省略不写

  - 相对路径中可以使用../表示当前资源所在路径的上一层路径,需要时要手动添加

- 绝对路径

  - 绝对路径的规则是: 使用以一个固定的路径做出出发点去寻找目标资源,和当前资源所
    在的路径没有关系

  - 绝对路径要以/ 开头

  - 绝对路径的写法中,不以当前资源的所在路径为出发点,所以不会出现 ./ 和../

  - 不同的项目和不同的协议下,绝对路径的基础位置可能不同,要通过测试确定

  - 绝对路径的好处就是:无论当前资源位置在哪,寻找目标资源路径的写法都一致

- base 标签定义页面相对路径公共前缀

  - base 标签定义在 head 标签中,用于定义相对路径的公共前缀

  - base 标签定义的公共前缀只在相对路径上有效,绝对路径中无效

  - 如果相对路径开头有 ./ 或者../修饰,则 base 标签对该路径同样无效

- 模组设置直接清除上下文路径

### MVC 架构模式

> MVC（Model View Controller）是软件工程中的一种 **`软件架构模式`**，它把软件系统分为 **`模型`**、**`视图`**和 **`控制器`**三个基本部分。用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

目的：

1. **高内聚低耦合**

2. **开闭原则**

- **M**：Model 模型层,具体功能如下

  1. 存放和数据库对象的实体类以及一些用于存储非数据库表完整相关的 VO 对象

  2. 存放一些对数据进行逻辑运算操作的的一些业务处理代码

- **V**：View 视图层,具体功能如下

  1. 存放一些视图文件相关的代码 html css js 等

  2. 在前后端分离的项目中,后端已经没有视图文件,该层次已经衍化成独立的前端项目

- **C**：Controller 控制层,具体功能如下

  1. 接收客户端请求,获得请求数据

  2. 将准备好的数据响应给客户端

> MVC 模式下，项目中的常见包

- M:

  1. 实体类包(pojo /entity /bean) 专门存放和数据库对应的实体类和一些 VO 对象

  2. 数据库访问包(dao/mapper) 专门存放对数据库不同表格 CURD 方法封装的一些类

  3. 服务包(service) 专门存放对数据进行业务逻辑运算的一些类

- C:

  1. 控制层包(controller)

- V:

  1. web 目录下的视图资源 html css js img 等

  2. 前端工程化后,在后端项目中已经不存在了

**非前后端分离的 MVC**

![非前后端分离的 MVC](img/Web_37.png)

**前后端分离的 MVC**

![前后端分离的 MVC](img/Web_38.png)

## 会话

### 会话管理概述

> HTTP 是无状态协议

- 无状态就是不保存状态，即无状态协议(stateless)，HTTP 协议自身不对请求和响应之间的通信状态进行保存，也就是说，在 HTTP 协议这个级别，协议对发送过的请求或者响应都不做持久化处理

- 浏览器发送请求，服务器接受并响应，但是服务器不记录请求是否来自哪个浏览器，服务器没记录浏览器的特征，即客户端的状态

> 实现会话管理：Cookie 和 Session

- cookie 是在客户端保留少量数据的技术，主要通过响应头向客户端响应一些客户端要保留的信息

- session 是在服务端保留更多数据的技术，主要通过 HttpSession 对象保存一些和客户端相关的信息

- cookie 和 session 配合记录请求状态

### Cookie

> Cookie 是一种客户端会话技术，Cookie 由服务端产生，它是服务器存放在浏览器的一小份数据，浏览器以后每次访问该服务器的时候都会将这小份数据携带到服务器

- 服务端创建 cookie，将 cookie 放入响应对象中，Tomcat 容器将 cookie 转化为`set-cookie`的响应头，响应给客户端

- 客户端在收到 cookie 的响应头，在下次请求该服务的资源时，会以 cookie 请求头的形式携带之前收到的 cookie

- cookie 是一种键值对格式的数据，从 tomcat8.5 开始保存中文，但是不推荐

- 由于 cookie 是存储于客户端的数据，比较容易暴露，一般不存储一些敏感或者影响安全的数据

> 原理图：

![Cookie原理图](img/Web_39.png)

常见应用场景有：记录用户名、保存电影播放进度

#### Cookie 的使用

> ServletA 向响应中增加 Cookie

```java
@WebServlet("/servletA")
public class ServletA extends HttpServlet {
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 创建Cookie
    Cookie cookie1 = new Cookie("c1","c1_message");
    Cookie cookie2 = new Cookie("c2","c2_message");
    // 将cookie放入响应对象
    resp.addCookie(cookie1);
    resp.addCookie(cookie2);
  }
}
```

> ServletB 从请求中读取 Cookie

```java
@WebServlet("/servletB")
public class ServletB extends HttpServlet {
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取请求中的cookie
    Cookie[] cookies = req.getCookies();
    // 迭代cookies数组
    if(null != cookies && cookies.length > 0)
      for(Cookie cookie : cookies)
        System.out.println(cookie.getName() + ":" + cookie.getValue());
}
```

#### Cookie 的时效性

> 默认情况下 Cookie 的有效期是一次会话范围内，我们可以通过 cookie 的`setMaxAge()`方法让 Cookie 持久化保存到浏览器上

- 会话级 Cookie

  - 服务器端并没有明确指定 Cookie 的存在时间

  - 在浏览器端，Cookie 数据存在于内存中

  - 只要浏览器还开着，Cookie 数据就一直存在

  - 浏览器关闭，内存中的 Cookie 数据就会被释放

- 持久化 Cookie

  - 服务器明确设置了 Cookie 的存在时间
  - 在浏览器端，Cookie 数据会被保存到硬盘上
  - Cookie 在硬盘上存在的时间根据服务器端限定的时间来管控，不受浏览器关闭的影响
  - 持久化 Cookie 达到了预设的时间会被释放

`cookie.setMaxAge(int expiry)`参数单位秒，表示 cookie 的持久化时间，如果设置参数为 0，表示将浏览器中保存的该 cookie 删除

#### Cookie 的提交路径

> 访问互联网资源时不能每次都需要把所有 Cookie 带上，访问不同的资源时，可以携带不同的 Cookie。可以通过 Cookie 的`setPath(String path)`对 Cookie 的路径进行设置

```java
public class ServletA extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 创建Cookie
        Cookie cookie1 =new Cookie("c1","c1_message");
        // 设置cookie的提交路径
        cookie1.setPath("/web03_war_exploded/servletB");
        Cookie cookie2 =new Cookie("c2","c2_message");
        // 将cookie放入响应对象
        resp.addCookie(cookie1);
        resp.addCookie(cookie2);
    }
}
```

经过以上设置，只有访问`/web03_war_exploded/servletB`时，才会带上`c1`这个 Cookie

### Session

#### Session 概述

> HttpSession 是一种保留更多信息在服务端的一种技术，服务器会为每一个客户端开辟一块内存空间，即 session 对象，客户端在发送请求时，都可以使用自己的 session，这样服务端就可以通过 session 来记录某客客户端的状态

- 服务端在为客户端创建 session 时，会同时将 session 对象的 id，即 JSESSION 以 Cookie 的形式放入响应对象

- 后端创建完 Session 后，客户端会收到一个特殊的 Cookie，叫做 JSESSION

- 客户端下一次请求携带 JSESSIONID，后端收到后根据 JSESSIONID 找到对应的 Session 对象

- 通过该机制，服务端通过 Session 就可以存储一些专门针对某个客户端的信息了

> 原理图：

![Session原理图](img/Web_40.png)

应用场景有：记录用户的登录状态、记录用户操作的历史

#### HttpSession 的使用

> 用户提交 form 表单到 ServletA，携带用户名，ServletA 获取 Session 将用户名存到 Session，用户再请求其他任意 Servlet，获取之间存储的用户

- 定义 ServletA，将用户名存入 Session

```java
@WebServlet("/servletA")
public class ServletA extends HttpServlet {
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 获取请求中的参数
    String username = req.getParameter("username");
    // 获取Session对象
    HttpSession session = req.getSession();
    // 获取Session的ID
    String jSessionID = session.getId();
    // 判断Session是否是新创建的Session
    boolean isNew = session.isNew();
    // 向session对象中存入数据
    session.setAttribute("username",username);
  }
}
```

![getSession方法的处理逻辑](img/Web_41.png)

#### HttpSession 的时效性

- 用户量很大之后，Session 对象相应的也要创建很多，如果一味创建不释放，那么服务器端的内存迟早要被耗尽

- 客户端关闭行为无法被服务端直接侦测，或者客户端长时间不操作也经常出现，就需要设置 Session 的时效性了

> 默认的 Session 最大限制时间在 Tomcat/conf/web.xml 中默认为 30 分钟，如果需要更改可以在当前项目的 web.xml 中进行配置（单位为分钟）

```xml
<session-config>
  <session-timeout>60</session-timeout>
</session-config>
```

> 也可以通过 HttpSession 的 API 对最大闲置时间进行设置，同时此处时间单位为秒，表示当前 session 在指定时间内若没有与服务器发生任何交互后失效，期间用户的任何活动都将刷新 Session 的失效时间

```java
session.setMaxInactiveInterval(int seconds);
```

> 三种修改的优先级：**代码设置** > **项目 web.xml 设置** > **容器 web.xml 设置值**

> 也可以直接让 Session 失效

```java
session.invalidate();
```

### 三大域对象

#### 域对象概述

> 域对象：一些用于存储数据和传递数据的对象，传递数据不同的范围，我们称之为不同的域，不同的域对象代表不同的域，共享数据的范围也不同

- web 项目中需要熟练使用的域对象分别是，请求域、会话域、应用域

- 请求域对象是 HttpServletRequest，传递数据的范围是一次请之内以及请求转发

- 会话域对象是 HttpSession，传递数据的范围是一次会话之内，可以跨多个请求

- 应用域对象是 ServletContext， 传递数据的范围是本应用之内，可以跨多个会话

> 三大域对象的数据作用范围图解：

- 请求域

![请求域](img/Web_42.png)

- 会话域

![会话域](img/Web_43.png)

- 应用域

![应用域](img/Web_44.png)

综合三大域对象：

![三大域对象](img/Web_45.png)

#### 域对象的使用

> 域对象的 API

| API                                         | 功能                    |
| ------------------------------------------- | ----------------------- |
| void setAttribute(String name,String value) | 向域对象中添加/修改数据 |
| Object getAttribute(String name);           | 从域对象中获取数据      |
| removeAttribute(String name);               | 移除域对象中的数据      |

- 请求转发时，请求域可以传递数据，请求域内一般放本次请求业务有关的数据

- 同一个会化内，不用请求转发，会话域可以传递数据，会话域内一般放本次会话的客户端有关的数据

- 同一个 APP 内，不同的客户端，应用域可以传递数据，应用域内一般放本程序应用有关的数据

## 过滤器

### 过滤器概述

> Filter 即过滤器，是 JAVAEE 技术规范之一，作用目标资源的请求进行过滤的一套技术规范，是 Java Web 项目中最实用的技术之一

- Filter 接口定义了过滤器的开发规范，所有的过滤器都要实现该接口

- Filter 的工作位置是项目中所有目标资源之前，容器在创建 HttpServletRequest 和 HttpServletResponse 对象后，会先调用 Filter 的 doFilter 方法

- Filter 的 doFilter 方法可以控制请求是否继续，如果放行，则请求继续，如果拒绝，则请求到此为止，由过滤器本身作出相应

- Filter 不仅可以对请求做出过滤，也可以在目标资源做出响应前，对响应再次进行处理

- Filter 是 GOF 中责任链模式的典型案例

- Filter 的常用应用包括但不限于：登录权限检查，解决网站乱码，过滤敏感字符，日志记录，性能分析，事务控制

> 过滤器工作位置：

![过滤器工作位置](img/Web_46.png)

> Filter 接口 API：

| API                                                                                       | 目标                                                                     |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| default public void init(FilterConfig filterConfig)                                       | 初始化方法,由容器调用并传入初始配置信息 filterConfig 对象                |
| public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) | 过滤方法,核心方法,过滤请求,决定是否放行,响应之前的其他处理等都在该方法中 |
| default public void destroy()                                                             | 销毁方法,容器在回收过滤器对象之前调用的方法                              |

### 过滤器的使用

> 过滤流程图解

![过滤器流程图解](img/Web_47.png)

> 开发过滤器流程

- 实现 Filter 接口

- 重写过滤方法

- 配置过滤器的映射路径：

  - web.xml 中配置

  - 在注解中配置

> 过滤请求的和响应的方法

- 请求到达目标资源之前，先经过该方法

- 该方法有能力控制请求是否继续向后到达目标资源 可以在该方法内直接向客户端做相应处理

- 请求到达目标资源后，相应之前，还会经过该方法

```java
public class LoggingFilter implements Filter {
  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
    // 1.请求到达目标资源之前的功能代码：校验权限是否满足、判断是否登录

    // 2.放行代码
    filterChain.doFilter(request, response);

    // 3.响应之前 HttpServletResponse 转换为响应报文之前的功能代码
  }
}
```

- 说明

  - doFilter 方法中的请求和响应对象是以父接口的形式声明的，实际传入的实参就是 HttpServletRequest 和 HttpServletResponse 子接口级别的，可以安全强转

  - filterChain.doFilter(request, response) 这行代码的功能是放行请求，如果没有这行代码，则请求到此为止

  - filterChain.doFilter(request, response) 在放行时需要传入 request 和 response 对象，意味着请求和响应对象要继续传递给后续的资源，这里没有产生新的 request 和 reponse 对象

```xml
<filter>
  <filter-name>loggingFilter</filter-name>
  <filter-class>com.example.LoggingFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>loggingFilter</filter-name>
  <!--
  url-pattern 根据请求的资源路径，对指定的请求惊醒过滤
    /* 代表所有资源
    /a/* 代表以a开头的资源
    *.html 代表过滤以html为后缀的资源
    /servlet1 代表对制定的servlet进行过滤
  servlet-name 根据请求的servlet的别名，对制定的servlet进行过滤

  一个 filter-mapping 中，可以同时存在多个 url-pattern 和 servlet-name
  -->
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

- 说明

  - filter-mapping 标签中定义了过滤器对那些资源进行过滤

  - 子标签 url-pattern 通过映射路径确定过滤范围

    - /servletA 精确匹配,表示对 servletA 资源的请求进行过滤

    - \*.html 表示对以.action 结尾的路径进行过滤

    - /\* 表示对所有资源进行过滤

    - 一个 filter-mapping 下可以配置多个 url-pattern

  - 子标签 servlet-name 通过 servlet 别名确定对那些 servlet 进行过滤

    - 使用该标签确定目标资源的前提是 servlet 已经起了别名

    - 一个 filter-mapping 下可以定义多个 servlet-name

    - 一个 filter-mapping 下,servlet-name 和 url-pattern 子标签可以同时存在

### 过滤器的生命周期

> 过滤器作为 Web 项目的组件之一，和 Servlet 的生命周期类似，略有不同，没有 Servlet 的 load-on-startup 的配置，默认就是系统启动立刻构造

| 阶段       | 对应方法                                                                                               | 执行时机       | 执行次数 |
| ---------- | ------------------------------------------------------------------------------------------------------ | -------------- | -------- |
| 创建对象   | 构造器                                                                                                 | web 应用启动时 | 1        |
| 初始化方法 | void init(FilterConfig filterConfig)                                                                   | 构造完毕       | 1        |
| 过滤请求   | void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) | 每次请求       | 多次     |
| 销毁       | default void destroy()                                                                                 | web 应用关闭时 | 1 次     |

### 过滤器链的使用

> 一个 Web 项目中可以同时定义多个过滤器，多个过滤器对同一个资源进行过滤时，工作位置有先后，整体形成一个工作链，称之为过滤连

- 过滤器链中的过滤器的顺序由 filter-mapping 顺序决定

- 每个过滤器过滤的范围不同，针对同一个资源来说，过滤器链中的过滤器个数可能是不同的

- 如果某个 Filter 是使用 ServletName 进行匹配规则的配置，那么这个 Filter 执行的优先级要更低

> 图解过滤器链

![图解过滤器链](img/Web_48.png)

> 工作流程图解

![工作流程图解](img/Web_49.png)

### 注解方式配置过滤器

> @WebFilter 注解的源码

```java
package jakarta.servlet.annotation;

import jakarta.servlet.DispatcherType;
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebFilter {
    String description() default "";

    String displayName() default "";

    WebInitParam[] initParams() default {};

    String filterName() default "";

    String smallIcon() default "";

    String largeIcon() default "";

    String[] servletNames() default {};

    String[] value() default {};

    String[] urlPatterns() default {};

    DispatcherType[] dispatcherTypes() default {DispatcherType.REQUEST};

    boolean asyncSupported() default false;
}
```

## 监听器

### 监听器概述

> 监听器：专门用于对域对象身上发生的事件或状态改变进行监听和相应处理的对象

- 监听器是 GOF 设计模式中，观察者模式的典型案例

- 观察者模式：当被观察的对象发生某些改变时，观察者自动采取对应行动的一种设计模式

- 监听器使用的感受类似 JS 中的事件，被观察的对象发生某些情况时吗，自动触发代码的执行

- 监听器并不监听 Web 项目中的所有组件，仅仅是对三大域对象做相关的事件监听

> 监听器的分类

- Web 中定义八个监听器接口作为监听器的规范，这八个接口按照不同的标准可以形成不同的分类

- 按监听的对象划分

  - application 域监听器 ServletContextListener ServletContextAttributeListener

  - session 域监听器 HttpSessionListener HttpSessionAttributeListener HttpSessionBindingListener HttpSessionActivationListener

  - request 域监听器 ServletRequestListener ServletRequestAttributeListener

- 按监听的事件划分

  - 域对象的创建和销毁监听器 ServletContextListener HttpSessionListener ServletRequestListener

  - 域对象数据增删改事件监听器 ServletContextAttributeListener HttpSessionAttributeListener ServletRequestAttributeListener

  - 其他监听器 HttpSessionBindingListener HttpSessionActivationListener

> 配置监听器

1. 在 web.xml 中配置监听器

```xml
<listener>
  <listener-class>com.example.MyListener</listener-class>
</listener>
```

2. 在注解中配置监听器，使用 `@WebListener` 注解

### 监听器的六个主要接口

#### application 域监听器

> ServletContextListener 监听 ServletContext 对象创建和销毁

| 方法名                                      | 作用                      |
| ------------------------------------------- | ------------------------- |
| contextInitialized(ServletContextEvent sce) | ServletContext 创建时调用 |
| contextDestroyed(ServletContextEvent sce)   | ServletContext 销毁时调用 |

- ServletContextEvent 对象代表从 ServletContext 对象上捕获到的事件，通过这个事件对象我们可以获取到 ServletContext 对象

> ServletContextAttributeListener 监听 ServletContext 中属性的添加、移除和修改

| 方法名                                               | 作用                                   |
| ---------------------------------------------------- | -------------------------------------- |
| attributeAdded(ServletContextAttributeEvent scab)    | 向 ServletContext 中添加属性时调用     |
| attributeRemoved(ServletContextAttributeEvent scab)  | 从 ServletContext 中移除属性时调用     |
| attributeReplaced(ServletContextAttributeEvent scab) | 当 ServletContext 中的属性被修改时调用 |

- ServletContextAttributeEvent 对象代表属性变化事件，包含的方法如下：

| 方法名              | 作用                     |
| ------------------- | ------------------------ |
| getName()           | 获取修改或添加的属性名   |
| getValue()          | 获取被修改或添加的属性值 |
| getServletContext() | 获取 ServletContext 对象 |

#### session 域监听器

> HttpSessionListener 监听 HttpSession 对象的创建与销毁

| 方法名                                 | 作用                       |
| -------------------------------------- | -------------------------- |
| sessionCreated(HttpSessionEvent hse)   | HttpSession 对象创建时调用 |
| sessionDestroyed(HttpSessionEvent hse) | HttpSession 对象销毁时调用 |

- HttpSessionEvent 对象代表从 HttpSession 对象身上捕获到的事件，通过这个事件对象可以获取到触发事件的 HttpSession 对象。

> HttpSessionAttributeListener 监听 HttpSession 中属性的添加、移除和修改

| 方法名                                        | 作用                                |
| --------------------------------------------- | ----------------------------------- |
| attributeAdded(HttpSessionBindingEvent se)    | 向 HttpSession 中添加属性时调用     |
| attributeRemoved(HttpSessionBindingEvent se)  | 从 HttpSession 中移除属性时调用     |
| attributeReplaced(HttpSessionBindingEvent se) | 当 HttpSession 中的属性被修改时调用 |

- HttpSessionBindingEvent 对象代表属性变化事件，它包含的方法如下：

| 方法名       | 作用                            |
| ------------ | ------------------------------- |
| getName()    | 获取修改或添加的属性名          |
| getValue()   | 获取被修改或添加的属性值        |
| getSession() | 获取触发事件的 HttpSession 对象 |

#### request 域监听器

> ServletRequestListener 监听 ServletRequest 对象的创建与销毁

| 方法名                                      | 作用                          |
| ------------------------------------------- | ----------------------------- |
| requestInitialized(ServletRequestEvent sre) | ServletRequest 对象创建时调用 |
| requestDestroyed(ServletRequestEvent sre)   | ServletRequest 对象销毁时调用 |

- ServletRequestEvent 对象代表从 HttpServletRequest 对象身上捕获到的事件，通过这个事件对象我们可以获取到触发事件的 HttpServletRequest 对象。另外还有一个方法可以获取到当前 Web 应用的 ServletContext 对象。

> ServletRequestAttributeListener 监听 ServletRequest 中属性的添加、移除和修改

| 方法名                                               | 作用                                   |
| ---------------------------------------------------- | -------------------------------------- |
| attributeAdded(ServletRequestAttributeEvent srae)    | 向 ServletRequest 中添加属性时调用     |
| attributeRemoved(ServletRequestAttributeEvent srae)  | 从 ServletRequest 中移除属性时调用     |
| attributeReplaced(ServletRequestAttributeEvent srae) | 当 ServletRequest 中的属性被修改时调用 |

- ServletRequestAttributeEvent 对象代表属性变化事件，它包含的方法如下：

| 方法名               | 作用                               |
| -------------------- | ---------------------------------- |
| getName()            | 获取修改或添加的属性名             |
| getValue()           | 获取被修改或添加的属性值           |
| getServletRequest () | 获取触发事件的 ServletRequest 对象 |

#### session 域的两个特殊监听器

##### session 绑定监听器

> HttpSessionBindingListener 监听当前监听器对象在 Session 域中的增加与移除

| 方法名                                      | 作用                                |
| ------------------------------------------- | ----------------------------------- |
| valueBound(HttpSessionBindingEvent event)   | 该类的实例被放到 Session 域中时调用 |
| valueUnbound(HttpSessionBindingEvent event) | 该类的实例从 Session 中移除时调用   |

- HttpSessionBindingEvent 对象代表属性变化事件，它包含的方法如下：

| 方法名       | 作用                            |
| ------------ | ------------------------------- |
| getName()    | 获取当前事件涉及的属性名        |
| getValue()   | 获取当前事件涉及的属性值        |
| getSession() | 获取触发事件的 HttpSession 对象 |

##### 钝化活化监听器

> HttpSessionActivationListener 监听某个对象在 Session 中的序列化与反序列化。

| 方法名                                    | 作用                                    |
| ----------------------------------------- | --------------------------------------- |
| sessionWillPassivate(HttpSessionEvent se) | 该类实例和 Session 一起钝化到硬盘时调用 |
| sessionDidActivate(HttpSessionEvent se)   | 该类实例和 Session 一起活化到内存时调用 |

- HttpSessionEvent 对象代表事件对象，通过 getSession()方法获取事件涉及的 HttpSession 对象。

> 什么是钝化活化

- session 对象在服务端是以对象的形式存储于内存的,session 过多,服务器的内存也是吃不消的

- 而且一旦服务器发生重启,所有的 session 对象都将被清除,也就意味着 session 中存储的不同客户端的登录状态丢失

- 为了分摊内存 压力并且为了保证 session 重启不丢失,我们可以设置将 session 进行钝化处理

- 在关闭服务器前或者到达了设定时间时,对 session 进行序列化到磁盘,这种情况叫做 session 的钝化

- 在服务器启动后或者再次获取某个 session 时,将磁盘上的 session 进行反序列化到内存,这种情况叫做 session 的活化

> 怎么配置钝化活化

- 在 web 目录下,添加 META-INF 下创建 Context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1">
        <Store className="org.apache.catalina.session.FileStore" directory="d:\mysession"></Store>
    </Manager>
</Context>
```

## Ajax

> 什么是 Ajax

- AJAX = Asynchronous JavaScript and XML （异步的 JavaScript 和 XML）

- AJAX 不是新的编程语言，而是一种使用现有标准的新方法

- AJAX 最大的优点实在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容

- AJAX 不需要任何浏览器插件，但需要用户允许 JavaScript 在浏览器上执行

- XMLHttpRequest 只是实现 Ajax 的一种方式

**Ajax 的工作原理**

![Ajax的工作原理](img/Web_50.png)

- 简单来说，之前发的请求通过类似 form 表单标签、 a 标签 这种方式，现在通过运行 js 代码动态决定什么时候发送什么样的请求

- 通过运行 JS 代码发送的请求，浏览器可以不用跳转页面，我们可以在 JS 代码中决定是否要跳转页面

- 通过运行 JS 代码发送的请求，接收到返回结果后，我们可以将结果通过 DOM 变成渲染到页面的某些元素上，实现局部更新

## 前端工程化

> 前端工程化的概念

**前端工程化** 是使用 **软件工程的方法** 来单独解决前端的开发流程中 模块化、组件化、规范化、自动化 的问题，其主要目的是为了提高效率和降低成本

> 前端工程化实现技术栈

- ECMAScript6 Vue3 中大量使用 ES6 语法

- Nodejs 前端项目运行环境

- npm 依赖下载工具

- vite 前端项目构建工具

- VUE3 渐进式前端框架

- router 通过路由实现页面切换

- pinia 通过状态管理实现组件数据传递

- axios Ajax 异步请求封装技术实现前后端数据交互

- Element-Plus 丰富的快速构建网页的组件仓库

## ECMAScript6

### ECMAScript6 简介

> ECMAScript 6 ，简称 ES6，是 **JavaScript** 语言的一次重大更新。
> ES6 带来了大量的新特性，包括箭头函数、模板字符串、let 和 const 关键字、解构、默认参数值、模块系统等等，大大提升了 JS 的开发体验。由于 VUE3 中大量使用了 ES6 的语法，所以 ES6 也是 VUE3 的基础。

- ES6 对 JS 的改进在以下几个方面：

1. 更加简洁：ES6 引入了一些新的语法，如箭头函数、类和模板字符串等，使代码更加简洁易懂

2. 更强大的功能：ES6 引入一些新的 API、解构语法和迭代器等功能，从而使得 JS 更加强大

3. 更好的适用性：ES6 引入的模块化功能为 JS 代码的组织和管理提供了更好的方式，不仅提高了程序的可维护性，还让 JS 更方便地应用于大型的应用程序

> ES6 新增了 let 和 const，用来声明变量，使用的细节上也存在诸多差异

#### let 和 var 的差别

1. let 不能重复声明

2. let 有块级作用域，非函数的花括号遇见 let 会有块级作用域，也就是只能在花括号里面访问

3. let 不会预解析进行变量提升

4. let 定义的全局变量不会作为 window 的属性

5. let 在 ES6 中推荐优先使用

```javascript
// 1. let 只有在当前代码块有效代码块，代码块、函数、全局
{
  let a = 1;
  var b = 2;
}
console.log(a); // a is not defined 花括号外面无法访问
console.log(b); // 可以正常输出

// 2. 不能重复声明
{
let name = '天真'
let name = '无邪'
}

// 3. 不存在变量提升（先声明再使用）
{
  console.log(test1); // 可以，但是值为undefined
  var test1 = 'test'
  console.log(test2); // 不可以 let 命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错
  let test2 = 'test'
}

// 4. 不会成为 window 的属性
{
  var a = 100;
  console.log(window.a) // 100
  let b = 200;
  console.log(window.b) // undefined
}

// 5. 循环中推荐使用 let
{
  for (let i = 0; i < 10; i++) {
    ...
  }
}
```

#### const 和 var 的差异

1. 新增 const 和 let 类似，只是 const 定义的变量不能修改

2. 并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动

```javascript
// 声明场景语法，建议变量名大写区分
const PI = 3.1415926;

// 1. 常量声明必须有初始化值
const A; // 报错

// 2. 常量的值不能修改
const A = "hello"
A = "world" // 报错

// 3. 和let一样，块作用域

  const A = "1"
  console.log(A) // 1
}
console.log(A) // 报错

// 4. 对应数组和对象元素修改，不算常量修改，修改值、不修改地址
const TEAM = ['1','2','3']
TEAM.push('4')
TEAM = [] // 报错
console.log(TEAM) // ['1', '2', '3', '4']
```

#### 模板字符串

模板字符串是增强版的字符串，用反引号 (`) 标识

1.  字符串中可以出现换行符

2.  可以使用 `${xxx}` 形式输出和拼接变量

```javascript
// 1. 多行普通字符串
let ulStr1 =
  `<ul>` + "<li>JAVA</li>" + "<li>html</li>" + "<li>VUE</li>" + "</ul>";
console.log(ulStr1);

// 2. 多行模板字符串
let ulStr2 = `
        <ul>
        	<li>JAVA</li>
        	<li>html</li>
        	<li>VUE</li>
        </ul>`;
console.log(ulStr2);

// 3. 普通字符串拼接
let str = "ABC";
let infoStr1 = str + "DEF";
console.log(infoStr1); // "ABCDEF"

// 4. 模板字符串拼接
let infoStr2 = `${str}DEF`;
console.log(infoStr2); // "ABCDEF"
```

#### 解构表达式

> ES6 的解构表达式是一种方便的语法，可以快速将数组或对象中的值拆分并赋值给变量。解构赋值的语法使用花括号表示对象，方括号表示数组。通过解构赋值，函数更方便进行参数接收

##### 数组解构赋值

- 可以通过数组解构将数组中的值赋值给变量

```javascript
let [a, b, c] = [1, 2, 3]; // 新增变量名任意合法即可，本质是按照顺序进行初始化变量的值
console.log(a, b, c); // 1 2 3
```

- 该语句将数组`[1,2,3]`中的每个值依次赋值给第一个变量`a`，第二个变量`b`，第三个变量`c`，可以使用默认值为变量提供备选值，在数组中缺失对应位置的值时使用该默认值

```javascript
let [a, b, c = 4] = [1, 2];
console.log(a, b, c); // 1 2 4
```

##### 对象解构赋值

- 可以通过对象解构将对象中的值赋值给变量

```javascript
let { a, b } = { a: 1, b: 2 };
// 新增变量名必须和属性名相同，本质是初始化变量的值为对象中的同名属性的值
// 等价于 let a = 对象.a， let b = 对象.b

console.log(a); // 1
console.log(b); // 2
```

- 该语句将对象 `{a:1, b:2}` 中的 `a` 和 `b` 属性的值分别赋值给变量 `a` 和 `b`，可以为标识符分配不同的变量名称，使用 `:` 操作符制定新的变量名

```javascript
let { a: x, b: y } = { a: 1, b: 2 };
console.log(x); // 1
console.log(y); // 2
```

##### 函数参数解构赋值

- 解构赋值也可以用于函数参数

```javascript
function add([x, y]) {
  return x + y;
}
add([1, 2]); // 3
```

- 该函数接收一个数组作为参数，将其中的第一个值赋给 x， 第二个值赋给 y，然后返回它们的和

- ES6 解构赋值让变量的初始化更加简单和便捷，我们可以访问到对象中的属性，并将其赋值给对应的变量，从而提高代码的可读性和可维护性

#### 箭头函数

> ES6 允许使用箭头定义函数，语法类 Java 的 Lambda 表达式

```javascript
//ES6 允许使用箭头定义函数

// 1. 函数声明
let fn1 = function () {};
let fn2 = () => {}; // 箭头函数，此处不需要书写function关键字
let fn3 = (x) => {}; // 单参数可以省略()，多参数无参数不可以
let fn4 = (x) => console.log(x); // 只有一行方法体可以省略{}
let fn5 = (x) => x + 1; // 当函数体只有依据返回值，可以省略花括号和return语句

// 2. 使用特点
// 在 JavaScript 中，this 关键字通常用来引用函数所在的对象，
// 或者在函数本身作为构造函数时，来引用新对象的实例。
// 但是在箭头函数中，this 的含义与常规函数定义中的含义不同，
// 并且是由箭头函数定义时的上下文来决定的，而不是由函数调用时的上下文来决定的。
// 箭头函数没有自己的this，this指向的是外层上下文环境的this

let person = {
  name: "张三",
  showName: function () {
    console.log(this); //  这里的this是person
    console.log(this.name);
  },
  viewName: () => {
    console.log(this); //  这里的this是window，和person同级的作用域
    console.log(this.name);
  },
};
person.showName();
person.viewName();

//this应用
function Counter() {
  this.count = 0;
  setInterval(() => {
    // 这里的 this 是上一层作用域中的 this，即 Counter实例化对象
    this.count++;
    console.log(this.count);
  }, 1000);
}
let counter = new Counter();
```

#### rest 和 spread

> rest 参数，在形参上使用和 Java 中的可变参数几乎一样

```javascript
// 1. 参数列表中多个普通参数 普通函数和箭头函数中都支持
let fun1 = function (a, b, c, d = 10) {
  console.log(a, b, c, d);
};
let fun2 = (a, b, c, d = 10) => console.log(a, b, c, d);

fun1(1, 2, 3); // 1 2 3 4
fun2(1, 2, 3, 4); // 1 2 3 4

// 2. ...作为参数列表，称之为 rest 参数 普通函数和箭头函数中都支持，因为箭头函数中无法使用 arguments 对象
let fun3 = function (...args) {
  console.log(args);
};
let fun4 = (...args) => console.log(args);

fun3(1, 2, 3); // [1, 2, 3]
fun4(1, 2, 3); // [1, 2, 3]

// rest参数必须在一个参数列表中的最后一位，并且要求不能有默认值，这也无形要求一个参数列表中只能有一个rest参数
```

> spread 参数，在实参上使用 rest

```javascript
let arr = [1, 2, 3];
// let arrSpread = ...arr; // 错误，...arr 必须在调用方法时作为形参使用
let fun1 = (a, b, c) => console.log(a, b, c);

// 调用方法时，对arr进行转换，转换为1，2，3
fun1(...arr); // 1 2 3

// 应用场景1 合并数组
let arr2 = [4, 5, 6];
let arr3 = [...arr, ...arr2];
console.log(arr3); // [1, 2, 3, 4, 5, 6]

// 应用场景2 合并对象属性
let p1 = { name: "张三" };
let p2 = { age: 20 };
let p3 = { ...p1, ...p2 };
console.log(p3); // {name: "张三", age: 20}
```

#### ES6 的对象创建和拷贝

##### 对象创建的语法糖

> ES6 中新增了对象创建的语法糖，支持了 class extends constructor 等关键字，让 ES6 的语法和面向对象的语法更加接近

```javascript
class Person {
  // 属性，带 # 符号说明该属性私有
  #name;
  #age;

  // 构造函数，带 constructor 关键字
  constructor(name, age) {
    this.#name = name;
    this.#age = age;
  }

  // getter 方法，带 get 关键字
  get name() {
    return this.#name;
  }

  get age() {
    return this.#age;
  }

  // setter 方法，带 set 关键字
  set name(name) {
    this.#name = name;
  }

  set age(age) {
    this.#age = age;
  }

  // 实例方法，不带关键词
  eat(food) {
    console.log(`${this.#age}岁的${this.#name} 正在吃 ${food}`);
  }

  // 静态方法，带 static 关键字
  static sum(a, b) {
    return a + b;
  }
}

// 类的继承
class Student extends Person {
  #grade;
  #score;

  constructor(name, age, grade, score) {
    // 调用父类的构造函数
    super(name, age);
    this.#grade = grade;
    this.#score = score;
  }

  study(grade, score) {
    console.log(
      `${this.#age}岁的${this.#name} 正在学习 ${grade} 年级，成绩为 ${score}`
    );
  }
}
```

##### 对象的深拷贝和浅拷贝

> 对象的拷贝，快速获得一个和已有对象相同的对象的方式

- 浅拷贝

```javascript
let arr = ["java", "c", "python"];
let person = {
  name: "张三",
  language: arr,
};

// 浅拷贝
let person2 = person;
person2.name = "李四";
console.log(person.name); // 李四
```

- 深拷贝

```javascript
let arr = ["java", "c", "python"];
let person = {
  name: "张三",
  language: arr,
};

// 深拷贝
let person2 = JSON.parse(JSON.stringify(person));
person2.name = "李四";
console.log(person.name); // 张三
console.log(person2.name); // 李四
```

#### ES6 的模块化处理

> 模块化是一种组织和管理前端代码的方式，将代码拆分成小的模块单元，使得代码更易于维护、拓展和复用。包括了定义、导出、导入以及管理模块的方法和规范。

前端模块化的主要优势如下：

1. 提高代码可维护性：通过将代码拆分丞小的模块单元，使得代码结构更为清晰，可读性更高，便于开发者阅读和维护

2. 提高代码可复用性：通过将重复使用的代码编程可复用的模块，减少代码重复率，降低开发成本

3. 提高代码可拓展性：通过模块化来实现代码的松耦合，便于更改和替换模块，从而方便地拓展功能

> 目前前端模块化有多种规范和实现，包括 CommonJS、AMD 和 ES6 模块化。ES6 木块还是 JavaScript 语言的木块标准，使用 import 和 export 来实现模块的导入和导出

#### ES6 模块化的几种暴露和导入方式

- 分别导出

- 统一导出

- 默认导出

ES6 种无论以何种方式导出，导出的都是一个对象，导出的内容都可以理解为是向这个对象中添加属性或者方法

##### 分别导出

`module.js` 向外分别暴露成员

```javascript
// 1. 分别暴露
// 模块想对外导出，添加export关键字即可

export const PI = 3.14;

expeort function sum(x, y){
  return x + y;
}

export class Person {
  constructor(name, age){
    this.name = name;
    this.age = age;
  }
  sayHello(){
    console.log(`hello, my name is ${this.name}. I am ${this.age} years old.`);
  }
}
```

`app.js` 导入 `module.js` 的成员

```javascript
/*
 * `*` 代表module.js中的所有成员
 * m1代表所有成员所属的对象
 */
import * as m1 from "./module.js";
// 使用暴露的属性
console.log(m1.PI); // 3.14

// 使用暴露的函数
console.log(m1.sum(1, 2)); // 3

// 使用暴露的类
let p = new m1.Person("张三", 20);
p.sayHello(); // hello, my name is 张三. I am 20 years old.
```

`index.html`作为程序启动的入口 导入 `app.js`，需要添加 `type="module"` 属性，否则不支持 ES6 的模块化语法

```html
<script src="./app.js" type="module"></script>
```

##### 统一导出

`module.js` 向外统一暴露成员

```javascript
// 2. 统一暴露
// 模块想对外导出，export统一暴露想暴露的内容

const PI = 3.14;

function sum(x, y) {
  return x + y;
}

class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`hello, my name is ${this.name}. I am ${this.age} years old.`);
  }
}

// 统一暴露
export { PI, sum, Person };
```

`app.js` 导入 `module.js` 的成员

```javascript
/* 
  {}中导入要使用的来自于module.js中的成员
  {}中导入的名称要和module.js中导出的一致,也可以在此处起别名
  {}中如果定义了别名,那么在当前模块中就只能使用别名
  {}中导入成员的顺序可以不是暴露的顺序
  一个模块中可以同时有多个import
  多个import可以导入多个不同的模块,也可以是同一个模块
*/
//import {PI ,Person ,sum }  from './module.js'
//import {PI as pi,Person as People,sum as add}  from './module.js'

import {
  PI,
  Person,
  sum,
  PI as pi,
  Person as People,
  sum as add,
} from "./module.js";

// 使用暴露的属性
console.log(PI); // 3.14
console.log(pi); // 3.14

// 使用暴露的函数
let result = add(1, 2);
console.log(result); // 3

// 使用暴露的类
let p = new People("张三", 20);
p.sayHello(); // hello, my name is 张三. I am 20 years old.
```

##### 默认和混合暴露

`module.js` 混合向外到处

```javascript
// 3. 默认和混合暴露
// 默认暴露语法 export default
// 默认暴露相当于实在暴露的对象中增加了一个名字为default的属性
// 三种暴露方式可以在一个module中混合使用

export const PI = 3.14;

function sum(x, y) {
  return x + y;
}

class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`hello, my name is ${this.name}. I am ${this.age} years old.`);
  }
}

// 导出默认
export default sum;

// 统一导出
export { PI, Person };
```

`app.js` 的 default 和其他导入写法混用

```javascript
/* 
  * 代表module.js中的所有成员
  m1 代表所有成员所属的对象
 */

import * as m1 from "./module.js";
import { default as add } from "./module.js";
import add2 from "./module.js"; // import {default as add2} from './module.js'

// 调用暴露的方法
let result = m1.default(1, 2);
console.log(result); // 3
// 引入其他方式暴露的内容
import { PI, Person } from "./module.js";

// 使用暴露的属性
console.log(PI); // 3.14
```

## 前端工程化

### Nodejs

> Nodejs 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以使 JavaScript 运行在服务器端。使用 Node.js 可以方便地开发服务器端应用程序，如 Web 应用、API、后端服务，还可以通过 Node.js 构建命令行工具等。相比于传统的服务器端语言，Node.js 具有以下特点：

- 单线程，但是采用了事件驱动、异步 I/O 模型，可以处理高并发请求

- 轻量级，使用 C++ 编写的 V8 引擎让 Node.js 的运行速度快

- 模块化，Node.js 内置了大量模块，同时也可以通过第三方扩展功能

- 跨平台，可以在 Windows, Linux, Mac 等多种平台运行

### npm 常用命令

> 1. 项目初始化

- `npm init`

- `npm init -y`

> 2. 安装依赖

- `npm install <package><@version>`

  - 安装包或者指定版本的依赖包

- `npm install -g <package>`

  - 安装全局依赖包

- `npm install`

  - 安装 package.json 中的所有记录的依赖

> 3. 升级依赖

- `npm update <package>`

  - 将依赖升级到最新版本

> 4. 卸载依赖

- `npm uninstall <package>`

> 5. 查看依赖

- `npm ls`

  - 查看项目依赖

- `npm ls -g`

  - 查看全局依赖

> 6. 运行命令

- `npm run`

### Vue3

**Vue 的两个核心功能：**

- **声明式渲染**：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系。

- **响应性**：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM

### Vite

- 当我们开始构建越来越大型的应用时，需要处理的 JavaScript 代码量也呈指数级增长。

- 包含数千个模块的大型项目相当普遍。基于 JavaScript 开发的工具就会开始遇到性能瓶颈：通常需要很长时间（甚至是几分钟！）才能启动开发服务器，即使使用模块热替换（HMR），文件修改后的效果也需要几秒钟才能在浏览器中反映出来。如此循环往复，迟钝的反馈会极大地影响开发者的开发效率和幸福感。

1.  快速创建项目：使用脚手架可以快速搭建项目基本框架，避免从零开始搭建项目的重复劳动和繁琐操作，从而节省时间和精力。

2.  统一的工程化规范：前端脚手架可以预设项目目录结构、代码规范、git 提交规范等统一的工程化规范，让不同开发者在同一个项目上编写出风格一致的代码，提高协作效率和质量。

3.  代码模板和组件库：前端脚手架可以包含一些常用的代码模板和组件库，使开发者在实现常见功能时不再重复造轮子，避免因为轮子质量不高带来的麻烦，能够更加专注于项目的业务逻辑。

4.  自动化构建和部署：前端脚手架可以自动进行代码打包、压缩、合并、编译等常见的构建工作，可以通过集成自动化部署脚本，自动将代码部署到测试、生产环境等。

#### Vite + Vue3 项目的目录结构

- public/ 目录：用于存放一些公共资源，如 HTML 文件、图像、字体等，这些资源会被直接复制到构建出的目标目录中

- src/ 目录：存放项目的源代码，包括 JavaScript、CSS、Vue 组件、图像和字体等资源。在开发过程中，这些文件会被 Vite 实时编译和处理，并在浏览器中进行实时预览和调试

  - assets/ 目录：用于存放一些项目中用到的静态资源，如图片、字体、样式文件

  - components/ 目录：用于存放组件相关的文件。组件是代码复用的一种方式，用于抽象出一个可复用的 UI 部件，方便在不同的场景中进行重复使用

  - layouts/ 目录：用于存放布局组件的文件。布局组件通常负责整个应用程序的整体布局，如头部、底部、导航菜单等

  - pages/ 目录：用于存放页面级别的组件文件，通常是路由对应的组件文件。在这个目录下，可以创建对应的文件夹，用于存储不同的页面组件

  - plugins/ 目录：用于存放 Vite 插件相关的文件，可以按需加载不同的插件来实现不同的功能，如自动化测试、代码压缩

  - router/ 目录：用于存放 Vue.js 的路由配置文件，负责管理视图和 URL 之间的映射关系，方便实现页面之间的跳转和数据传递

  - store/ 目录：用于存放 Vuex 状态管理相关的文件，负责管理应用程序中的数据和状态，方便统一管理和共享数据，提高开发效率

  - utils/ 目录：用于存放一些通用的工具函数，如日期处理函数、字符串操作函数

- vite.config.js 文件：Vite 的配置文件，可以通过该文件配置项目的参数、插件、打包优化等，该文件可以使用 CommonJs 或 ES6 模块的语法进行配置

- package.json 文件：标准的 Node.js 项目配置文件，包含了项目的基本信息和依赖关系。其中可以通过 scripts 字段定义几个命令，如 dev、build、serve 等，用于启动开发、构建和启动本地服务器等操作

- Vite 项目的入口为 src/main.js 文件，这是 Vue.js 应用程序的启动文件，也是整个前端应用程序的入口文件。在该文件中，通常会引入 Vue.js 及其相关插件和组件，同时会创建 Vue 实例，挂载到 HTML 页面上指定的 DOM 元素中

#### Vite + Vue3 项目组件

- 一个页面作为整体，是由多个部分组成，每个部分在这里就可以理解为一个组件

- 每个.vue 文件就可以理解成一个组件，多个.vue 文件可以构成一个整体方面

- 组件化给我们带来的另一个好处就是组件的复用和维护非常方便

![组件方式](img/Web_51.png)

> .vue 文件

- 传统的页面有 .html 文件 .css 文件 和.js 文件三个文件组成（多文件组件）

- vue 将这文件合并成一个.vue 文件（Single-File Component，SFC，单文件组件）

- .vue 文件对 js/css/html 统一封装，这是 VUE 中的概念，该文件由三个部分组成

  - `<template>`标签：代表组件的 html 部分代码，代替传统的.html 文件

  - `<script>`标签：代表组件的 js 代码，代替传统的.js 文件

  - `<style>`标签：代表组件的 css 代码，代替传统的.css 文件

> vue 项目组织结构

- index.html：项目的入口，其中`<div id="app"></div>`是用于挂载所有组件的元素

- index.html 中的 script 标签引入了一个 main.js 文件,具体的挂载过程在 main.js 中执行

- main.js 是 vue 工程中非常重要的文件,他决定这项目使用哪些依赖,导入的第一个组件

- App.vue 是 vue 中的核心组件,所有的其他组件都要通过该组件进行导入,该组件通过路由可以控制页面的切换

![Vue文件](img/Web_52.png)

#### Vue3 响应式数据入门

`<style scoped>`：用于设置组件样式，含义是将样式局限在当前组件中，不对全局样式造成影响

> 响应式数据

// 非响应式数据：修改后 Vue 不会更新 DOM
// 响应式数据：修改后 Vue 会更新 DOM

// VUE3 中数据要经过 ref 或者 reactive 处理后才是响应式的
// ref 是 Vue3 框架中提供的一个函数

// ref 处理的响应式数据在 js 编码修改的时候需要通过.value 进行取值
// ref 响应式数据在绑定到 html 上时不需要.value

```vue
<script type="module">
import { ref } from "vue";
export default {
  setup() {
    let count = ref(0);
    function increase() {
      count.value++;
    }
    function decrease() {
      count.value--;
    }
    return {
      count,
      increase,
      decrease,
    };
  },
};
</script>
```

> setup 函数和语法糖

```vue
<script type="module" set>
/* 通过setup关键字可以省略冗余的语法结构 */
</script>
```

1. 全局引入 main.js

```javascript
import "./style/reset.css"; //书写引入的资源的相对路径即可！
```

2. vue 文件 script 代码引入

```javascript
import "./style/reset.css";
```

3. Vue 文件 style 代码引入

```javascript
@import './style/reset.css'
```

#### Vue3 视图渲染技术

##### 模版语法

> Vue 使用一种基于 HTML 的模板语法，能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。在底层机制中，Vue 会将模板编译成高度优化的 JavaScript 代码。结合响应式系统，当应用状态变更时，Vue 能够智能地推导出需要重新渲染的组件的最少数量，并应用最少的 DOM 操作。

###### 插值表达式和文本渲染

插值表达式：最基本的数据绑定形式是文本插值，它使用的是"Mustache"语法（双大括号）：

- 插值表达式是将数据渲染到元素的指定位置手段之一

- 插值表达式不绝对依赖标签，其位置相对自由

- 插值表达式中支持 JavaScript 的运算表达式

- 插值表达式中也支持函数的调用

> 为了渲染双标中的文本，也可以使用 v-exte 和 v-html 命令

- `v-***`这种写法的方式使用的是 Vue 命令

- `v-***`的命令必须依赖元素，并且要写在元素的开始标签中

- `v-***`指令支持 ES6 中的字符串模版

- `v-text`可以将数据渲染成双标签中的文本，但是不识别 html 元素结构的文本

- `v-html`可以将数据渲染成双标签中间的文本，识别 html 元素结构的文本

###### Attribute 属性渲染

想要渲染一个元素的 attribute，应该使用 `v-bind` 指令

- 由于插值表达式不能直接放在标签的属性中，所有要渲染元素属性就应该使用`v-bind`

- `v-bind`可以用于渲染任何元素的属性，语法为 `v-bind:属性名='数据名'`

###### 事件的绑定

使用`v-on`来监听 DOM 事件，并在事件触发时执行对应的 Vue 的 JavaScript 代码

- 用法：`v-on:事件名='handler'` 或简写为 `@事件名='handler'`

- vue 中的事件名=原生事件名去掉 `on` 前缀 如：onClick --> click

- handler 的值可以使方法事件处理器，也可以是内联事件处理器

- 绑定事件的时候，可以通过一些绑定的修饰符：

  - `.once`：只触发一次事件

  - `.prevent`：阻止默认事件

  - `.stop`：阻止事件冒泡

  - `.capture`：使用事件捕获模式而不是冒泡模式

  - `.self`：只在事件发送者自身触发时才触发事件

##### 响应式基础

数据模型发生变化时，自动更新 DOM 树内容，页面上显示的内容会进行同步变化，Vue3 的数据模型不是自动响应式的，需要我们进行特殊处理

响应式实现关键字

`ref` 可以将一个基本类型的数据（字符串、数字等）转换为一个响应式对象，`ref`只能包裹单一元素

`reactive()` 函数可以创建一个响应式对象或数组

- 使用 `ref` 适用于以下开发场景：

  - 包装基本类型数据：`ref` 主要用于包装基本类型数据（如字符串、数字等），即只有一个值的数据，如果想监听这个值的变化，用 `ref` 最为方便。在组件中使用时也很常见。

  - 访问方式简单：`ref` 对象在访问时与普通的基本类型值没有太大区别，只需要通过 `.value` 访问其实际值即可。

- 使用 `reactive` 适用于以下开发场景：

  - 包装复杂对象：`reactive` 可以将一个普通对象转化为响应式对象，这样在数据变化时会自动更新界面，特别适用于处理复杂对象或者数据结构。

  - 需要递归监听的属性：使用 `reactive` 可以递归追踪所有响应式对象内部的变化，从而保证界面的自动更新。

综上所述，`ref` 适用与简单情形下的数据双向绑定，对于只有一个字符等基本类型数据或自定义组件等情况，建议可以使用 `ref`；而对于对象、函数等较为复杂的数据结构，以及需要递归监听的属性变化，建议使用 `reactive`。当然，在实际项目中根据需求灵活选择也是十分必要的。

##### 条件渲染

> v-if

- `v-if='表达式'`只会在指令的表达式返回真值时才被渲染

- 可以使用 `v-else-if` 和 `v-else` 来实现条件渲染，必须跟在 `v-if` 后面

> v-show

- `v-show`会在 DOM 渲染中保留该元素，仅切换了 `display` 的 CSS 属性

- `v-show`不支持在`<template>`元素上使用，也不能搭配 `v-else`

##### 列表渲染

> v-for

- `v-for='item in items'` 用于便利源数据数组，而 `item` 是迭代项的**别名**

- 需要提供 key 属性，以便 Vue 能够跟踪每个节点的身份，从而高效的更新 DOM `v-for='item in items':key='item.id'`

- 在`v-for` 块中可以完整地访问父作用域内的属性和变量

##### 双向绑定

> 单向绑定和双向绑定

- 单向绑定：响应式数据的变化会更新 DOM 树，但是 DOM 树上用户操作造成的数据改变不会同步更新到响应式数据

- 双向绑定：响应式数据的变化会更新 DOM 树，但是 DOM 树上用户操作造成的数据改变会同步更新到响应式数据

  - 用户通过表单标签才能输入数据，所以双向绑定都是应用到表单标签上，其他标签不行

  - `v-model` 专门用于双向绑定表单标签的 value 属性，语法为`v-model:value='数据名'`，可以简写为`v-model='数据名'`

  - `v-model` 还可以用于各种不同类型的输入，如`<textarea>`，`<select>`元素

##### 属性计算

> 模版中的表达式虽然方便，但也只能能用来做简单的操作。如果在末班中写太多逻辑，会让模版变得臃肿，难以维护

`computed()`方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**

Vue 的计算属性会自动追踪响应式依赖，检测到该计算属性依赖于某个响应式数据，当该响应式数据发生变化时，会同时更新任何依赖于该计算属性的绑定

##### 数据监听器

> 计算属性允许我们声明性地计算衍生值，然而在有些情况下，需要在状态变化时执行副操作，可以使用 watch 函数在每次响应式状态发生变化时出发回调函数

- watch 主要用于以下场景：

  - 当数据发生变化时需要执行相应的操作

  - 监听数据变化，当满足一定条件时触发相应操作

  - 在异步操作前后需要执行相应的操作

- watchEffect 默认监听所有的响应式数据

##### Vue 生命周期

> 每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置数据侦听、编译模版、挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为 `生命周期钩子的函数`，让开发者有机会再特定阶段运行自己的代码

**周期图解**：

![生命周期](img/Web_53.png)

- 常见钩子函数：

  - `onMounted()` 注册一个回调函数，在组件挂载完成后执行

  - `onUpdated()` 注册一个回调函数，在组件因为响应式状态变更而更新其 DOM 树之后调用

  - `onUnmounted()` 注册一个回调函数，在组件销毁之前调用

  - `onBeforeMount()` 注册一个钩子，在组件被挂载之前被调用

  - `onBeforeUpdate()` 注册一个钩子，在组件即将因为响应式状态变更而更新其 DOM 树之前调用

  - `onBeforeUnmount()` 注册一个钩子，在组件实例被卸载之前调用

##### Vue 组件

###### 组件基础

> 组件允许我们将 UI 划分为独立的、可重用的部分，并且可以对每个部分进行单独的思考。组件就是实现应用中局部功能代码和资源的集合！在市级应用中，组件常常被组织成层层嵌套的树状结构：

![组件](img/Web_54.png)

- 这和我们嵌套 HTML 元素的方式类似，Vue 实现了自己的组件模型，使我们可以在每个组件内封装自定义内容与逻辑

- 组件化：对 html/css/js 统一封装，这是 Vue 中的概念

- 模块化：对 js 的统一封装，这是 ES6 中的概念

- 组件化中，对 js 部分代码的处理使用 ES6 中的模块化

在主 Vue 文件的`<script>`标签中，通过`import`关键字导入组件，然后在`template`标签中使用组件标签，并设置属性值。

```vue
<script>
import HelloWorld from "./components/HelloWorld.vue";
</script>

<template>
  <div>
    <HelloWorld class="helloWorld"></HelloWorld>
  </div>
</template>

<style scoped>
.helloWorld {
  color: red;
}
</style>
```

###### 组件之间传递数据

1. 父传子

> Vue3 中父组件向子组件传值可以通过 props 进行，具体操作如下：

- 首先在父组件中定义需要传递给子组件的值，接着在父组件模版中引入子组件，同时引入子组件的标签中添加 props 属性并为其设置需要传递的值

- 在 Vue3 中，父组件通过 props 传递给子组件的值是响应式的，也就是说，如果在父组件中的传递的值发生了改变，子组件的值也会相应地更新

使用`defindProps()`方法定义 props，并在组件的选项中设置 props 属性

2. 子传父

> Vue3 中子组件向父组件传值可以通过事件进行，具体操作如下：

- 首先在父组件中定义一个方法，在方法中接收子组件传递过来的值，接着在父组件模版中引入子组件，同时在子组件的标签中添加一个事件绑定，并将事件绑定到父组件的方法上

- 在 Vue3 中，子组件通过事件传递给父组件的值是响应式的，也就是说，如果在子组件中的传递的值发生了改变，父组件的方法也会相应地执行

使用`defineEmits()`方法定义事件，并在组件的选项中设置 emits 属性

3. 兄弟组件通信

> Vue3 中兄弟组件通信可以通过事件总线进行，具体操作如下：

子传父 -> 父传子 -> 兄弟组件通信

#### Vue3 路由机制 router

> 路由简介

定义：路由就是根据不同的 URL 地址展示不同的内容或页面

作用：

- 单页应用程序（SPA），路由可以实现不同视图之间的无刷新切换，提升用户体验

- 路由还可以实现页面的认证的权限控制，保护用户的隐私和安全

- 路由还可以利用浏览器的前进后退，帮助用户更好地返回值访问过的页面

##### Vue3 路由的使用

1. 导入路由依赖

```shell
npm install vue-router@4 --save //安装全局的 vue-router 4版本
```

2. 创建路由实例

```vue
<template>
  <div>
    // 路由视图，使用name属性进行区分，没有定义name属性的路由由default渲染
    <router-view></router-view>
    <router-view name="homeView"></router-view>
    <router-view name="listView"></router-view>
    <router-view name="addView"></router-view>
    <router-view name="updateView"></router-view>
    // 路由连接导航
    <router-link to="/">Home</router-link>
    <router-link to="/list">List</router-link>
    <router-link to="/add">Add</router-link>
    <router-link to="/update">Update</router-link>
  </div>
</template>
```

1. 配置路由映射

创建`src/routers/router.js`文件，配置路由映射

```javascript
// 导入路由创建的相关方法
import { createRouter, createWebHashHistory } from "vue-router";

// 导入组件
import Home from '../components/Home.vue'
import List from '../components/List.vue'
import Add from '../components/Add.vue'
import Update from '../components/Update.vue'

// 创建路由对象，声明路由规则
const routes = createRouter({

  //createWebHashHistory() 方法是 Vue.js 基于 hash 模式创建路由的工厂函数。在使用这种模式下，路由信息将被保存在 URL 和 hash 中
  //使用 createWebHashHistory() 方法可以创建一个路由历史记录对象，用于管理应用程序的路由记录
  //通常用该方法来创建路由的历史记录对象
  //路由中缓存历史记录的对象由vue-router提供
  history: createWebHashHistory(),
  routes: [
    {
      path: "/",
      /*
        component指定组件在默认的路由视图位置展示
        components:Home
        components指定组件在name为某个值的路由视图位置展示
        components:{
            default:Home,// 默认路由视图位置
            homeView:Home// name为homeView的路由视图位置
        }
      */
      components:{
        default: Home
        homeView: Home
      }
    },
    {
      path: "/list",
      components: {
        listView: List
      }
    },
    {
      path:'/add',
      components:{
        addView:Add
      }
    },
    {
      path:'/update',
      components:{
        updateView:Update
      }
    },
  ]
});

export default routers;
```

4. main.js 中引入 router 配置

```javascript
import { createApp } from "vue";
import App from "./App.vue";
//导入router模块
import router from "./routers/router.js";
let app = createApp(App);
//绑定路由对象
app.use(router);
//挂载视图
app.mount("#app");
```

##### 路由的重定向

`src/routers/router.js`，在路由对象后面添加 redict 属性，并指定重定向的路径

```javascript
routes: [
  {
    path: "/",
    redirect: "/home",
  },
];
```

##### 编程式路由

> 普通路由

- `<router-link to="/list">List</router-link>` 这种路由，to 中的内容目前是固定的，点击后智能切换/list 对象组件（声明式路由）

> 编程路由

- 通过`useRouter`，动态决定向哪个组件切换的路由

- 在 Vue3 和 Vue Router 4 中，使用 `useRouter` 来实现动态路由

- 这里的 `useRouter` 方法返回的是一个 `router` 对象，可以用来做如导航到新页面、返回上一页面等操作

```vue
<script setup>
import { useRouter } from "vue-router";
const router = useRouter();

function goToList() {
  router.push("/list");
  //router.push({ path: "/list" }); // 也可以使用对象形式的路径
}
</script>
```

##### 路由传参

> 路径参数

- 在路径中使用一个动态字段来实现，我们称之为 **路径参数**

  - 路径参数在路径中以冒号开头，后面跟着参数名，如 `/user/:id`

> 键值对参数

- 类似与 get 请求通过 url 传参，数据是键值对形式的

  - 例如：查看数据详情`/showDetail?hid=1`，hid 为键，1 为值

  - 在 Vue3 和 Vue Router4 中，可以使用 `useRoute` 这个函数从 Vue 的组合式 API 中获取路由对象

  - `useRoute` 方法返回的是当前的 route 对象，可以用它来获取关于当前路由的信息

```vue
<script setup>
import { useRoute } from "vue-router";
let route = useRoute();
let showDetail = (id, language) => {
  // 尝试使用凭借字符串方式传递参数
  // router.push({ path: `/showDetail?id=${id}&language=${language}` });
  // 使用对象形式传递参数，即使用params
  router.push({ name: "showDetail", params: { id: id, language: language } });
  // 后续可以使用route.params.id获取参数值
};

let showDetail2 = (id, language) => {
  // uri键值对参数，需要使用query
  router.push({ path: "/showDetail2", query: { id: id, language: language } });
  // 后续可以使用route.query.id获取参数值，此时参数是键值对形式的
};
</script>
```

同时使用路径参数，需要配置 router.js 文件，增加路径参数占位符

`src/routers/router.js`

```javascript
router: [
  {
    path: "/showDetail2/:id/:language",
    name: "showDetail2",
    component: { showDetailView: ShwoDetail },
  },
];
```

##### 路由守卫

> 在 Vue3 中，路由守卫是用于在路由切换期间进行一些特定任务的回调函数。路由守卫可以用于许多任务，例如验证用户是否已经登录、在路由切换前提供确认提示、请求数据等。Vue3 为路由守卫提供了全面的支持

1. **全局前置守卫**：在路由切换前被调用，可以用于验证用户是否已登录、中断导航、请求数据

2. **全局后置守卫**：在路由切换后被调用，可以用于处理数据、操作 DOM、记录日志

**守卫代码的位置**：`router.js`

```javascript
//全局前置路由守卫
router.beforeEach((to, from, next) => {
  //to 是目标地包装对象  .path属性可以获取地址
  //from 是来源地包装对象 .path属性可以获取地址
  //next是方法，不调用默认拦截！ next() 放行,直接到达目标组件
  //next('/地址')可以转发到其他地址,到达目标组件前会再次经过前置路由守卫
  console.log(to.path, from.path, next);

  //需要判断，注意避免无限重定向
  if (to.path == "/index") {
    next();
  } else {
    next("/index");
  }
});

//全局后置路由守卫
router.afterEach((to, from) => {
  console.log(`Navigate from ${from.path} to ${to.path}`);
});
```

#### Vue3 数据交互 axios

##### promise

> 普通函数和回调函数

- 普通函数：正常调用的函数，一般函数执行完毕后才会继续执行下一行代码

```javascript
let fun1 = function () {
  console.log("fun1");
};
```

- 回调函数：一些特殊的函数，表示未来才会执行的一些功能，后续代码不会等待该函数执行完毕就开始执行了

```javascript
setTimeout(function () {
  console.log("fun2");
}, 1000);
console.log("fun3");
```

> promise：前端中的异步编程技术，类似 Java 中的多线程+线程结果回调

- promise 是异步编程的一种解决方案，比传统的解决方案-回调函数和事件-更合理和强大，ES6 将其写入了语言标准，统一了用法，原生提供了 Promise 对象。

- 所谓 Promise，简单说就是一个容器里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上来说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方式进行处理

> Promise 对象特点

- Promise 对象代表一个异步操作，有三种状态：pending（进行中）、resolved（已成功）、rejected（已失败）。只有异步操作的结果可以决定当前是哪种状态，任何其他操作都无法改变这个状态

- 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 pending 变成 resolved 和从 pending 变成 rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果

> Promise 基本用法

- ES6 规定，Promise 对象是一个构造函数，用来生成 Promise 实例

```javascript
/*  
  1.实例化promise对象,并且执行(类似Java创建线程对象,并且start)
  参数: resolve,reject随意命名,但是一般这么叫!
  参数: resolve,reject分别处理成功和失败的两个函数! 成功resolve(结果)  失败reject(结果)
  参数: 在function中调用这里两个方法,那么promise会处于两个不同的状态
  状态: promise有三个状态
    pending   正在运行
    resolved  内部调用了resolve方法
    rejected  内部调用了reject方法
  参数: 在第二步回调函数中就可以获取对应的结果 
*/
let promise = new Promise((resolve, reject) => {
  console.log("promise");
  // resolve("success");
  // reject("error");
});

// 2. 获取回调函数结果 then会在此处等待promise中的运行结果，但是不会阻塞代码继续运行

promise.then(
  function (value) {
    console.log(`promise 中执行了resolve: ${value}`);
  },
  function (error) {
    console.log(`promise 中执行了reject: ${error}`);
  }
);
```

> promise catch()

`promise.prototype.catch()` 方法是 `.then(null, onRejected)` 的别名，用于指定发生错误时的回调函数

```javascript
let promise = new Promise((resolve, reject) => {
  console.log("promise");
  throw new Error("error");
});
console.log("other");

/* 
  then中的reject()的对应方法可以在产生异常时执行，接收到的就是异常中的提示信息
  then中可以只有一个resolve()的对应方法，reject()方法可以用后续的catch替换
  then中的reject对应的回调函数被后续的catch替换后，catch中接收的数据是一个异常对象
*/

promise
  .then(
    function (value) {
      console.log(`promise 中执行了resolve: ${value}`);
    }
    // function (error) {
    //   console.log(`promise 中执行了reject: ${error}`);
    // }
  )
  .catch(function (error) {
    console.log(`promise 中执行了catch: ${error}`);
  });
```

> async 的使用

- async 和 await 是 ES6 中用于处理异步操作的新特性。通常，异步操作会涉及到 Promise 对象，而 async/await 则是在 Promise 基础上提供了更加直观和易于使用的语法

- async 用于标识函数，标识函数后，async 函数的返回值会变成一个 promise 对象

- 如果函数内部返回的数据是一个非 promise 对象，async 函数的结果会返回一个成功状态的 promise 对象

- 如果函数内部返回的是一个 promise 对象，则 async 函数返回的状态和结果由该对象决定

- 如果函数内部抛出的是一个异常，则 async 函数返回的是一个失败的 promise 对象

```javascript
async function fun1() {
  //return 10
  //throw new Error('error')
  let promise = Promise.reject("hello");
  return promise;
}
let promise = fun1();

promise
  .then(function (value) {
    console.log("success:" + value);
  })
  .catch(function (error) {
    console.log("error:" + error);
  });
```

> await 的使用

- await 右侧的表达式一般为一个 promise 对象，但是也可以是一个其他值

- 如果表达式是 promise 对象，await 返回的是 promise 成功的值

- await 会等右边的 promise 对象执行结束，然后再获取结果，后续代码也会等待 await 的执行

- 如果表达式是其他值，则直接返回该值

- await 必须在 async 函数中使用，但是 async 函数中可以没有 await

- 如果 await 右边的 promise 失败了，就会抛出异常，需要通过 try...catch 捕获异常

```javascript
async function fun1() {
  return 10;
}

async function fun2() {
  try {
    let res = await fun1();
    // let res = await Promise.reject("error");
  } catch (error) {
    console.log(error);
  }
  console.log("other");
}
```

##### Axios

> AJAX

- AJAX = Asynchronous JavaScript and XML，异步 JavaScript 和 XML

- AJAX 不是新的编程语言，而是一种使用现有标准的新方法

- AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容

- AJAX 不需要任何浏览器插件，但需要用户允许 JavaScript 在浏览器上执行

- XMLHttpRequest 只是实现 AJAX 的一种方法

Ajax 工作原理：

![Ajax工作原理](img/Web_55.png)

> Axios

- Axios 是一个基于 promise 网络请求库，作用于 node.js 和浏览器中。它是 isomorphic 的（即同一套代码可以运行在浏览器和 nodejs 环境中）。在服务端使用原生 node.js 的 http 模块，在浏览器中使用 XMLHttpRequest 对象。有着如下特性

  - 从浏览器创建 XMLHttpRequest

  - 从 node.js 创建 http 请求

  - 支持 Promise API

  - 拦截请求和响应

  - 转换请求和响应数据

  - 取消请求

  - 自动转换 JSON 数据

  - 客户端支持防御 XSRF

> Axios 的基本使用方法

导入 Axios 模块依赖

```shell
npm install axios
```

```javascript
import axios from "axios";

// 使用axios发送请求，axios({设置请求的参数}) 请求三要素：1.url：请求的资源路径 2.method：请求的方式 3.data：请求的参数

let promise = axios({
  url: "http://www.example.com/api",
  method: "get",
  data: {
    // 如果请求方式是get，则data中的数据会以键值对形式放在url后
    // 如果请求方式是post，则data中的数据会以JSON形式放入请求体
  },
});

promise
  .then(function (response) {
    // 请求成功，response是服务器返回的数据
    console.log(response);
    /* response 响应结果对象
      data 服务端响应回来的数据
      status 响应状态码
      statusText 响应状态描述
      headers 本次响应的所有响应头
      config 本次请求的配置信息
      request 本次请求发送时使用的XMLHttpRequest对象
    */
  })
  .catch(function (error) {
    // 请求失败，error是错误信息
    console.log(error);
  });
```

> Axios get 和 post 方法

- 添加配置语法

```javascript
axios.get(url[, config])

axios.get(url,{
  key:value,
  key:value
})

axios.post(url[, data[, config]])

axios.post(url,{
  key:value
  key:value
})
```

> Axios 的拦截器

```javascript
import axios from "axios";

// 创建 instance 实例
const instance = axios.create({
  baseURL: "https://api.uomg.com",
  timeout: 10000,
});

// 添加请求拦截
instance.interceptors.request.use(
  // 设置请求头配置信息
  (config) => {
    //处理指定的请求头
    console.log("before request");
    config.headers.Accept = "application/json, text/plain, text/html,_/_";
    return config;
  },
  // 设置请求错误处理函数
  (error) => {
    console.log("request error");
    return Promise.reject(error);
  }
);
// 添加响应拦截器
instance.interceptors.response.use(
  // 设置响应正确时的处理函数
  (response) => {
    console.log("after success response");
    console.log(response);
    return response;
  },
  // 设置响应异常时的处理函数
  (error) => {
    console.log("after fail response");
    console.log(error);
    return Promise.reject(error);
  }
);
// 默认导出
export default instance;
```

### 跨域请求

> 什么是跨域

- 同源策略（Sameoriginpolicy）是浏览器最核心也是最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说 Web 是构建在同源策略基础之上的，浏览器只是针对同源策略的一种具体实现。**同源策略会阻止一个域的 JavaScrip 脚本和另外一个域的内容进行交互。所谓同源（即指在同一个域）就是两个页面具有相同的协议、主机和端口号**

> 为什么会产生跨域

- 前后端分离模式下，客户端请求前端服务器获取视图资源，然后客户端自行向后端服务器获取数据资源，前端服务器的协议、IP 和端口号都与后端服务器很可能是不一样的，这就产生了跨域

![前后端分离模式请求图解](img/Web_56.png)

> 跨域解决方案

- 前端项目代理模式处理

![代理模式处理图解](img/Web_57.png)

- 后端跨域过滤器方式处理

![跨域过滤器处理图解](img/Web_58.png)

### Pinia

> 实现多个组件之间的数据传递

- 组件传参

- 路由传参

- 通过 pinia 状态管理定义共享数据

> 当我们有**多个组件共享一个共同的状态（数据源）**时，多个视图可能都依赖于同一份状态。来自不同视图的交互也可能需要更改同一份状态。虽然手动状态管理（props，组件间通信，模块化）在基础场景够用，在大规模的生产应用中海油很多需要考虑

- 更强的团队协作约定

- 与 Vue DevTools 集成，包括时间轴、组件内部审查和时间旅行调试

- 模块热更新（HMR）

- 服务端渲染支持

Pinia 就是一个实现了上述需求的状态管理库，由 Vue 核心团队维护

#### Pinia 基本用法

> 准备 vite 项目，安装 pinia 依赖

```shell
npm install pinia
```

> 定义 pinia store 对象

`src/store/store.js`

```javascript
import { defineStore } from "pinia";

// 定义数据并对外暴露
// store就是定义共享状态的包装对象
// 内部包含四个属性： id 唯一标识
//                    state 响应式数据，内部键值对格式存储数据，推荐使用箭头函数
//                    getters 存储获得数据、使用数据计算结果的函数（不要修改数据）
//                    actions 定义一些修改数据的函数
export const definedPerson = defineStore({
  id: "personPinia", //必须唯一
  state: () => {
    // state中用于定义数据
    return {
      username: "张三",
      age: 0,
      hobbies: ["唱歌", "跳舞"],
    };
  },
  getters: {
    // 用于定义一些通过数据计算而得到结果的一些方法 一般在此处不做对数据的修改操作
    // getters中的方法可以当做属性值方式使用
    getHobbiesCount() {
      return this.hobbies.length;
    },
    getAge() {
      return this.age;
    },
  },
  actions: {
    // 用于定义一些对数据修改的方法
    doubleAge() {
      this.age = this.age * 2;
    },
  },
});
```

> 使用 pinia store 对象

`src/main.js`开启全局的 pinia 功能

```javascript
import { createApp } from "vue";
import App from "./App.vue";
import router from "./routers/router.js";
// 导pinia
import { createPinia } from "pinia";
// 创建pinia对象
let pinia = createPinia();

let app = createApp(App);
app.use(router);
// app中使用pinia功能
app.use(pinia);
app.mount("#app");
```

> 在组件中使用 pinia store 对象

```javascript
import {} from "../store/store.js";
let person = definePerson();
```

#### Pinia 其它细节

**State（状态）**在大多数情况下，state 都是你的 store 核心。通常会先定义代表他们 APP 的 state。在 Pinia 中，state 被定义为一个返回初始状态的函数

```javascript
import { defineStore } from "pinia";

export const definedPerson = defineStore(`personPinia`,
{
  state: () => {
    return {
      username: "张三",
      age: 0,
      hobbies: ["唱歌", "跳舞"],
    };
  }
}
```

**Getter** 完全等同于 store 的 state 的计算值。可以通过 `defineStore()` 中的 getters 属性来定义，推荐使用箭头函数，并且作为第一个参数接收 state。

```javascript
export const useStore = defineStore("main", {
  state: () => ({
    count: 0,
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
});
```

**Action** 相当于组件中的 method，他们可以通过 defineStore() 中的 actions 属性来定义，并且它们也是定义业务逻辑的完美选择。类似 getter、action 可以以通过 this 访问整个 store 实例，并支持\*\*完整的类型标注（以及自动补全）。不同的是 action 可以是异步的

```javascript
export const useStore = defineStore("main", {
  state: () => ({
    count: 0,
  }),
  actions: {
    increment(state) {
      state.count++;
    },
    randomizeCounter() {
      this.count = Math.round(Math.random() * 100);
    },
  },
});
```
