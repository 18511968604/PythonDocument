```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import ssl


def download(url):
    return urllib2.urlopen(url).read()


url = "https://www.baidu.com"
print download(url)

context = ssl._create_unverified_context()  # 忽略安全
header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}
request = urllib2.Request(url, headers=header)  # 发送请求，伪装浏览器访问
request.add_header("Connection", "keep-alive")  # 一直活着
response = urllib2.urlopen(request, context=context)
print response.read()
```



