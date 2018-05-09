```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
from urllib import request
import lxml
from lxml import etree

url = "http://news.sina.com.cn/c/nd/2018-05-07/doc-ihacuuvu6744467.shtml"

data = urllib.request.urlopen(url).read().decode("utf-8")
# print(data)
mytree = lxml.etree.HTML(data)
# 提取正文
content = mytree.xpath("//*[@id=\"article\"]//p/text()")
for c in content:
    print(c)

```



