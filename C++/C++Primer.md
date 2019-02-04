# C++ Primer

## 1.2

* `>>` 和 `<<` 返回左侧对象。

## 2.2

* extern int i 声明i而非定义，在外部使用
：：全局作用域

* 初始化一个变量有以下几种方法:
  ```c++
  int units_sold = 0;
  int units_sold = {0};
  int units_sold{0};
  int units_sold(0);
  ```
  作为c++11标准的一部分，用花括号来初始化得到了全面应用，这种形式被称为列表初始化。

## 2.3
* &引用（Reference），给变量起另一个名字，必须初始化，无法重新绑定，不能定义引用的引用

* &取地址符

* 不能定义指向引用的指针

* ```*``` 解引用（取内容）

* ```void*``` 能存放任何类型对象的地址，但不能直接操作

* ```int *&a = b；``` a是对指针b的引用

* 要理解一个变量的类型，最简单的方法是从右到左阅读它的定义，离变量名最近的符号对变量的类型有最直接的影响。

## 2.4

* const对象必须初始化

* const默认状态下只在文件内有效

* const int &a 对常量的引用a

* 不能让一个非常量引用指向一个常量对象

* 只能用常量指针指向常量对象

* int *const a常量指针 初始化后不可改变

* 顶层const表示本身是个常量，不能改变，底层const表示指向的对象是常量，自己可以改变

## 2.5

* typedef定义类型别名

* typedef double wages //wages是double的同义词

* using SI = Sales_item //SI是Sales_item的同义词

* (C++11)auto可以在一行里声明多个变量，但是其基本数据类型必须一样

* decltype返回操作数的数据类型而不实际计算表达式的值

* decltype(f()) sum=x; //sum的类型就是函数f的返回类型

* decltype(*p)得到的是int&而非int

## 2.6
头文件保护符

```C
#ifdef 仅当变量已定义时为真
#ifndef 仅当变量为定义时为真
```

一旦检查结果为真，则执行后续操作直至遇到#endif指令为止

## 3.1

* using std::cin声明cin

* 头文件不应该包含using声明，因为会拷贝到所有被引用的文件中

* string s7(10, 'c') // “cccccccccc”

* getline(is, s)从is中读取一行赋给s

* string.zi返回的是size_type类型，无符号整型数

* string用+号必须确保两边有一个是string对象

* for(变量：对象)  例如for(auto c: str)

* 用引用可以改变string中字符的值

* C++语言中的字符串字面值并不是标准库string的对象。

## 3.3

* 老式C++标准中vector<vector<int> >必须有空格

* vector<int> ivec(10,-1)初始化10个-1

* 当花括号无法列表初始化时会尝试默认值初始化（），比如 `vector<string> v(10, "hi"); //v有10个值为"hi"的元素`

* vector可以高效添加元素增长

* 如果循环内部有向vector添加元素的语句，不能用范围for循环

* for(auto &i:v)可以通过i给v的元素赋值

* vector的下标可以用于访问已存在元素而不能用于添加元素

## 3.4

* ```*iter``` 返回迭代器iter所指元素的引用

```
iter->men等价于（*iter）.mem
vector<int>::iterator it
```

* vector<int>::const_iterator只能读取不能修改，常量对象只能用他读取

## 3.5

* a[d]  d必须是一个常量表达式

* 不能用auto推断数组类型，得到的类型是指针

* 用字符串字面值初始化字符数组会自动添加'\0'

* 不能用一个数组初始化另外一个数组或者赋值

* int (*p)[10]=&a   p指向含有10个整数的数组

* 指针也是迭代器，也可以用来访问数组

* begin，end可以操作数组得到头尾指针，例如begin(a)

* 传入strlen等函数的指针必须指向以空字符结尾的数组

* `string *p2 = nums; //等价于p2 = &nums[0]`

## 3.6

* 这个循环没有任何写操作，但是还是将外层循环的控制变量声明成了引用类型，这是为了避免数组被自动转换为指针。

  ```C++
  int ia[3][4];
  for (const auto &row: ia) 
    for (auto col:row)
      cout << col << endl;
  ```

  假设不是循环类型，下列程序将无法通过编译。这是因为row不是引用类型，编译器初始化row时会自动将数组转换为指向数组内首元素的指针，这样得到的row就是 `int*` 类型，内层循环就不合法了。

  ```C++
  for (auto row:ia)
    for (auto col:row)
  ```

## 4.1

* 当一个对象被用作右值的时候，用的是对象的值（内容）；当对象被用作左值的时候，用的是对象的身份（在内存中的位置）

## 4.5

* 尽量用++i而不用i++，因为i++会有创建副本

* ```*beg=toupper(*beg++)``` 有问题，结果未定义

## 4.9
sizeof (type) 返回一个类型名字所占字节数
sizeof expr 返回表达式结果类型的大小，并不实际计算值

## 4.11

cast-name<type>(expression)显示转换，casst-name是static_cast、dynamic_cast、const_cast、reinterpret_cast中的一种

只要不包含底层const都可以用static_cast
```
void* p = &d;
double *dp=static_cast<double*>(p);可以召回存在于void指针中的值
```
const_cast只能改变对象的底层const

reinterpret_cast本质上依赖于机器，非常危险


## 4.12
运算符优先级表

## 5.5
goto不能跳过变量定义	
					
## 5.6
throw引发异常
try处理语句块处理异常，以一个或多个catch结束
exception class用于在throw跟catch之间传递异常具体信息
throw runtime_error是标准库异常类型的一种
execption.what()返回一个C字符串

## 6.1
函数返回类型不能是数组和函数，但是可以是指向他们的指针
static在第一次定义时初始化，直到程序终止

## 6.2
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

## 6.3
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

## 6.4
* 如果一个作用域内的几个函数名字相同但是参数列表不同，我们称之为重载函数。

## 6.5
可以重复声明一个函数，但是不可以修改已经声明的默认参数值
在函数的返回类型前面加强关键字```inline``` ，可以把函数声明为内联函数，消除运行时开销
```inline const string& func()```
constexpr函数指能用于常量表达式的函数,函数的返回值和所有形参的类型都得是字面值类型

## 6.7
要声明一个可以指向函数的指针，只需要把函数名替换成指针：
```
bool com(string a, string b);
bool (*pf)(string, string);
```
尾置返回类型，在参数列表后指定的返回类型：
```
auto f1(int) -> int (*) (int*, int);
```

## 7.1
* 把const关键字放在成员函数的参数列表之后，如下，此时表示this是一个指向常量的指针
```
std:string Sales_data::isbn(const Sales_data *const this)
```

## 7.2
* 使用class和struct唯一的区别就是默认访问权限，struct是public，class是private
类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元
* 任何成员函数都可以改变可变数据成员的值
* 提供一个类内初始值时必须以符号=或者花括号表示

## 7.3
* 返回引用可以像下列用法
```
myScreen.move(4,0).set('#')
```
如果返回的不是引用，set是无法成功的，因为他修改的是拷贝值
* 一个类的成员不能是它自己，但是可以是自己的指针或者引用
* 友元可以声明一个类，也可以声明一个函数。如果要把重载函数定义为友元，需要对这组函数中的每一个分别声明

## 7.4
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

## 7.5

* 构造初始值列表：

```C
Salas_data(const std::string &s, unsigned n, double p):
    bookNo(s), units_sold(n), revenue(p*n) { }
```

* const和引用必须被初始化，不能赋值

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

## 7.6 类的静态成员
* 静态成员函数不能声明成const，我们使用作用与运算符 `::` 直接访问静态成员，也可以使用域的对象，指针或者引用来访问。
```
doublr r;
r = Account::rate();
Account ac1;
Account *ac2 = &ac1;
r = ac1.rte();
r = ac2->rate();
```
成员函数不通过作用域运算符就能直接使用静态成员

* 静态成员可以在类内部或者外部定义，在外部定义时不能重复static关键字，一般不能在类的内部初始化静态成员。
```
//定义并初始化一个静态成员
double Account::interestRate = initRate();
```

*静态成员可以声明为类类型，而普通成员只能是类的引用或者指针

## 8.1
* iostream定义了用于读写流的基本类型，fstream定义了读写命名文件的类型，sstream定义了读写内存string对象的类型。
  1. istream 从流读取数据
  2. ostream 向流写入数据
  3. iostream 读写流
  4. ifstream 从文件读取数据
  5. ofstream 向文件写入数据
  6. fstream 读写文件
  7. istringstream 从string读取数据
  8. ostringstream 向string写入数据
  9. stringstream 读写string
  10. 加一个w开头的用于读写宽字符版本（wchar_t）
* getline(stream, line)读取行放到line中
* 不能拷贝或者对IO对象赋值
* IO库条件状态：
    1. strm::iostate   strm是一种IO类型，iostate是一种机器相关的类型，提供了表达条件状态的完整功能。
    2. strm::badbit    指示流已崩溃 
    3. strm::failbit   指出一个IO操作失败了
    4. strm::eofbit    指出流到达了文件结束
    5. strm::goodbit   指出流未处于错误状态，此值保证为0
    6. s.eof()         若eofbit置位，则返回true
    7. s.fail()
    8. s.bad()
    9. s.good()
    10. s.clear()      将所有条件状态位复位，将流的状态设置为有效，返回void
    11. s.clear(flags) 复位指定的条件状态位
    12. s.setstate(flags) 置位指定条件状态位
    13. s.rdstate(）    返回当前流的条件状态
* 输出flush和endl会刷新缓存区，区别是flush不换行。输出 `unitbuf` 则以后每次写操作都会刷新缓冲区， 输出 `nounitbuf` 则重置流，使其恢复使用正常的缓冲区刷新机制。
* **注：如果程序崩溃，输出缓冲区不会被刷新**
* 任何试图从输入流读取数据的操作都会先刷新关联的输出流。tie有两个版本，一个不带参数，返回指向输出流的指针。第二个接受一个指向ostream的指针，将自己关联到ostream。即，x.tie(&o)将流关联到输出流o。每个流最多同时关联到一个流，但多个流可以同时关联到一个ostream。

## 8.2
* 特有操作：
    1. fstream fstrm；    创建一个未绑定的文件流
    2. fstream fstrm(s);  创建一个fstream并打开名为s的文件，s可以是string类型。
    3. fstream fstrm（s， mod）； 与前一个构造函数想似，但按指定mode打开文件
    4. fstrm.open(s)      打开文件s
    5. fstrm.close()      关闭绑定的文件，返回void
    6. fstrm.is_open()    查看是否成功打开
* 文件模式：
    1. in  以读方式打开
    2. out 以写方式打开
    3. app 每次写操作均定位到文件末尾
    4. ate 打开文件后立即定位到文件末尾
    5. trunc 截断文件
    6. binary 以二进制方式进行IO
以out打开文件会丢弃已有数据

## 8.3 string流
* 特有操作：
    1. sstream strm；
    2. sstream strm（s）
    3. strm.str() 返回strm保存的string的拷贝
    4. strm.str9(s) 将string s拷贝到strm中，返回void
* 利用string转换double等：  
```
#include <iostream>  
#include <sstream>    //使用stringstream需要引入这个头文件  
using namespace std;  
//模板函数：将string类型变量转换为常用的数值类型（此方法具有普遍适用性）  
template <class Type>  
Type stringToNum(const string& str)  
{  
    istringstream iss(str);  
    Type num;  
    iss >> num;  
    return num;      
}  
int main(int argc, char* argv[])  
{  
    string str("00801");  
    cout << stringToNum<int>(str) << endl;  
  
    system("pause");  
    return 0;  
}
```

## 9.2
* C seq(n) seq包含n个元素，进行了值初始化
  C seq(n,t) seq包含n个初始化为t的元素
* 标准库array具有固定大小
  `array<int, 42> s;`
  array类型不支持assign
* swap操作可以交换两个容器的内容
`swap(vec1, vec2)`
* 容器中的元素是拷贝，而不是对象本身，改变容器中元素的值不会改变原对象
* emplace类似于insert，但是它是构造而非拷贝元素，调用构造函数
* 访问元素的成员函数返回的是引用，const容器返回const的引用，如果不是const，可以用来改变元素的值。可以使用at（）函数访问下标

## 9.3
* 删除操作：
  c.erase(p) 删除p指向的元素
  c.erase(b, e) 删除b到e范围元素，返回最后一个被删除元素之后元素的迭代器
* forward_list中添加或删除元素的操作是通过改变给定的元素之后的元素来实现的，定义的行为有insert_after,emplace_after和erase_after。forward_list 定义了一个before_begin首前迭代器
* resize可以增大或者缩小容器

## 9.4
* capacity是在不分配新的内存空间的前提下它最多可以保存多少元素，size是它已经保存的元素数目
  reserve（n）分配至少能容纳n个元素的空间
  shrink_to_fit()只适用于vector，string和deque，退回不需要的内存空间（未必成功），减少为与size（）相同
  capacity和reserve只适用于vector和string

## 9.5 string操作
* string s(cp, n) 拷贝cp指向的数组前n个元素
* string s(s2, pos2) 从s2的pos位置开始拷贝
* string s(s2, pos2, len2) 从s2的pos2开始拷贝len2个拷贝
* s.substr(pos, len)
* s.insert(pos, len, char); 在pos位置插入len个char符号
* s.erase(pos, len) 从pos开始删除len个符号
* replace调用erase和insert
* string搜索函数返回unsigned的string::size_type，找不到则返回-1：
    1. find（args）  寻找第一次出现的下标
    2. find_first_of（args） 查找与给定字符串中任何一个字符匹配的位置
    3. find_first_not_of（args） 查找第一个不在给定字符串中的位置
    4. rfind（args） 找到最后一次出现的下标
    5. find_last_of（args）
    6. find_last_not_of（args）
* args必须有下列形式：
    1. c, pos 从s中pos位置开始查找，pos默认为0
    2. s2, pos
    3. cp, pos
    4. cp, pos, n 从s中位置pos开始查找cp指向的数组的前n个字符，pos和n无默认值
* s.compare函数能比较字符串是大于、小于还是等于
* 新标准引入了多个函数，可以实现数值数据与标准库string之间的转换：
```C
int i = 42;
string s = to_string(i);
double d = stod(s);
```
  b表示转换用的基数，p保存第一个非数值字符的下标，默认为0
  返回类型分别是int，long，unsigned long, long long, unsigned long long,float, double, long double
  1. stoi(s, p, b)
  2. stol(s, p, b)
  3. stoul(s, p, b)
  4. stoll(s, p, b)
  5. stoull(s, p, b)
  6. stof(s, p)
  7. stod(s, p)
  8. stold(s, p)
  
## 9.6 容器适配器（adaptor）
* 定义了三个顺序容器适配器：stack，queue和priority_queue。本质上，适配器是一种机制，能使某种事物的行为看起来像另外一种事物。一种容器适配器接受一种已有的容器类型，使其行为看起来像一种不同的类型。例如一个stack接受一个顺序容器（除array或forward_list外），并使其操作看起来像stack一样。
* 适配器支持的操作：
    1. size_type
    2. value_type
    3. container_type
    4. A a; 创建一个名为a的适配器
    5. A a(c); 创建适配器a带有c的拷贝
    6. 关系运算符 ==、！=等
    7. a.empty()
    8. a.size()
    9. swap(a, b)
    10. a.swap(b)
```
stack<int> stk(deq);
stack<string, vector<string>> str_stk;
//可以在创建时将一个命名的顺序容器作为第二个类型参数
```

* 栈默认基于deque实现，也可以在list或者vector之上实现，操作包括：
  1. s.pop()
  2. s.push(item)
  3. s.emplace(args)
  4. s.top()
* queue默认基于deque实现，priority_queue默认基于vector
  queue可以用list或者vector实现，priority_queue也可以用deque实现，操作包括：
  1. q.pop()
  2. q.front()
  3. q.back()
  4. q.top()
  5. q.push(item)
  6. q.emplace(args)

## 10.1 泛型算法概述
* 大多数算法都定义在头文件algorithm中，标准库还在头文件numeric定义了一组数组泛型算法
* 标准库算法find可用于搜索容器
* 泛型算法本身不会执行容器的操作，它们只运行于迭代器之上

## 10.2
* 除了少数例外，标准库算法都对一个范围内的元素进行操作，我们将此元素的范围称为“输入范围”，两个输入元素分别是第一个元素和尾元素之后位置的迭代器。
* 只读算法，值读取范围内的元素，不改变元素。例如find，还有定义在numeric之中的accumulate
  输入范围和和的初始值，求范围内的和，此外还有equal算法
* 写容器元素的算法：
  fill接受一个范围和一个值，将一个子序列全部设置为给定值，必须在足够大的容器上面操作，或者使用插入迭代器back_inserter进行迭代
```
vector<int> vec;
fill_n(back_inserter(vec), 10, 0)
```
* 拷贝算法
  copy接受输入范围跟目的序列起始位置，目的序列至少要包含跟输入序列一样多的元素
  replace接受四个参数，分别是输入范围，要搜索的值和要替换的值
```
//将所有值为0的元素改成42
replace(list.begin(), list.end(), 0, 42)
```

* 重排容器元素的算法
  sort算法是利用元素类型运算符<实现
  要消除重复元素，可以将vector排序，使重复的单词相邻出现，然后用标准库算法unique重排vector，由于算法不能执行容器的操作，需要erase成员来完成真正的删除操作
```
sort(words.begin(), words.end());
auto end_unique = unique(words.begin(), words.end());
words.erase(end_unique, words,end())
```

## 10.3 定制操作
* 很多算法会比较输入序列中的元素，默认情况下，使用元素类型的<或者==来完成比较，标准库允许我们提供自己定义的操作来替代默认运算符。
* 我们可以使用sort的第二个版本，它接受第三个参数，这个参数是一个**谓词（predicate）**。谓词是一个可调用的表达式，其运算结果是返回一个能用做条件的值。标准库算法使用的谓词分为两类：一元谓词（unary predicate）和二元谓词（binary predicate），代表参数的个数
* 在我们将words大小重排的同时，还希望具有相同长度的元素按字典序排列。为了保持相同长度的单词按字典序排列，可以使用stable_sort算法，这种稳定排序算法维持相等元素的原有顺序。
* lambda表达式具有如下形式
  `[capture list](parameter list) -> return type { function body}`
其中，capture list(捕获列表)是一个lambda所在函数中定义的局部变量的列表（通常为空）； return type、 parameter list和function body与任何普通函数一样，分别表示返回类型，参数列表和函数体。与普通函数不同，lambda**必须使用尾置返回**来指定返回类型。我们可以忽略参数列表和返回类型，但必须**永远包括捕获列表和函数体**，捕获列表只用于局部非static变量。
* for_each算法接受一个可调用对象，并对输入序列中每个元素调用此对象
* find_if寻找第一个满足条件的位置
* 引用捕获必须保证在lambda执行时变量是存在的，[=]是值捕获，[&]是引用捕获，必须是捕获列表中第一个符号，定义默认捕获方式，可以采用混合捕获，如 `[&, c]` 或者 `[=, &c]` 
  如果一个lambda包含return之外的任何语句，编译器假定此lambda返回void
* transform算法接受三个迭代器和一个可调用对象。前两个代表输入序列，第三个表示目的位置，transform将输入序列中每个元素替换为可调用对象操作该元素得到的结果。
* functional头文件中的bind标准库函数可以看作一个通用的函数适配器。它接受一个可调用对象，生成一个新的可调用对象来“适应”原对象的参数列表。一般调用形式为：
 `auto newCallable = bind(callable, arg_list);`
  其中，newCallable本身是一个可调用对象，arg_list是一个逗号分隔的参数列表，对应给定的callable的参数。当我们调用newCallable时，newCallable会调用callable，并传递给它arg_list中的参数。
  arg_list中的参数可能包含形如_n的名字，其中n是一个整数。这些参数是“占位符”，表示newCallable的参数，占据了传递给newCallable的参数的“位置”。数值n表示生成的可调用对象中参数的位置。
  例如 `auto g = bind(f, a, b, _2, c, _1);` 调用g(_1, _2)可以映射成 f(a, b, _2, c, _1) 
  使用placeholders名字，名字_n都定义在这个命名空间中，这个命名空间本身定义在std命名空间内，所以两个都要写上。bind拷贝参数，如果我们希望传递给bind一个对象而又不拷贝它，就必须使用ref函数

## 10.4
* 迭代器包括以下几种：
    1. 插入迭代器（insert iterator） 绑定到一个容器上，可以用来向容器插入元素
        * it = t，根据插入迭代器的不同种类，会分别调用push_back,push_front或者insert
        * *it,++it,it++这些操作虽然存在，返回的都是it，不会做任何事情
        * 插入器有三种，back_inserter,front_inserter, inserter
    2. 流迭代器（stream iterator） 绑定到输入流或者输出流上，可用来遍历所有关联的IO流
        * 流迭代器有如下用法：
        ```
		istream_iterator<int> in_iter(cin), eof; 从cin读取int，用空的迭代器eof来作尾后迭代器
		vector<int> vec(in_iter, eof); 从迭代器范围构造vec
        ```
		* 还可以使用算法
		```
		istream_iterator<int> in(cin),eof;
		cout << accumulate(int, eof, 0) << endl;
		```
		* 只要定义了>>或者<<就可以创建输入流对象或者输出流对象
    3. 反向迭代器（reverse iterator） 这些迭代器不是向前而是向后移动
    4. 移动迭代器（move iterator） 这些迭代器不是拷贝其中的元素，而是移动他们

## 10.5 泛型算法结构
* 算法要求的迭代器分为5个级别：
	1. 输入迭代器       只读，不写；单边扫描，只能递增
	2. 输出迭代器       只写，不读；单边扫描，只能递增
	3. 前向迭代器       可读写；多遍扫描，只能递增
	4. 双向迭代器       可读写；多遍扫描，可递增递减
	5. 随机访问迭代器   可读写，多遍扫描，支持全部迭代器运算

## 10.6 特定容器算法
* 链表类型定义的其他算法的通用版本可以用于链表，但是成本太高，应该优先使用成员函数版本的算法而不是通用算法。
* list和forward_list成员函数版本的算法
    1. lst.merge(lst2)       将来自lst2的元素合并入lst，合并后lst2为空，使用<运算符
    2. lst.merge(lst2, comp)
    3. lst.remove(val)
    4. lst.remove_if(pred)
    5. lst.reverse()
    6. lst.sort()
    7. lst.sort(cmp)
    8. lst.unique()          调用erase删除同一个值的连续拷贝，第一个版本使用==
    9. lst.unique(pred)      第二个使用给定的二元谓词
* splice算法，这是链表数据结构所特有的，不需要通用版本
	1. (p, lst2)   p是一个指向lst中元素的迭代器，将lst2中的元素全部删除，然后添加到p的位置
	2. (p, lst2, p2) 
	3. (p, lst2, b, e)
	
## 11.1
* 主要关联容器：map和set。multi开头的表示允许重复。从map提取一个元素时，会得到一个pair类型的对象，这是一个模板类型，保存一个名为first和second的（公有）数据成员。
* find调用返回一个迭代器，找不到则返回尾后迭代器

## 11.2
* 默认情况下，标准库使用关键字类型的<运算符比较两个关键字，可以向算法提供我们定义的比较操作，所提供的的操作必须在关键字类型上定义一个**严格弱序（strict weak ordering）**，必须具有以下性质：
	1. 两个关键字不能同时“小于等于”对方；
	2. 如果k1小于等于k2<,且k2小于等于k3，那么k1必须小于等于k3
	3. 如果存在两个关键字，任何一个都不小于等于另一个，我们称这两个关键字是等价的
  在定义set时，应该提供两个类型：关键字类型以及比较操作类型——如果是一种函数指针类型，例：
`multiset<Sales_data, decltype(compareIsbn)*>   bookstore(compareIsbn);`
* pair定义在头文件utility中，一个pair保存两个数据成员，pair的默认构造函数对数据成员进行值初始化，我们也可以为每个成员提供初始化器，pair的操作如下：
	1. pair<T1, T2> p;
	2. pair<T1, T2> p(v1, v2)
	3. pair<T1, T2> = {v1, v2};
	4. make_pair(v1, v2)
	5. p.first
	6. p.second
	7. p1 relop p2 
	8. p1 == p2 利用元素的<运算符来实现
	9. p1 != p2

## 11.3 关联容器操作
* 关联容器定义的类型：
  * key_type 关键字类型
  * mapped_type 每个关键字关联的类型，只适用于map
  * value_type，对于set与key_type相同，对于map为pair<const key_type, mapped_type>
* 添加元素，使用insert函数，可以添加一个元素或一个元素范围。insert的返回值依赖于容器类型和参数，对于不含重复关键字的set和map，insert和emplace返回一个pair，first成员是一个迭代器，指向具有给定关键字的元素，second是一个bool值，指出元素是插入成功还是已经在容器中。
* 通过erase来进行删除，可以传递一个迭代器，或者一个迭代器范围，或者一个key_type参数，返回删除的元素的数量
* 可以使用下标运算符或者at()函数读取map，at（）会进行参数检查，如果不在map中会抛出out_of_range异常，下标和at（）操作只适用于非const的map
* 查找元素的操作：
	1. c.find(k)
	2. c.count(k) 返回关键字等于k的元素的数量
	3. c.lower_bound(k) 返回一个迭代器，指向第一个关键字不小于k的元素
	4. c.upper_bound(k) 返回一个迭代器，指向第一个关键字大于k的元素
	5. c.equal_range(k) 返回一个迭代器的pair，表示关键字等于k的元素的范围。若k不存在，pair的两个成员等于c.end()

## 11.4 无序容器
* 无序容器不是使用比较运算符来组织元素，而是使用一个哈希函数和关键字类型的==运算符。
* 用于map和set的操作能用于unordered_map和unordered_set。无序容器的性能依赖于哈希函数的质量和桶的数量和大小。
* 无序容器的成员函数：
  * 桶接口：
	1. c.bucket_count() 正在使用的桶的数目
	2. c.max_bucket_count() 容器能容纳的最多的桶的数量
	3. c.bucket_size(n) 第n个桶中有多少个元素
	4. c.bucket(k) 关键字为k的元素在哪个桶中
  * 桶迭代：
    1. local_iterator 可以用来访问桶中元素的迭代器类型
    2. const_local_iterator 桶迭代器的const版本
    3. c.begin(n),c.end(n) 桶n的首元素迭代器和尾后迭代器
    4. c.cbegin(n),c.cend(n) 返回const版本
  * 哈希策略：
    1. c.load_factor() 每个桶的平均元素数量，返回float值
    2. c.max_load_factor() c试图维护的平均桶大小，返回float值
    3. c.rehash(n) 重组存储，使得bucket_count>=n且bucket_count > size/max_load_factor
    4. c.reserve(n) 重组存储，使得c可以保存n个元素且不必rehash

* 默认情况下，无序容器使用关键字类型的==运算符来比较元素，它们还使用一个hash<key_type>类型的对象来生成每个元素的hash值。标准库为内置类型（包括指针）提供了hash模板。但是我们**不能直接定义**关键字是自定义类类型的无序容器，不能直接使用哈希模板，而必须提供我们自己的hash模板版本。

## 12.1
* 全局对象在程序启动时分配，在程序结束时销毁。对于局部自动对象，当我们进入其定义的程序块时被创建，在离开块时销毁。局部static对象在第一次使用前分配，在程序结束时销毁。
* new为对象分配内存空间，delete销毁对象
* 新标准库提供了两种**智能指针（smart pointer）**类型来管理动态对象，其行为类似于常规指针，区别是它负责自动释放对象。**shared_ptr**允许多个指针指向同一对象，**unique_ptr**则独占所指向的对象。标准库还定义了一种名为**weak_ptr**的伴随类，它是一种弱引用，指向shared_ptr所管理的对象。
* 类似vector，智能指针也是模板，当我们创建时必须提供额外信息，指针指向的类型，默认初始化的智能指针保留一个空指针。
* shared_ptr和unique_ptr都支持的操作：
	1. shared_ptr<T> sp 空智能指针，可以指向类型为T的对象
	2. unique_ptr<T> up
	3. p 将p用作一个条件判断，若p指向一个对象，则为true
	4. *p 解引用p，获得它指向的对象
	5. p->mem 等价于（*p）.mem
	6. p.get() 返回p中保存的指针。要小心使用，若智能指针释放了其对象，返回的指针所指向的对象也就消失了。
	7. swap(p, q) 交换q和p中的指针
	8. p.swap(q)
* shared_ptr独有的操作：
	1. make_shared<T>(args) 返回一个shared_ptr，指向一个动态分配的类型为T的对象，并且用args初始化此对象
	2. shared_ptr<T>p (q) p是shared_ptr q的拷贝，此操作会递增q中的计数器。q中的指针必须转换为T*
	3. p=q p和q都是shared_ptr,所保存的指针必须能相互转换。此操作会递减p的引用计数，递增q的引用计数，递增q的引用计数；若p的引用计数变为0，则将其管理的原内存释放
	4. p.unique() 若p.use_count()为1，返回true，否则返回false
	5. p.use_count() 返回与p共享对象的智能指针变量；可能很慢，主要用于调试

* 最安全的分配和使用动态内存的方式是调用一个名为make_shared的标准库函数，此函数在动态内存中分配一个对象并初始化他，返回指向此对象的shared_ptr。与智能指针一样，make_shared也定义在头文件memory中。
```
//指向一个值为42的int的shared_ptr
shared_ptr<int> p3 = make_shared<int>(42)
```

* 程序使用动态内存的三个原因：
	1. 程序不知道自己需要使用多少对象
	2. 程序不知道所需对象的准确类型
	3. 程序需要在多个对象间共享数据

* 内存耗尽
一旦一个程序用尽了它所有可用的内存，new表达式就会失败，默认情况下，如果new不能分配所要求的内存空间，它会抛出一个类型为bad_alloc的异常，我们可以改变new的方式来阻止它抛出异常：
```
int *p1 = new int; //如果分配失败，new抛出std::bad_alloc
int *p2 = new (nothrow) int; //如果分配失败，new返回空指针
```
这种形式称为**定位new（placement new）**

* 释放一块非new分配的内存，或者将相同的指针释放多次，其行为是未定义的

* 定义和改变shared_ptr的其他方法：
	1. shared_ptr<T> p(q) p管理内置指针q所指向的对象；q必须指向new分配的内存，且能够转换成T*类型
	2. shared\_ptr<T> p(u) p从unique\_ptr u那里接管了对象的所有权；将u置为空
	3. shared\_ptr<T> p(q,d) p接管了内置指针q所指向的对象的所有权。q必须能够转换为T*类型。p将使用可调用对象d来代替delete
	4. shared_ptr<T> p(p2,d) p是shared_ptr p2的拷贝，唯一的区别是p将调用可调用对象d来替代delete
	5. p.reset() 若p是唯一指向其对象的shared_ptr,reset会释放此对象
	6. p.reset(q) 若传递了可选的参数内置指针q，会令p指向q，否则会将p置为空
	7. p.reset(q, d) 若还传递了参数d，将会调用d而不是delete

* 不能使用get初始化另一个智能指针或为智能指针赋值
* 可以使用智能指针管理连接，来保证资源的释放
```
void f(destination &d /*其他参数*/)
{
	connection c = connect(&d);
	shared_ptr<connection> p(&c, end_connection);
	//使用连接
	//当f退出时（即使是由于异常而退出），connection会被正确关闭
}
```

* 智能指针基本规范：
	1. 不适用相同的内置指针值初始化（或reset）多个智能指针
	2. 不delete get()返回的指针
	3. 不适用get（）初始化或者reset另一个智能指针
	4. 如果使用get（）返回的指针，当最后一个对应的智能指针销毁后我们的指针就无效了
	5. 如果使用的智能指针管理的资源不是new分配的内存，记住传递给他一个删除器

* unique_ptr必须使用直接初始化形式

* weak_ptr是一种不控制所指向对象生存期的智能指针，它指向一个由shared_ptr管理的对象，并且不会改变shared_ptr的引用计数。

## 12.2 动态内存
* 标准库中包含一个allocator的类，允许我们将分配和初始化分离，使用allocator通常会提供更好的性能和更灵活的内存管理能力。