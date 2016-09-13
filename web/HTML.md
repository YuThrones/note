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
[HTML文本格式化](http://www.w3school.com.cn/html/html_formatting.asp "HTML文本格式化")
