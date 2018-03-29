```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import cookielib

filePath = "cookie.txt"

cookie = cookielib.LWPCookieJar(filePath)  # 设置保存路径
header = urllib2.HTTPCookieProcessor(cookie)
opner = urllib2.build_opener(header)
response = opner.open("http://www.baidu.com/")
cookie.save(ignore_discard=True, ignore_expires=True)  # 忽略错误

```



