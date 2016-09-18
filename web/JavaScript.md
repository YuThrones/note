#JavaScript
* JavaScript代码可以直接嵌在网页的**任何地方**，不过通常我们都把JavaScript代码放到```<head>```中

* 第二种方法是把JavaScript代码放到一个单独的.js文件，然后在HTML中通过<script src="..."></script>引入这个文件：
```
<html>
<head>
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```

* 有些时候你会看到```<script>```标签还设置了一个type属性：
```
<script type="text/javascript">
  ...
</script>
```
但这是没有必要的，因为默认的type就是JavaScript，所以不必显式地把type指定为JavaScript。

##用Google Chrome调试代码
* 按F12打开控制台
先点击“控制台(Console)“，在这个面板里可以直接输入JavaScript代码，按回车后执行。
要查看一个变量的内容，在Console中输入console.log(a)，回车后显示的值就是变量的内容。

##基本语法
* JavaScript的语法和Java语言类似，每个语句以;结束，语句块用{...}。但是，JavaScript并不强制要求在每个语句的结尾加;，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上;。

* 下面的一行代码就是一个完整的赋值语句：
```var x = 1;```

* 以//开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript引擎会自动忽略：
  另一种块注释是用/*...*/把多行字符包裹起来，把一大“块”视为一个注释：

##数据类型
* JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型

* 计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，十六进制用0x前缀和0-9，a-f表示，例如：0xff00，0xa5b4c3d2，等等，它们和十进制表示的数值完全一样。

* Number可以直接做四则运算，规则和数学一致

* JavaScript允许对任意数据类型做比较：

* 要特别注意相等运算符```==```。JavaScript在设计时，有两种比较运算符：
  第一种是```==```比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
  第二种是```===```比较，它不会自动转换数据类型，如果数据类型不一致，返回```false```，如果一致，再比较。
  由于JavaScript这个设计缺陷，不要使用```==```比较，始终坚持使用```===```比较。
  另一个例外是NaN这个特殊的Number与所有其他值都不相等，包括它自己：
  ```NaN === NaN; // false```

* 唯一能判断NaN的方法是通过isNaN()函数：
```isNaN(NaN); // true```

* 最后要注意浮点数的相等比较：
```1 / 3 === (1 - 2 / 3); // false```
这不是JavaScript的设计缺陷。浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：
```Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true```

* ```null``` 表示一个“空”的值，它和 ```0``` 以及空字符串 ```''``` 不同， ```0``` 是一个数值， ```''``` 表示长度为0的字符串，而null表示“空”。

* 数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。例如：
```[1, 2, 3.14, 'Hello', null, true];```

* 另一种创建数组的方法是通过Array()函数实现：
```new Array(1, 2, 3); // 创建了数组[1, 2, 3]```

* JavaScript的对象是一组由键-值组成的无序集合，例如：
```
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

* 在JavaScript中，使用等号=对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量，但是要注意只能用var申明一次

* JavaScript在设计之初，为了方便初学者学习，并不强制要求用var申明变量。这个设计错误带来了严重的后果：如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量：  
```i = 10; // i现在是全局变量```  
  在同一个页面的不同的JavaScript文件中，如果都不用var申明，恰好都使用了变量i，将造成变量i互相影响，产生难以调试的错误结果。  
  使用var申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内（函数的概念将稍后讲解），同名变量在不同的函数体内互不冲突。  
  为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误。
  启用strict模式的方法是在JavaScript代码的第一行写上：  
```'use strict';```  

###字符串
* 由于多行字符串用\n写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用` ... `表示：  
```
`这是一个
多行
字符串`;
```

* 如果有很多变量需要连接，用+号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：  
```
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

* 字符串常见的操作如下：  
```
var s = 'Hello, world!';
s.length; // 13
```
**需要特别注意的是，字符串是不可变的**，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：  
```
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```

* JavaScript为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串：  
toUpperCase()把一个字符串全部变为大写：  
```
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
```
indexOf()会搜索指定字符串出现的位置：  
```
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```
substring()返回指定索引区间的子串：  
```
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```
###数组
* 注意，直接给Array的length赋一个新的值会导致Array大小的变化：  
```
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
```

* 请注意，如果通过索引赋值时，索引超过了范围，同样会引起Array大小的变化：  
```
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

* slice()就是对应String的substring()版本，它截取Array的部分元素，然后返回一个新的Array：  
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```
如果不给slice()传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个Array  

* push和pop  
push()向Array的末尾添加若干元素，pop()则把Array的最后一个元素删除掉：  
```
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
```

* unshift和shift  
如果要往Array的头部添加若干元素，使用unshift()方法，shift()方法则把Array的第一个元素删掉：  
```
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
```

* sort  
sort()可以对当前Array进行排序，它会直接修改当前Array的元素位置，直接调用时，按照默认顺序排序：  
```
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

* reverse()把整个Array的元素给掉个个，也就是反转：  
```
var arr = ['one', 'two', 'three'];
arr.reverse(); 
arr; // ['three', 'two', 'one']
```

* splice()方法是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：  
```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

* ```concat()```  方法把当前的Array和另一个Array连接起来，并返回一个新的Array：  
```
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']
```
实际上，```concat()```  方法可以接收任意个元素和Array，并且自动把Array拆开，然后全部添加到新的Array里：  
```
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```
```join()``` 方法是一个非常实用的方法，它把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：    
```
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

* JavaScript用一个{...}表示一个对象，键值对以xxx: xxx形式申明，用,隔开。注意，最后一个键值对不需要在末尾加,，如果加了，有的浏览器（如低版本的IE）将报错。  
```
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
```
访问属性是通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用''括起来：  
```
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```
xiaohong的属性名middle-school不是一个有效的变量，就需要用''括起来。访问这个属性也无法使用.操作符，必须用['xxx']来访问：  
```
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
```
如果访问一个不存在的属性会返回什么呢？JavaScript规定，访问不存在的属性不报错，而是返回undefined：  
```
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
```
* 由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性：  
```
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
```

* 如果我们要检测xiaoming是否拥有某一属性，可以用```in```操作符
不过要小心，如果in判断一个属性存在，这个属性不一定是xiaoming的，它可能是xiaoming继承得到的：  
要判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用hasOwnProperty()方法：  
```
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```

* JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true，因此上述代码条件判断的结果是true。  
```
var s = '123';
if (s.length) { // 条件计算结果为3
    //
}
```

* for循环的一个变体是```for ... in```循环，它可以把一个对象的所有属性依次循环出来：  
```
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    alert(key); // 'name', 'age', 'city'
}
```

* ```Map``` 是一组键值对的结构，具有极快的查找速度。  
```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```
初始化Map需要一个二维数组，或者直接初始化一个空Map。Map具有以下方法：  
```
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

* Set和Map类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key。
要创建一个Set，需要提供一个Array作为输入，或者直接创建一个空Set：  
```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

* 遍历Array可以采用下标循环，遍历Map和Set就无法使用下标。为了统一集合类型，ES6标准引入了新的iterable类型，Array、Map和Set都属于iterable类型。
具有iterable类型的集合可以通过新的for ... of循环来遍历。
for ... of循环是ES6引入的新的语法，请测试你的浏览器是否支持

##函数
* 在JavaScript中，定义函数的方式如下：  
```
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```
第二种定义函数的方式如下：  
```
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

* 调用函数时，按顺序传入参数即可：  
```
abs(10); // 返回10
```
由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：  
```
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```
传入的参数比定义的少也没有问题：  
```
abs(); // 返回NaN
```
要避免收到undefined，可以对参数进行检查：  
```
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

* arguments
JavaScript还有一个免费赠送的关键字arguments，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。arguments类似Array但它不是一个Array,利用arguments，你可以获得调用者传入的所有参数。也就是说，即使函数不定义任何参数，还是可以拿到参数的值:  
```
function foo(x) {
    alert(x); // 10
    for (var i=0; i<arguments.length; i++) {
        alert(arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

* rest参数
由于JavaScript函数允许接收任意个参数，于是我们就不得不用arguments来获取所有参数：  
```
function foo(a, b) {
    var i, rest = [];
    if (arguments.length > 2) {
        for (i = 2; i<arguments.length; i++) {
            rest.push(arguments[i]);
        }
    }
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}
```
ES6标准引入了rest参数，上面的函数可以改写为：  
```
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}
```
rest参数只能写在最后，前面用```...```标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量rest，所以，不再需要arguments我们就获取了全部参数。
如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是```undefined```）。  

* 由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：  
```
'use strict';
function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```
JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。 

* 不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性：  
```
'use strict';
var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'
```  

* 全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：  

```
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

* 局部作用域
由于JavaScript的变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量的：  
```
'use strict';
function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```  
为了解决块级作用域，ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量：  
```
'use strict';
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    i += 1; // SyntaxError
}
```

* ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域：
```
'use strict';
const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```