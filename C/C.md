# C
## 函数内存分配
![](http://book.51cto.com/files/uploadimg/20081108/2059580.jpg)

## Linux下pthread使用注意注意
* 由于pthread 库不是 Linux 系统默认的库，连接时需要使用静态库 libpthread.a，所以在使用pthread_create()创建线程，以及调用 pthread_atfork()函数建立fork处理程序时，在编译中要加 `-lpthread`参数。

## 双缓冲问题：
* 双缓冲可以解决cmd输出整个屏幕时闪屏的问题，参考[双缓冲控制台应用程序输出“闪屏”（Windows） ](http://www.zmland.com/forum.php?mod=viewthread&tid=133)
