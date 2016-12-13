#python基础
1. input和print
```
name = input("Please input your name:\n")
print("Hello,", name)
```

2. 处理文件
```
f = open('poem.txt', 'w') #open for 'w'riting
f.write(poem)             #write text to file
f.close() 
```

3. Python调用C或者C++
使用ctypes，[参考](http://blog.csdn.net/joeblackzqq/article/details/10431733) 
