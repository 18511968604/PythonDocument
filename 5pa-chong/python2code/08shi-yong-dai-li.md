```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2

def noPasswd():
    # 安装ccproxy代理
    # 禁止外部用户访问--》设置-》高级-》网络-》禁止局域网外用户，取消勾选
    httpproxy = urllib2.ProxyHandler({"http": "10.36.100.109:808"})  # 代理，无需账号
    opener = urllib2.build_opener(httpproxy)  # 创建一个打开器
    request = urllib2.Request("https://www.baidu.com/")  # 访问百度
    response = opener.open(request)  # 使用代理打开网页
    print response.read()
    

# 使用密码
def usePasswd

    httpproxy = urllib2.ProxyHandler({"http": "User:123456@10.36.100.109:808"})  # 代理，无需账号
    opener = urllib2.build_opener(httpproxy)  # 创建一个打开器
    request = urllib2.Request("http://www.baidu.com/")  # 访问百度
    response = opener.open(request)  # 使用代理打开网页
    print response.read()
```



