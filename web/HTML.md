#HTML
##简介
* HTML 指的是**超文本标记语言 (Hyper Text Markup Language)**
HTML 不是一种编程语言，而是一种**标记语言 (markup language)**
标记语言是一套**标记标签 (markup tag)**
* HTML 标签是由**尖括号**包围的关键词，比如 <html>
HTML 标签通常是**成对**出现的，比如 <b> 和 </b
标签对中的第一个标签是**开始标签**，第二个标签是**结束标签**
开始和结束标签也被称为**开放标签**和**闭合标签**
* 例子：
```
<html>
<body>
<h1>My First Heading</h1>
<p>My first paragraph.</p>
</body>
</html>
##########
<html> 与 </html> 之间的文本描述网页
<body> 与 </body> 之间的文本是可见的页面内容
<h1> 与 </h1> 之间的文本被显示为标题
<p> 与 </p> 之间的文本被显示为段落
```

##基本的 HTML 标签 
* HTML 标题
HTML 标题（Heading）是通过 ```<h1> - <h6>``` 等标签进行定义的。
* HTML 段落
HTML 段落是通过 ```<p>``` 标签进行定义的。
* HTML 链接
HTML 链接是通过 ```<a>``` 标签进行定义的。
例如```<a href="http://www.w3school.com.cn">This is a link</a>```
* HTML 图像
HTML 图像是通过 ```<img>``` 标签进行定义的。
例如```<img src="w3school.jpg" width="104" height="142" />```
**注释**：图像的名称和尺寸是以属性的形式提供的。
* ```<hr />``` 标签在 HTML 页面中创建水平线。
* ```<q>```定义短的引用。
* ```<blockquote>``` 元素定义被引用的节。 
* ```<script>``` 标签用于定义客户端脚本，比如 JavaScript。
script 元素既可包含脚本语句，也可通过 src 属性指向外部脚本文件。
必需的 type 属性规定脚本的 MIME 类型。
JavaScript 最常用于图片操作、表单验证以及内容动态更新。
* 如何应付老式的浏览器
如果浏览器压根没法识别 ```<script>``` 标签，那么 ```<script>``` 标签所包含的内容将以文本方式显示在页面上。为了避免这种情况发生，你应该将脚本隐藏在注释标签当中。那些老的浏览器（无法识别 ```<script>``` 标签的浏览器）将忽略这些注释，所以不会将标签的内容显示到页面上。而那些新的浏览器将读懂这些脚本并执行它们，即使代码被嵌套在注释标签内。
```
<script type="text/javascript">
<!--
document.write("Hello World!")
//-->
</script>
```
* ```<noscript>``` 标签提供无法使用脚本时的替代内容，比方在浏览器禁用脚本时，或浏览器不支持客户端脚本时。
noscript 元素可包含普通 HTML 页面的 body 元素中能够找到的所有元素。
只有在浏览器不支持脚本或者禁用脚本时，才会显示 noscript 元素中的内容

##HTML 元素
* HTML 文档是由 HTML 元素定义的。
* HTML 元素指的是从开始标签（start tag）到结束标签（end tag）的所有代码。  
某些 HTML 元素具有**空内容（empty content）**
空元素在开始标签中进行关闭（以开始标签的结束而结束）
大多数 HTML 元素可拥有属性
在 XHTML、XML 以及未来版本的 HTML 中，所有元素都必须被关闭。
在开始标签中添加斜杠，比如```<br />```，是关闭空元素的正确方法，HTML、XHTML 和 XML 都接受这种方式。
即使```<br>``` 在所有浏览器中都是有效的，但使用 ```<br />``` 其实是更长远的保障。
可以将注释插入 HTML 代码中，这样可以提高其可读性，使代码更易被人理解。浏览器会忽略注释，也不会显示它们。
**例：**```<!-- This is a comment -->```
如果您希望在不产生一个新段落的情况下进行换行（新行），请使用 ```<br />``` 标签
* 在显示计算机代码示例时，并不需要如此。
```<kbd>```, ```<samp>```, 以及 ```<code>``` 元素全都支持固定的字母尺寸和间距。

* 表格由 ```<table>``` 标签来定义。每个表格均有若干行（由 ```<tr>``` 标签定义），每行被分割为若干单元格（由 ```<td>``` 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。
```
<table border="1">
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>
```

* 有序列表用```<ul>```,无序列表用```<ol>```  
```
<ol start="50">
  <li>咖啡</li>
  <li>牛奶</li>
  <li>茶</li>
</ol>
```

* iframe 用于在网页内显示网页。
height 和 width 属性用于规定 iframe 的高度和宽度。
属性值的默认单位是像素，但也可以用百分比来设定（比如 "80%"）。
frameborder 属性规定是否显示 iframe 周围的边框。
设置属性值为 "0" 就可以移除边框：
```
<iframe src="demo_iframe.htm" width="200" height="200"></iframe>
<iframe src="demo_iframe.htm" frameborder="0"></iframe>
```

* ```<head>``` 元素是所有头部元素的容器。```<head>``` 内的元素可包含脚本，指示浏览器在何处可以找到样式表，提供元信息，等等。
以下标签都可以添加到 head 部分：```<title>```、```<base>```、```<link>```、```<meta>```、```<script>``` 以及 ```<style>```。

* ```<title>``` 标签定义文档的标题。
title 元素在所有 HTML/XHTML 文档中都是必需的。
title 元素能够：
定义浏览器工具栏中的标题
提供页面被添加到收藏夹时显示的标题
显示在搜索引擎结果中的页面标题

* ```<base>``` 标签为页面上的所有链接规定默认地址或默认目标（target）：
```
<head>
<base href="http://www.w3school.com.cn/images/" />
<base target="_blank" />
</head>
```

* ```<link>``` 标签定义文档与外部资源之间的关系。
```<link>``` 标签最常用于连接样式表：
```
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

* ```<style>``` 标签用于为 HTML 文档定义样式信息。
您可以在 style 元素内规定 HTML 元素在浏览器中呈现的样式

* 元数据（metadata）是关于数据的信息。
```<meta>``` 标签提供关于 HTML 文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。
典型的情况是，meta 元素被用于规定页面的描述、关键词、文档的作者、最后修改时间以及其他元数据。
```<meta>``` 标签始终位于 head 元素中。
元数据可用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 web 服务。
一些搜索引擎会利用 meta 元素的 name 和 content 属性来索引您的页面。

* <form> 元素
HTML 表单用于收集用户输入。
<input> 元素是最重要的表单元素。
<input> 元素有很多形态，根据不同的 type 属性, 例如text，radio，submit
value 属性规定输入字段的初始值
readonly 属性规定输入字段为只读（不能修改）
disabled 属性规定输入字段是禁用的。
被禁用的元素是不可用和不可点击的。
被禁用的元素不会被提交。
```
<form>
<input type="radio" name="sex" value="male" checked >Male
<br>
<input type="radio" name="sex" value="female">Female
</form> 
```
表单处理程序在表单的 action 属性中指定, method 属性规定在提交表单时所用的 HTTP 方法（GET 或 POST）:
```
<form action="action_page.php" method="POST">
First name:<br>
<input type="text" name="firstname" value="Mickey" readonly>
<br>
Last name:<br>
<input type="text" name="lastname" value="Mouse">
<br><br>
<input type="submit" value="Submit">
</form>
```
何时使用 GET？
您能够使用 GET（默认方法）：
如果表单提交是被动的（比如搜索引擎查询），并且没有敏感信息。
何时使用 POST？
您应该使用 POST：
如果表单正在更新数据，或者包含敏感信息（例如密码）。
POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的。

##HTML 属性
* HTML 标签可以拥有属性。属性提供了有关 HTML 元素的更多的信息。
属性总是以名称/值对的形式出现，比如：name="value"。
属性总是在 HTML 元素的开始标签中规定。
例：```<h1 align="center">``` 拥有关于对齐方式的附加信息,居中排列标题.
```<body bgcolor="yellow">``` 拥有关于背景颜色的附加信息。
* 适用于大多数 HTML 元素的属性
class:规定元素的类名（classname）
id:规定元素的唯一 id
style:规定元素的行内样式（inline style）
title:规定元素的额外信息（可在工具提示中显示）
* 条件注释
您也许会在 HTML 中偶尔发现条件注释：
```
<!--[if IE 8]>
    .... some HTML here ....
<![endif]-->
```
* 使用 Target 属性，你可以定义被链接的文档在何处显示。
```<a href="http://www.w3school.com.cn/" target="_blank">Visit W3School!</a>```

* name 属性规定锚（anchor）的名称。
您可以使用 name 属性创建 HTML 页面中的书签。
当使用命名锚（named anchors）时，我们可以创建直接跳至该命名锚（比如页面中某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了。
首先，我们在 HTML 文档中对锚进行命名（创建一个书签）：
```<a name="tips">基本的注意事项 - 有用的提示</a>```
然后，我们在同一个文档中创建指向该锚的链接：
```<a href="#tips">有用的提示</a>```
您也可以在其他页面中创建指向该锚的链接：
```<a href="http://www.w3school.com.cn/html/html_links.asp#tips">有用的提示</a>```

* 在 HTML 中，某些字符是预留的。
在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。
如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。
如需显示小于号，我们必须这样写：&lt; 或 &#60;

##HTML样式
* style 属性用于改变 HTML 元素的样式，提供了一种改变所有 HTML 元素的样式的通用方法。  
例：
```
<html>
<body style="background-color:yellow">
<h2 style="background-color:red">This is a heading</h2>
<p style="background-color:green">This is a paragraph.</p>
</body>
</html>
```  
文本格式化网站链接：
[HTML文本格式化](http://www.w3school.com.cn/html/html_formatting.asp "HTML文本格式化")
* 连接外部样式表
```
<link rel="stylesheet" type="text/css" href="/html/csstest1.css" >
```

* 内部样式表
当单个文件需要特别样式时，就可以使用内部样式表。你可以在 head 部分通过``` <style>``` 标签定义内部样式表：
```
<head>
<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
</head>
```

* 内联样式
当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。
```
<p style="color: red; margin-left: 20px">
This is a paragraph
</p>
```

* alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。
```<img src="boat.gif" alt="Big Boat">```

##HTML 块元素
大多数 HTML 元素被定义为块级元素或内联元素。
编者注：“块级元素”译为 block level element，“内联元素”译为 inline element。
* 块级元素在浏览器显示时，通常会以新行来开始（和结束）。  
例子：```<h1>```, ```<p>```,``` <ul>```, ```<table>```
* HTML 内联元素
内联元素在显示时通常不会以新行开始。  
例子：```<b>```, ```<td>```, ```<a>```, ```<img>```
* HTML ```<div>``` 元素是块级元素，它是可用于组合其他 HTML 元素的容器。
* HTML ```<span>``` 元素是内联元素，可用作文本的容器。

##HTML类
对 HTML 进行分类（设置类），使我们能够为元素的类定义 CSS 样式。
为相同的类设置相同的样式，或者为不同的类设置不同的样式。
```
<!DOCTYPE html>
<html>
<head>
<style>
.cities {
    background-color:black;
    color:white;
    margin:20px;
    padding:20px;
} 
</style>
</head>

<body>

<div class="cities">
<h2>London</h2>
<p>
London is the capital city of England. 
It is the most populous city in the United Kingdom, 
with a metropolitan area of over 13 million inhabitants.
</p>
</div> 

</body>
</html>
```

##什么是响应式 Web 设计？
RWD 指的是响应式 Web 设计（Responsive Web Design）
RWD 能够以可变尺寸传递网页
RWD 对于平板和移动设备是必需的

##框架
通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。每份HTML文档称为一个框架，并且每个框架都独立于其他的框架。
使用框架的坏处：
开发人员必须同时跟踪更多的HTML文档
很难打印整张页面

框架结构标签（```<frameset>```）定义如何将窗口分割为框架
每个 frameset 定义了一系列行或列
rows/columns 的值规定了每行或每列占据屏幕的面积
```
<frameset cols="25%,75%">
   <frame src="frame_a.htm">
   <frame src="frame_b.htm">
</frameset>
```
假如一个框架有可见边框，用户可以拖动边框来改变它的大小。为了避免这种情况发生，可以在 ```<frame>``` 标签中加入：noresize="noresize"。

##<!DOCTYPE> 声明
Web 世界中存在许多不同的文档。只有了解文档的类型，浏览器才能正确地显示文档。
HTML 也有多个不同的版本，只有完全明白页面中使用的确切 HTML 版本，浏览器才能完全正确地显示出 HTML 页面。这就是 <!DOCTYPE> 的用处。
<!DOCTYPE> 不是 HTML 标签。它为浏览器提供一项信息（声明），即 HTML 是用什么版本编写的。