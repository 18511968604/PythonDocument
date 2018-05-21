## User-Agent {#useragent}

**一、为何要设置User Agent**

有一些网站不喜欢被爬虫程序访问，所以会检测连接对象，如果是爬虫程序，也就是非人点击访问，它就会不让你继续访问，所以为了要让程序可以正常运行，需要隐藏自己的爬虫程序的身份。此时，我们就可以通过设置User Agent的来达到隐藏身份的目的，User Agent的中文名为用户代理，简称UA。

    User Agent存放于Headers中，服务器就是通过查看Headers中的User Agent来判断是谁在访问。在Python中，如果不设置User Agent，程序将使用默认的参数，那么这个User Agent就会有Python的字样，如果服务器检查User Agent，那么没有设置User Agent的Python程序将无法正常访问网站。

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import urllib2


def download1(url):
    return urllib2.urlopen(url).read().decode("gb2312")


def download2(url):
    header = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}
    request = urllib2.Request(url, headers=header)  # 发送请求，伪装浏览器访问
    request.add_header("Connection", "keep-alive")  # 一直活着
    print request.get_full_url()  # 访问的网页链接
    print request.get_host()  # 服务器域名
    print request.get_method()  # get或post
    print request.get_type()  # http/https/ftp

    response = urllib2.urlopen(request)
    print response.code  # 状态码200, 404，500
    print response.info  # 网页详细信息

    data = response.read().decode("gb2312")
    print response.code  # 响应状态码
    return data


if __name__ == '__main__':
    url = "http://search.51job.com/list/040000,000000,0000,00,9,99,python,2,1.html?lang=c&stype=&postchannel=0000&workyear=99&cotype=99&degreefrom=99&jobterm=99&companysize=99&providesalary=99&lonlat=0%2C0&radius=-1&ord_field=0&confirmdate=9&fromType=&dibiaoid=0&address=&line=&specialarea=00&from=&welfare="
    print download2(url)
```

## 添加更多的Header信息 {#添加更多的header信息}

在 HTTP Request 中加入特定的 Header，来构造一个完整的HTTP请求消息。

> 可以通过调用`Request.add_header()`添加/修改一个特定的header 也可以通过调用`Request.get_header()`来查看已有的header。

```
request.add_header("Connection", "keep-alive")  # 一直活着
```



