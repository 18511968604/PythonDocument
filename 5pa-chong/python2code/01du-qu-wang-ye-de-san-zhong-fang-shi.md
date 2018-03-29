```py
#!C:\Python36\python.exe
# coding=utf-8
import urllib2


# 读取网页的三种方式

def download1(url):
    print urllib2.urlopen(url).info()  # 包含网络请求的详细信息
    return urllib2.urlopen(url).read()  # 读取整个网页，返回字符串


def download2(url):
    return urllib2.urlopen(url).readlines()  # 读取网业的每一行数据，返回列表


def download3(url):
    response = urllib2.urlopen(url, timeout=10)
    while True:
        line = response.readline()  # 读取一行
        if not line:
            break
        print line


if __name__ == '__main__':
    url = "http://www.baidu.com"  # urllib2 只能处理http协议
    print download1(url)

```



