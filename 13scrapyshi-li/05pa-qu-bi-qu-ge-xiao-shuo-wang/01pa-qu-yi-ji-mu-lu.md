```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import urllib
import urllib.request

import lxml
import lxml.etree

url = "https://www.biquge5200.cc/"

data = urllib.request.urlopen(url).read().decode('gbk')
# print(data)

mytree = lxml.etree.HTML(data)
level1 = mytree.xpath("//*[@class=\"nav\"]/ul//li")
for i in level1:
    print(i.xpath("./a/text()"))
    url = i.xpath("./a/@href")[0]
    newUrl = url[2:]
    print(newUrl)

```



