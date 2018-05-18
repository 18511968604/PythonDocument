## 处理HTTPS请求 SSL证书验证 {#处理https请求-ssl证书验证}

现在随处可见 https 开头的网站，urllib2可以为 HTTPS 请求验证SSL证书，就像web浏览器一样，如果网站的SSL证书是经过CA认证的，则能够正常访问，如：`https://www.baidu.com/`等...

如果SSL证书验证不通过，或者操作系统不信任服务器的安全证书，比如浏览器在访问12306网站如：[https://www.12306.cn/mormhweb/](https://www.12306.cn/mormhweb/)的时候，会警告用户证书不受信任。（据说 12306 网站证书是自己做的，没有通过CA认证）

```py
import urllib
import urllib2
# 1. 导入Python SSL处理模块
import ssl

# 2. 表示忽略未经核实的SSL证书认证
context = ssl._create_unverified_context()

url = "https://www.12306.cn/mormhweb/"

headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36"}

request = urllib2.Request(url, headers = headers)

# 3. 在urlopen()方法里 指明添加 context 参数
response = urllib2.urlopen(request, context = context)

print response.read()
```



