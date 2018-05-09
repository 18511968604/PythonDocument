```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
from urllib import request
import lxml
from lxml import etree

url = "http://news.sina.com.cn/china/"

data = urllib.request.urlopen(url).read().decode("utf-8")
mytree = lxml.etree.HTML(data)

threeTitle = mytree.xpath("//div[@class=\"links\"]//a")

for t in threeTitle:
    print(t.xpath("./text()"))
    print(t.xpath("./@href"))

```



