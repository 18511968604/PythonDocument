```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
from urllib import request
import lxml
from lxml import etree

url = "http://news.sina.com.cn/guide/"

data = urllib.request.urlopen(url).read().decode("utf-8")
# print(data)
mytree = lxml.etree.HTML(data)

parent = mytree.xpath("//div[@class=\"clearfix\"]")

for title in parent:
    # 提取一级标题
    bigTitle = title.xpath("./h3/a/text() | ./h3/span/text()")
    print(bigTitle)

    # 提取二级标题
    for t in title.xpath("./ul//li"):
        secondTitle = t.xpath("./a/text()")
        secondTitleUrl = t.xpath("./a/@href")
        print(secondTitle, secondTitleUrl)
    print()

```



