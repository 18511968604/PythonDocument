```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import urllib
import cookielib

# 创建一个cookie对象
cookie = cookielib.CookieJar()
# 提取cookie
cookie_hander = urllib2.HTTPCookieProcessor(cookie)
# 创建一个打开启，使用cookie
opener = urllib2.build_opener(cookie_hander)
# 模拟浏览器登陆
opener.addheaders("User-Agent",
                  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36")

loginurl = "http://www.renren.com/PLogin.do"
data = {"email": "xxxx", "password": "xxxx"}
data = urllib.urlencode(data)  # 编码
request = urllib2.Request(url=loginurl, data=data)  # post登陆
response = opener.open(request)  # 载入cookie，登陆
response_index = opener.open("http://www.renren.com/4354719823/profile")

print response_index.read()
```

## 2、使用登录后的cookie访问

```
使用sublime替换
^(.*):\s(.*)$
"\1":"\2",
```



