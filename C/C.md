# C
## 函数内存分配
![](http://book.51cto.com/files/uploadimg/20081108/2059580.jpg)

## Linux下pthread使用注意注意
* 由于pthread 库不是 Linux 系统默认的库，连接时需要使用静态库 libpthread.a，所以在使用pthread_create()创建线程，以及调用 pthread_atfork()函数建立fork处理程序时，在编译中要加 `-lpthread`参数。