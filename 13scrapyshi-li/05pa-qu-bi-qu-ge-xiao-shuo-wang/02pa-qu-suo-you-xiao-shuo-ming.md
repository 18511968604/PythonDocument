```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
import urllib.request

import lxml
import lxml.etree

url = "http://www.biquge5200.cc/xuanhuanxiaoshuo/"

data = urllib.request.urlopen(url).read().decode('gbk')
# print(data)

mytree = lxml.etree.HTML(data)

level2 = mytree.xpath("//div[@class=\"r\"]/ul//li")

for i in level2:
    print(i.xpath("./span/a/text()"))
    print(i.xpath("./span/a/@href"))
    print(i.xpath("./span[2]/text()"))

```



