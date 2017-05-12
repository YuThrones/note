# 爬虫资料与笔记
## 爬虫参考教程
[Python爬虫学习笔记](http://cuiqingcai.com/1052.html)
* 模仿POST登录的范例
```python
import urllib2
 
values = {"username":"1016903103@qq.com","password":"XXXX"}
data = urllib.urlencode(values) 
url = "https://passport.csdn.net/account/login?from=http://my.csdn.net/my/mycsdn"
request = urllib2.Request(url,data)
response = urllib2.urlopen(request)
print response.read()
```
可以尝试输出data，可以看到是一个可以用于GET方式的字符串

* 有些服务器会识别头部以决定是否响应，可能需要设置User-Agent和Referer等。
* 代理服务器用法如下：
```python
import urllib2
enable_proxy = True
proxy_handler = urllib2.ProxyHandler({"http" : 'http://some-proxy.com:8080'})
null_proxy_handler = urllib2.ProxyHandler({})
if enable_proxy:
    opener = urllib2.build_opener(proxy_handler)
else:
    opener = urllib2.build_opener(null_proxy_handler)
urllib2.install_opener(opener)
```
* URLError可能产生的原因：
  1. 网络无连接，即本机无法上网
  2. 连接不到特定的服务器
  3. 服务器不存在
* 可以使用`cookielib`里面的函数跟类来满足cookie的使用需求，可以用`MozillaCookieJar`将cookie保存到文件中。
* 高级的的使用需求可以使用requests库，具体使用方法查看文档
* 可以使用 beautiful soup来进行html和xml的解析，没必要写复杂易错的正则表达式

* 要在requests库中保持一个对话，可以使用requests.Session()。

## pyspider
* 在遇到ＨＴＴＰ　５９９的时候，最简单的解决方法是在 crawl 方法中加入忽略证书验证的参数，validate_cert=False，即
`self.crawl(url, callback=method_name, validate_cert=False)`，
详细可以参照[PySpider HTTP 599: SSL certificate problem错误的解决方法](http://cuiqingcai.com/2703.html)
* 在运行pyspider的时候如果遇到`No module named xmlrpc_server` 的问题，可以重新安装six模块，使用 `pip install -U six` 命令来解决。

## python多进程
由于Python的多线程同时只能有一个线程在执行，python的多进程功能比多线程强大太多，因此建议使用多线程使得程序并行执行，参考[多进程用法](http://cuiqingcai.com/3335.html)
* python的多进程要使用`mutilprocessing` 库来实现，可以使用使用里面的`Process` 类或者继承它并实现它的`run` 方法来实现。
* 设置`Process` 类的`daemon` 属性如果设置为True，父进程结束后子进程会被自动终止。如果需要子进程执行完了再结束父进程，只需要加入join方法。
* 我们可以使用`Lock` 来加锁以设立临界区，使用Semaphore来做到同步和互斥，并且控制临界资源的数量。使用Semaphore.acquire()来锁定一个资源，使用Semaphore来解锁一个资源。使用`multiprocessing.Queue` 作为共享队列，在进程间共享数据。
* 我们可以使用`multiprocessing.Pipe` 来进行进程间的通信。
* `multiprocessing.Pool` 中的 `apply_async` 可以添加非阻塞进程， `apply` 可以添加阻塞进程，map方法可以对列表中的所有元素都启动一个进程并将参数传入来执行。
