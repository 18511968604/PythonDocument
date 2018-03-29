```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
import urllib2


def download2(addr, job):
    header = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}

    wd = {"jl": addr, "kw": job}
    wd = urllib.urlencode(wd)
    url = "https://sou.zhaopin.com/jobs/searchresult.ashx?"
    newurl = url + wd
    request = urllib2.Request(newurl, headers=header)  # 发送请求，伪装浏览器访问
    request.add_header("Connection", "keep-alive")  # 一直活着
    response = urllib2.urlopen(request)
    data = response.read().decode("utf-8")
    print response.code  # 响应状态码
    return data


if __name__ == '__main__':
    addr = "深圳"
    job = "python"
    print download2(addr, job)

```



