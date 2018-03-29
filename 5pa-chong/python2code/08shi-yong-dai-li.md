```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
# 安装ccproxy代理
httpproxy = urllib2.ProxyHandler({"http": "10.36.100.109:808"})  # 代理，无需账号
opener = urllib2.build_opener(httpproxy)  # 创建一个打开器
header = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}
request = urllib2.Request("https://www.baidu.com/", headers=header)  # 访问百度
response = opener.open(request)  # 使用代理打开网页
print response.read()

```



