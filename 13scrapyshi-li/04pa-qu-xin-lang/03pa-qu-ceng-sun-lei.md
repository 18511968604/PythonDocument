```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
from urllib import request
import lxml
from lxml import etree

url = "http://roll.news.sina.com.cn/news/gnxw/gdxw1/index_1.shtml"


def get_page(url):
    data = urllib.request.urlopen(url).read().decode("gbk")
    mytree = lxml.etree.HTML(data)

    fourTitle = mytree.xpath("//ul[@class=\"list_009\"]//li")
    if len(fourTitle) != 0:
        for t in fourTitle:
            title = t.xpath("./a/text()")
            url = t.xpath("./a/@href")
            times = t.xpath("./span/text()")
            print(title, url, times)

    else:
        pass

    # 实现翻页
    nexturl = mytree.xpath("//span[@class=\"pagebox_next\"]/a/@href")[0]
    nexturl = nexturl[1:]
    print(nexturl)

    newUrl = "http://roll.news.sina.com.cn/news/gnxw/gdxw1" + nexturl
    print(newUrl)
    get_page(newUrl)


get_page(url)

```



