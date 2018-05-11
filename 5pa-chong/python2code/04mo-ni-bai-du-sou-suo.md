```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import urllib


def getInfo(url):
    request = urllib2.Request(url)
    return urllib2.urlopen(request).read()


if __name__ == '__main__':
    url = "http://www.baidu.com/s"
    keyWord = {"wd": "谭"}
    kw = urllib.urlencode(keyWord)  # 编码
    # print kw
    # print urllib.unquote(kw)  # 解码
    newurl = url + "?" + kw
    print getInfo(newurl)
```



