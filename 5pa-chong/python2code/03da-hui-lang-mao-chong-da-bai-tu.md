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



