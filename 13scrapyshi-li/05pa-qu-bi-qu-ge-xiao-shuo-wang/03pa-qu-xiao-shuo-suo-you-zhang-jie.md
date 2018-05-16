```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
import urllib.request

import lxml
import lxml.etree

url = "https://www.biquge5200.cc/2_2598/"

data = urllib.request.urlopen(url).read().decode('gbk')
# print(data)

mytree = lxml.etree.HTML(data)

chapter = mytree.xpath("//*[@id=\"list\"]/dl//dd")
for i in chapter:
    print(i.xpath("./a/text()"))
    print(i.xpath("./a/@href"))

```



