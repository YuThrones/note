## 1.9  
	from math import floor向下取整  
	ceil 向上取整  
	pow 次方  
	cmath能处理复数  

## 1.10
* 可执行文件以.py结尾  
* Ctrl+F5运行  
* input("Press <enter>")  
### 注释
str转换字符串 repr创建字符串

### 1.11
* 可以使用''' 或者"""来使用跨多行的字符串，也可以在一行的末尾用\来使用跨多行字符串  
* r''格式的字符串不会进行转义  
* u''是unicode字符串  

### 2.1
* 序列中最后一个元素下标可以为-1  
* 列表不能作为字典的键，但元组可以  
* 列表格式为 [a,b]  
* 容器主要分为序列（例如列表和原则）和映射（例如字典），既不是序列也不是映射的有集合（set）  
eg.  fourth = raw_input('Year: ')[3]

### 2.2
* 分片 eg.
* tar='abc'
* tar[1:2]  
前闭后开
[1:]
从开始到最后
第一个下标比第二个后出现返回空
**tar[1:2:1]**
最后一个为步长
步长为负数则从右向左提取元素，此时开始索引必须大于结束索引
相同类型的序列才能进行连接操作
乘法会使序列重复N遍
none可以作为空的元素
a in tag检验一个值是否在序列中

### 2.3
list('Hello')根据String创建列表 

a[1]=10  

为列表中元素赋值 

del a[1]删除列表中元素

a[1:3]='23333'可以用不等长的序列替换 

a[1:1]='1231'可以直接插入新元素 

a.count(1)统计某个元素出现次数 

a.extend(b)直接用b列表拓展a列表 

len(a)获得a列表元素个数

a.index(1)获得元素第一个匹配的索引

a.insert(1,'a')

a.pop(1)移除并返回值

a.remove('a')移除第一个匹配项

a.reverse()反向存储

a.sort

y=x只是让x,y指向同一个列表，不是复制

sorted(x)返回x排序后的副本

a.sort(key=len,reverse=true，cmp=mycmp)key指定用于排序的键，cmp指定排序函数

### 2.4
(1,)创建只有一个值的元组
tuple('abc')以一个序列为参数将他转化为元组

### 3.1
字符串都是不可变的

eg.
format = "%s %s"
values=("Hello" "World")
print format % values
用%%输出%

### 3.3
%-010.2f宽度10，精度2，用0填充，左对齐
*表示数值从元祖参数中读取
+无论正负都标示出符号

### 3.4
find返回找到的最左端的索引，找不到返回-1
a.find（'a',1,3）可以提供起始点
join可以把队列中元素连接成字符串，与split相反
strip去除两段空格，也可以带参数指定去除字符
translate用于转化字符串

### 4.1
a={'alice':'12','bob':'22'}
创建字典

### 4.3
可以用dict函数为其他映射或者序列创建字典，也可以通过关键字参数创造
eg.
item =[('name', 'Gumby'), ('age', 42)]
d=dict(item)

d=dict(name='Gumby', age=42)
deepcopy是完全复制
items返回字典项列表
iteritems返回迭代器对象
keys返回键列表，iterkeys返回迭代器
pop(x)返回值并删除字典项
popitem()随机删除一个字典项
update用一个字典更新另一个字典

### 5.2
x,y=y,x

### 5.4
if  :    
elif:

range(0,10,3)产生0到9，步长为3的列表
xrange功能相同，但一次创建一个数，迭代大数列时更好用

zip压缩两个列表
for index, string in enumerate(Strings):得到索引

### 5.6
[x*x for x in range(10)]
pass 空语句
exec 执行字符串中的代码
可以创造命名空间，在命名空间中执行代码
scope={}
exec 'sqrt = 1' in scope

eval 求值

### 6.3
callable(x)判断函数是否可以调用
```
def fibs(num)
    'Calculate...'
    return result
```
help（fib）能得到关于函数和他的文档字符串的信息
默认返回none

### 6.4
```
def x(a, *b)
  *收集剩余参数为一个元组
  **能收集关键字参数为字典
```
调用函数时使用*和**可以分配参数

### 6.5
vars()返回全局变量字典
globals()[]获取全局变量值

### 6.6
map(func,seq [, seq, ...])对列表中的每个元素应用函数

### 7.2
```
__metaclass = type #确定使用新式类
class Person:
    def setName(self, name): #self是传入本身
        self.name = name
    def getName(self):
        return self.name
```
函数名前加双下划线无法从外部访问
但是还是可以在前面加单下划线和雷鸣访问
class Student(Person): #继承
issubclass(a,b) #检查a是不是b的子类
a.__bases__得到基类
s.__class__查看类型
多重继承先继承的类会重写后继承的方法
hasattr(a,"setname") #检查是否存在该方法
random.choice从非空序列中随机选择元素

### 8.1
```
raise Exception引发异常
try:
    ...
except Exception:
    ...
else: #没有错误发生才会执行
    ...
finally:
    ... #必然执行
```

### 9.1
__init__是构造方法
__del__是析构方法，但具体调用时间无法确定，应该尽量减少使用
super(Kid,self)调用父类构造函数

### 9.5
size = property(getSize, setSize)
静态函数没有self参数，类方法传入cls而非self
__iter__方法返回一个迭代器
包含yield语句的函数称为生成器，每次产生一个值函数就会被冻结

### 10.1
在主程序中，__name__值为'__main__'，在导入模块中为模块名
set PYTHONPATH=%PYTHONPATH%;C:\python  #可以这样添加路径
必须包含一个名为__init__.py的文件才能当成包对待

### 10.2
__all__能看到public函数
help看到函数开头的字符串
__file__能看到文件位置


