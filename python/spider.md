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
