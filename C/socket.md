# socket编程

## 简介
* 所谓协议族（Protocol Family），就是一组协议（多个协议）的统称。最常用的是 TCP/IP 协议族，它包含了 TCP、IP、UDP、Telnet、FTP、SMTP 等上百个互为关联的协议，由于 TCP、IP 是两种常用的底层协议，所以把它们统称为 TCP/IP 协议族。

* 计算机之间有很多数据传输方式，各有优缺点，常用的有两种： `SOCK_STREAM ` 和 ` SOCK_DGRAM` 。有可能多种协议使用同一种数据传输方式，所以在 socket 编程中，需要同时指明数据传输方式和协议。

* **优雅的断开连接--shutdown()**  
默认情况下，close()/closesocket() 会立即向网络中发送FIN包，不管输出缓冲区中是否还有数据，而shutdown() 会等输出缓冲区中的数据传输完毕再发送FIN包。也就意味着，调用 close()/closesocket() 将丢失输出缓冲区中的数据，而调用 shutdown() 不会。

* **代码中使用htonl(INADDR_ANY)来自动获取IP地址。**  
利用常数 INADDR_ANY 自动获取IP地址有一个明显的好处，就是当软件安装到其他服务器或者服务器IP地址改变时，不用再更改源码重新编译，也不用在启动软件时手动输入。而且，如果一台计算机中已分配多个IP地址（例如路由器），那么只要端口号一致，就可以从不同的IP地址接收数据。

* GET和POST请求参考
[HTTP请求参考](http://blog.csdn.net/jinchaoh/article/details/47438911) 

* 使用select控制超时
[select函数详解](http://www.cnblogs.com/renyuan/p/5100184.html) 