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

## Scrapy教程笔记
### 创建项目
* scrapy可以用以下命令创建项目
```
scrapy startproject projectname
```
  创建后项目包括以下的结构：
```
projectname/
    scrapy.cfg            # 项目的配置文件
    projectname/          
        __init__.py       
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
            ...
```

* item是保存爬取到的数据的容器，使用方法类似python字典，范例如下：
```python
import scrapy
class DmozItem(scrapy.Item):
    title = scrapy.Field()
    link = scrapy.Field()
    desc = scrapy.Field()
```

### 编写爬虫
* 为了创建爬虫，必须继承`scrapy.Spider` 类，且定义以下三个属性：
  * `name` 用于区别Spider，该名字必须唯一
  * `start_urls`:包含了Spider在启动时进行爬取的url列表。 因此，第一个被获取到的页面将是其中之一。 后续的URL则从初始的URL获取到的数据中提取。
  * `parse()` 是spider的一个方法。 被调用时，每个初始URL完成下载后生成的 `Response` 对象将会作为唯一的参数传递给该函数。 该方法负责解析返回的数据(response data)，提取数据(生成item)以及生成需要进一步处理的URL的`Request` 对象。

* 爬虫代码范例如下：
```python
import scrapy

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://scrapy-chs.readthedocs.io/zh_CN/0.24/intro/tutorial.html",
    ]

    def parse(self, response):
        filename = response.url.split("/")[-1]
        with open(filename, 'wb') as f:
            f.write(response.body)
```

* 要启动爬虫，需要进入项目的根目录，然后执行下列命令启动spider
```
scrapy crawl spidername
```

### 提取Item
从网页中提取数据有很多方法。Scrapy使用了一种基于 XPath 和 CSS 表达式机制: Scrapy Selectors 。 关于selector和其他提取机制的信息请参考 Selector文档 。
* XPath表达式的例子和对应的含义：
  * `/html/head/title`:选择HTML文档中 `<head>` 标签内的 `<title>` 元素
  * `/html/head/title/text()`:选择上面提到的 `<title>` 元素的文字
  * `//td`:选择所有的 `<td>` 元素
  * `//div[@class="mine"]`:选择所有具有 `class="mine"` 属性的 `div` 元素

* Selector有四个基本的方法：
  * `xpath()` 传入xpath表达式，返回该表达式所对应的所有节点的selector list列表 。
  * `css()` 传入CSS表达式，返回该表达式所对应的所有节点的selector list列表.
  * `extract()` 序列化该节点为unicode字符串并返回list。
  * `re()` 根据传入的正则表达式对数据进行提取，返回unicode字符串list列表。

* `Item` 是对象自定义的python字典。我们可以用标准的字典语法来获取每个字段的值(字段即是我们之前的Field赋值的属性)。
一般的说，Spider将会将爬取的数据以`Item` 对象返回。所以为了将爬取的数据返回，我们最终的代码将是:
```python
import scrapy
from tutorial.items import DmozItem
class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
    ]
    def parse(self, response):
        for sel in response.xpath('//ul/li'):
            item = DmozItem()
            item['title'] = sel.xpath('a/text()').extract()
            item['link'] = sel.xpath('a/@href').extract()
            item['desc'] = sel.xpath('text()').extract()
            yield item
```
* 最简单的保存爬取数据的方式是：
```
scrapy crawl dmoz -o items.json
```
该命令采用JSON格式对爬取的数据进行序列化，生成 `items.json` 文件。如果需要对爬取到的item做更为复杂的操作，可以编写Item Pipeline。
