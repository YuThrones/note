#C++ Primer
##2.2
extern int i 声明i而非定义，在外部使用
：：全局作用域

##2.3
&引用（Reference），给变量起另一个名字，必须初始化，无法重新绑定，不能定义引用的引用
&取地址符
不能定义指向引用的指针
```*``` 解引用（取内容）
```void*``` 能存放任何类型对象的地址，但不能直接操作
```int *&a = b；``` a是对指针b的引用

##2.4
const对象必须初始化
const默认状态下只在文件内有效
const int &a 对常量的引用a
不能让一个非常量引用指向一个常量对象
只能用常量指针指向常量对象
int *const a常量指针 初始化后不可改变

##2.5
typedef定义类型别名
typedef double wages //wages是double的同义词
using SI = Sales_item //SI是Sales_item的同义词
(C++11)auto可以在一行里声明多个变量，但是其基本数据类型必须一样
decltype返回操作数的数据类型而不实际计算表达式的值
decltype(f()) sum=x; //sum的类型就是函数f的返回类型
decltype(*p)得到的是int&而非int

##2.6
头文件保护符
```
#ifdef仅当变量已定义时为真
#ifndef仅当变量为定义时为真
```
一旦检查结果为真，则执行后续操作直至遇到#endif指令为止

##3.1
using std::cin声明cin
头文件不应该包含using声明，因为会拷贝到所有被引用的文件中
string s7(10, 'c')1   “cccccccccc”
getline(is, s)从is中读取一行赋给s
string.zi返回的是size_type类型，无符号整型数
string用+号必须确保两边有一个是string对象
for(变量：对象)  例如for(auto c: str)
用引用可以改变string中字符的值

##3.3
老式C++标准中vector<vector<int> >必须有空格
vector<int> ivec(10,-1)初始化10个-1
vector<int> ivec{10，-1}初始化10和-1
当花括号无法列表初始化时会尝试默认值初始化（）
vector可以高效添加元素增长
如果循环内部有向vector添加元素的语句，不能用范围for循环
for(auto &i:v)可以通过i给v的元素赋值
vector的下标可以用于访问已存在元素而不能用于添加元素

##3.4
```*iter``` 返回迭代器iter所指元素的引用
```
iter->men等价于（*iter）.mem
vector<int>::iterator it
```
vector<int>::const_iterator只能读取不能修改，常量对象只能用他读取

##3.5
a[d]  d必须是一个常量表达式
不能用auto推断数组类型
用字符串字面值初始化字符数组会自动添加'\0'
不能用一个数组初始化另外一个数组或者赋值
int (*p)[10]=&a   p指向含有10个整数的数组
指针也是迭代器，也可以用来访问数组
begin，end可以操作数组得到头尾指针，例如begin(a)
传入strlen等函数的指针必须指向以空字符结尾的数组

##4.1
当一个对象被用作右值的时候，用的是对象的值（内容）；当对象被用作左值的时候，用的是对象的身份（在内存中的位置）

##4.5
尽量用++i而不用i++，因为i++会有创建副本
```*beg=toupper(*beg++)``` 有问题，结果未定义

##4.9
sizeof (type) 返回一个类型名字所占字节数
sizeof expr 返回表达式结果类型的大小，并不实际计算值

##4.11

cast-name<type>(expression)显示转换，casst-name是static_cast、dynamic_cast、const_cast、reinterpret_cast中的一种

只要不包含底层const都可以用static_cast
```
void* p = &d;
double *dp=static_cast<double*>(p);可以召回存在于void指针中的值
```
const_cast只能改变对象的底层const

reinterpret_cast本质上依赖于机器，非常危险


##4.12
运算符优先级表

##5.5
goto不能跳过变量定义	
					
##5.6
throw引发异常
try处理语句块处理异常，以一个或多个catch结束
exception class用于在throw跟catch之间传递异常具体信息
throw runtime_error是标准库异常类型的一种
execption.what()返回一个C字符串

##6.1
函数返回类型不能是数组和函数，但是可以是指向他们的指针
static在第一次定义时初始化，直到程序终止

##6.2
* 命令行选项
  例如main函数位于prog之内，我们可以传递下面的选项：
```
prog -d -o ofile data0
int main(int argc, char * argv[]) {...}
```
第一个形参argc表示这组数组中的字符串数量，第二个形参是数组，可以表示成
```
int main(int argc, char **argv){...}
```

##6.3
不能返回局部变量的引用，不然会产生错误
如果想定义一个返回数组指针的函数，则数组维度必须跟在函数名字之后，返回的数组指针的函数形式如下：
```Type (*function(parameter _list))[dimension]```
例：```int (*func(int i))[10];``` 
* `func(int i)` 表示调用func函数需要一个int类型的实参
* `(*func(int i))` 意味着我们可以对函数调用的结果执行解引用操作
* `(*func(int i))[10]` 表示解引用func的调用将得到一个大小是10的数组
* `int (*func(int i))[10]` 表示数组中的元素是int类型

可以用decltype关键字声明返回类型，如果我们已知指针将指向哪个数组：
```
int odd[] = {1,3,5,7,9}
int even[] = {0,2,4,6,8}
decltype(odd) *arrPtr(int i)
{
	return (i%2) ? &odd : &even;
}
```
不过decltype并不负责将数组类型转换成相应的指针，所以decltype的结果是个数组，要想返回arrPtr返回数组还必须在函数声明时加一个`*`符号。

##6.4
* 如果一个作用域内的几个函数名字相同但是参数列表不同，我们称之为重载函数。

##6.5
可以重复声明一个函数，但是不可以修改已经声明的默认参数值
在函数的返回类型前面加强关键字```inline``` ，可以把函数声明为内联函数，消除运行时开销
```inline const string& func()```
constexpr函数指能用于常量表达式的函数,函数的返回值和所有形参的类型都得是字面值类型

##6.7
要声明一个可以指向函数的指针，只需要把函数名替换成指针：
```
bool com(string a, string b);
bool (*pf)(string, string);
```
尾置返回类型，在参数列表后指定的返回类型：
```
auto f1(int) -> int (*) (int*, int);
```

##7.1
* 把const关键字放在成员函数的参数列表之后，如下，此时表示this是一个指向常量的指针
```
std:string Sales_data::isbn(const Sales_data *const this)
```

##7.2
* 使用class和struct唯一的区别就是默认访问权限，struct是public，class是private
类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元
* 任何成员函数都可以改变可变数据成员的值
* 提供一个类内初始值时必须以符号=或者花括号表示

##7.3
* 返回引用可以像下列用法
```
myScreen.move(4,0).set('#')
```
如果返回的不是引用，set是无法成功的，因为他修改的是拷贝值
* 一个类的成员不能是它自己，但是可以是自己的指针或者引用
* 友元可以声明一个类，也可以声明一个函数。如果要把重载函数定义为友元，需要对这组函数中的每一个分别声明

##7.4
* 一个类就是一个作用域。在类中，如果一个成员使用了外层作用域中的名字，就不可以重新定义它
```
typedef double Money;
class Account{
public:
	Money balance() {return bal;} //使用外层作用域的Money
private:
	typedef double Money; 		  //错误，不能重新定义Money
	Money bal
}
```

##7.5
* 构造初始值列表：
```
Salas_data(const std::string &s, unsigned n, double p):
			bookNo(s), units_sold(n), revenue(p*n) { }
```
const和引用必须被初始化，不能赋值

* 如果构造函数只接受一个实参，则它实际上定义了转换为此类类型的隐式转换机制，有时候我们称之为**转换构造函数**。但是只允许一步转换。
```
// 错误：需要用户定义的两种转换：
// （1）把"9-999-99999-9"转换成string
// （2）再把这个临时的string转换成Sales_data;
item.combine("9-999-99999-9");
// 如果换成item.combine(string("9-999-99999-9"))则正确;
```
可以将构造函数声明为 `explicit` 阻止隐式转换，不能用于拷贝形式的初始化过程，用explicit声明构造函数时只能以直接初始化的形式使用

* `聚合类` 使得用户可以直接访问其成员，并且具有特殊的初始化语法形式，当一个类满足以下条件时我们称它为聚合的：
    1. 所有成员都是public的
    2. 没有定义任何构造函数
    3. 没有任何类内初始值
    4. 没有基类，也没有viitual函数
我们可以提供一个花括号括起来的成员初始值列表，并用它初始化聚合类的数据成员：
```
struct Data {
	int ival;
	string s;
}
// vall.ival = 0; vall.s = string("Aana")
Data vall = {0, "Aana"};
```
初始值的顺序必须与声明一致

* 数据成员都是字面值类型的聚合类是字面值常量类。如果一个类不是聚合类，但是它符合下述要求，那么它也是一个聚合类：
    1. 数据成员都必须是字面值类型；
    2. 类必须至少含有一个 `constexpr` 构造函数
    3. 如果一个数据成员含有类内初始值，则内置数据成员的初始值必须是一条常量表达式；或者如果成员属于某种类类型，则初始值必须使用成员自己的constexpr构造函数。
    4. 类必须使用析构函数的默认定义，该成员负责销毁类的对象

