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