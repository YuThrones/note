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

## 要在requests库中保持一个对话，可以使用requests.Session()。
