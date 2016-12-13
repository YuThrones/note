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
**注意：**要传入C中的char\*类型，必须使用ctypes里面的**c\_char\_p**，并且，其**输入参数应为byte**，即需要把str转成byte，使用`str.encode(encoding="utf-8")`