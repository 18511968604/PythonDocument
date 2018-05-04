```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
'''
@Time    : 2018/5/4 16:11
@Author  : Fate
@File    : urllibCookie.py
'''
# coding=utf-8
import urllib
from urllib import request, parse
from http import cookiejar

# 创建一个cookie对象
filename = "cookie.txt"
cookie = cookiejar.LWPCookieJar(filename)
# 提取cookie
hander_cookie = urllib.request.HTTPCookieProcessor(cookie)
# 创建一个打开启
opener = urllib.request.build_opener(hander_cookie)
# 安装opener,可全局使用
urllib.request.install_opener(opener)
header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"}
loginUrl = "http://www.renren.com/PLogin.do"
data = {
    "email": "18588403840",
    "password": "Changeme_123"
}
data = urllib.parse.urlencode(data).encode('utf-8')
req = urllib.request.Request(url=loginUrl, headers=header, data=data)
response = urllib.request.urlopen(req)
cookie.save(ignore_discard=True, ignore_expires=True)  # 保存cookie可重复使用
print(response.read())

indexurl = "http://zhibo.renren.com/top"

print("==================")
print(urllib.request.urlopen(indexurl).read().decode())
```

反复使用cookie

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
'''
@Time    : 2018/5/4 16:11
@Author  : Fate
@File    : urllibCookie.py
'''
# coding=utf-8
import urllib
from urllib import request, parse
from http import cookiejar

# 创建一个cookie对象
filename = "cookie.txt"
cookie = cookiejar.LWPCookieJar()
cookie.load(filename, ignore_expires=True, ignore_discard=True)  # 加载cookie
# 提取cookie
hander_cookie = urllib.request.HTTPCookieProcessor(cookie)
# 创建一个打开启
opener = urllib.request.build_opener(hander_cookie)
# 安装opener,可全局使用
urllib.request.install_opener(opener)
header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"}

indexurl = "http://zhibo.renren.com/top"

print("==================")
print(urllib.request.urlopen(indexurl).read().decode())
```



