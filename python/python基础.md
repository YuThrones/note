# python基础
* input和print
```
name = input("Please input your name:\n")
print("Hello,", name)
```
* 处理文件
```
f = open('poem.txt', 'w') #open for 'w'riting
f.write(poem)             #write text to file
f.close() 
```
* Python调用C或者C++
使用ctypes，[参考](http://blog.csdn.net/joeblackzqq/article/details/10431733)   
**注意：**要传入C中的char\*类型，必须使用ctypes里面的**c\_char\_p**，并且，其**输入参数应为byte**，即需要把str转成byte，使用`str.encode(encoding="utf-8")`

* 可以itertools包下面的permutations和combinations函数分别实现排列和组合
* marshal模块可以用来将一个python数据结构写入文件
* 使用msvcrt模块可以实现命令行的无阻塞输入，使用其中的```kbhit()``` 函数来检测输入，使用```getch()``` 函数来接受单个字符输入，使用范例如下：
```python
while not msvcrt.kbhit():
    # 在没有接收到输入的时候进行的操作
    do_something
press_button = msvcrt.getch()
# 接受字符
```
