# CSS
## CSS 概述
* CSS 指层叠样式表 (Cascading Style Sheets)
* 样式定义如何显示 HTML 元素
* 样式通常存储在样式表中
* 把样式添加到 HTML 4.0 中，是为了解决内容与表现分离的问题
* 外部样式表通常存储在 CSS 文件中
* 多个样式定义可层叠为一
* 当同一个 HTML 元素被不止一个样式定义时，会使用哪个样式呢？
一般而言，所有的样式会根据下面的规则层叠于一个新的虚拟样式表中，其中数字 4 拥有最高的优先权。
1.浏览器缺省设置
2.外部样式表
3.内部样式表（位于 <head> 标签内部）
4.内联样式（在 HTML 元素内部）

## CSS 语法
CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。
```selector {declaration1; declaration2; ... declarationN }```
选择器通常是您需要改变样式的 HTML 元素。
每条声明由一个属性和一个值组成。
属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。
```selector {property: value}```
例:
![示意图](http://www.w3school.com.cn/i/ct_css_selector.gif)

* 如果值为若干单词，则要给值加引号：
```p {font-family: "sans serif";}```

* 你可以对选择器进行分组，这样，被分组的选择器就可以分享相同的声明。用逗号将需要分组的选择器分开。
```
h1,h2,h3,h4,h5,h6 {
  color: green;
  }
```

* 派生选择器
通过依据元素在其位置的上下文关系来定义样式，你可以使标记更加简洁。
比方说，你希望列表中的 strong 元素变为斜体字，而不是通常的粗体字，可以这样定义一个派生选择器：
```
li strong {
    font-style: italic;
    font-weight: normal;
  }
```

* id 选择器
id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。
id 选择器以 "#" 来定义。
```
#red {color:red;}
#green {color:green;}
```
**注意：id 属性只能在每个 HTML 文档中出现一次。**
即使被标注为 sidebar 的元素只能在文档中出现一次，这个 id 选择器作为派生选择器也可以被使用很多次
```
#sidebar p {
	font-style: italic;
	text-align: right;
	margin-top: 0.5em;
	}
#sidebar h2 {
	font-size: 1em;
	font-weight: normal;
	font-style: italic;
	margin: 0;
	line-height: 1.5;
	text-align: right;
	}
```
上面的样式只会应用于出现在 id 是 sidebar 的元素内的段落。这个元素很可能是 div 或者是表格单元，尽管它也可能是一个表格或者其他块级元素。它甚至可以是一个内联元素，比如 ```<em></em>``` 或者 ```<span></span>```，不过这样的用法是非法的，因为不可以在内联元素 ```<span>``` 中嵌入 ```<p>``` 

* 在 CSS 中，类选择器以一个点号显示：
```.center {text-align: center}```
注意：类名的**第一个字符不能使用数字**！它无法在 Mozilla 或 Firefox 中起作用。
和 id 一样，class 也可被用作派生选择器
```
.fancy td {
	color: #f60;
	background: #666;
	}
```
元素也可以基于它们的类而被选择：
```
td.fancy {
	color: #f60;
	background: #666;
	}
```

* 对带有指定属性的 HTML 元素设置样式。
可以为拥有指定属性的 HTML 元素设置样式，而不仅限于 class 和 id 属性。
下面的例子为带有 title 属性的所有元素设置样式：
```
[title]
{
color:red;
}
```
属性和值选择器 - 多个值
下面的例子为包含指定值的 title 属性的所有元素设置样式。适用于由空格分隔的属性值：
```[title~=hello] { color:red; }```
下面的例子为带有包含指定值的 lang 属性的所有元素设置样式。适用于由连字符分隔的属性值：
```[lang|=en] { color:red; }```

##如何插入样式表
当读到一个样式表时，浏览器会根据它来格式化 HTML 文档。插入样式表的方法有三种：
* 外部样式表
当样式需要应用于很多页面时，外部样式表将是理想的选择。在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。每个页面使用 ```<link>``` 标签链接到样式表。```<link>``` 标签在（文档的）头部：
```
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

* 内部样式表
当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用 ```<style>``` 标签在文档头部定义内部样式表，就像这样:
```
<head>
<style type="text/css">
  hr {color: sienna;}
  p {margin-left: 20px;}
  body {background-image: url("images/back40.gif");}
</style>
</head>
```

* 内联样式
由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。
要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：
```
<p style="color: sienna; margin-left: 20px">
This is a paragraph
</p>
```

* 多重样式
如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。
**注：**即有同名属性时，较内层的属性会取代外面的，但是，**其他没有同名的不会被取代**

* 背景图像
要把图像放入背景，需要使用 background-image 属性。background-image 属性的默认值是 none，表示背景上没有放置任何图像。
如果需要设置一个背景图像，必须为这个属性设置一个 URL 值：
```body {background-image: url(/i/eg_bg_04.gif);}```
