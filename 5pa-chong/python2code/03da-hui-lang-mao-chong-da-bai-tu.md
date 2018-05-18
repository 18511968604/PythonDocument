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

## User-Agent {#useragent}

但是这样直接用urllib2给一个网站发送请求的话，确实略有些唐突了，就好比，人家每家都有门，你以一个路人的身份直接闯进去显然不是很礼貌。而且有一些站点不喜欢被程序（非人为访问）访问，有可能会拒绝你的访问请求。

但是如果我们用一个合法的身份去请求别人网站，显然人家就是欢迎的，所以我们就应该给我们的这个代码加上一个身份，就是所谓的`User-Agent`头。

> * 浏览器 就是互联网世界上公认被允许的身份，如果我们希望我们的爬虫程序更像一个真实用户，那我们第一步，就是需要伪装成一个被公认的浏览器。用不同的浏览器在发送请求的时候，会有不同的User-Agent头。 urllib2默认的User-Agent头为：
>   `Python-urllib/x.y`
>   （x和y是Python主版本和次版本号,例如 Python-urllib/2.7）

## 添加更多的Header信息 {#添加更多的header信息}

在 HTTP Request 中加入特定的 Header，来构造一个完整的HTTP请求消息。

> 可以通过调用`Request.add_header()`添加/修改一个特定的header 也可以通过调用`Request.get_header()`来查看已有的header。

```
request.add_header("Connection", "keep-alive")  # 一直活着
```



