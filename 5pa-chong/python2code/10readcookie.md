```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import cookielib

# 创建一个对象存储cookie
cookie = cookielib.CookieJar()
# 提取cookie
header = urllib2.HTTPCookieProcessor(cookie)
# 处理 cookie
opener = urllib2.build_opener(header)
response = opener.open("http://www.baidu.com/")
cookies = ""
for c in cookie:
    print c
for data in cookie:
    cookies = cookies + data.name + "=" + data.value + "\r\n"

print cookies

```



