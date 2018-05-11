## ProxyHandler处理器（代理设置） {#proxyhandler处理器（代理设置）}

使用代理IP，这是爬虫/反爬虫的第二大招，通常也是最好用的。

很多网站会检测某一段时间某个IP的访问次数\(通过流量统计，系统日志等\)，如果访问次数多的不像正常人，它会禁止这个IP的访问。

所以我们可以设置一些代理服务器，每隔一段时间换一个代理，就算IP被禁止，依然可以换个IP继续爬取。

urllib2中通过ProxyHandler来设置使用代理服务器，下面代码说明如何使用自定义opener来使用代理：

```py
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

```
import random

import requests

# 假设此时有一已经格式化好的ip代理地址proxies
# 西刺代理：http://www.xicidaili.com/
iplist = [
    "http://183.159.84.198:18118",
    "http://183.159.92.206:18118",
    "http://119.179.209.43:61234",
    "http://183.159.82.181:18118"
]

# print(random.choice(iplist))

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36X-Requested-With: XMLHttpRequest"}
url = 'https://blog.csdn.net/linangfs/article/details/78331419?locationNum=9&fps=1'
for i in range(10):
    proxies = {"http": random.choice(iplist)}
    try:
        page = requests.get(url, headers=headers, proxies=proxies)
    except:
        print('失败')
    else:
        print('成功')
```



