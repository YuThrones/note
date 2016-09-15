#JavaScript
* JavaScript代码可以直接嵌在网页的**任何地方**，不过通常我们都把JavaScript代码放到<head>中
* JavaScript：写入 HTML 输出
只能在 HTML 输出中使用 document.write。如果在文档加载后使用该方法，会覆盖整个文档。
```
document.write("<h1>This is a heading</h1>");
document.write("<p>This is a paragraph</p>");
```
* JavaScript：对事件作出反应
```<button type="button" onclick="alert('Welcome!')">点击这里</button>```
alert() 函数在 JavaScript 中并不常用，但它对于代码测试非常方便。

* JavaScript：改变 HTML 内容
使用 JavaScript 来处理 HTML 内容是非常强大的功能。
```
x=document.getElementById("demo")  //查找元素
x.innerHTML="Hello JavaScript";    //改变内容
```

* HTML 中的脚本必须位于 ```<script>``` 与 ```</script>``` 标签之间,浏览器会解释并执行位于 ```<script>``` 和 ```</script>``` 之间的 JavaScript。

* ```<head>``` 中的 JavaScript 函数
在本例中，我们把一个 JavaScript 函数放置到 HTML 页面的 ```<head>``` 部分。
该函数会在点击按钮时被调用：  
```
<!DOCTYPE html>
<html>
<head>
<script>
function myFunction()
{
document.getElementById("demo").innerHTML="My First JavaScript Function";
}
</script>
</head>
<body>
<h1>My Web Page</h1>
<p id="demo">A Paragraph</p>
<button type="button" onclick="myFunction()">Try it</button>
</body>
</html>
```

* 外部的 JavaScript
也可以把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码。
外部 JavaScript 文件的文件扩展名是 .js。
如需使用外部文件，请在 <script> 标签的 "src" 属性中设置该 .js 文件:
```
<!DOCTYPE html>
<html>
<body>
<script src="myScript.js"></script>
</body>
</html>
```

* 操作 HTML 元素
如需从 JavaScript 访问某个 HTML 元素，您可以使用 document.getElementById(id) 方法。
请使用 "id" 属性来标识 HTML 元素