```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
import urllib.request

import lxml
import lxml.etree

url = "http://www.biquge5200.cc/2_2598/1847694.html"

data = urllib.request.urlopen(url).read().decode('gbk')
# print(data)

mytree = lxml.etree.HTML(data)

title = mytree.xpath("//div[@class=\"bookname\"]/h1/text()")
print(title)
content = mytree.xpath("//*[@id=\"content\"]//p")
for i in content:
    print(i.xpath("./text()")[0].replace("\u3000", ''))

```



