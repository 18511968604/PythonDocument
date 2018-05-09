```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib
from urllib import request
import lxml
from lxml import etree

import os

url = "http://news.sina.com.cn/guide/"

# 根目录
rootdir = r"C:\Users\Administrator\Desktop\image\data"

data = urllib.request.urlopen(url).read().decode("utf-8")
# print(data)
mytree = lxml.etree.HTML(data)

parent = mytree.xpath("//div[@class=\"clearfix\"]")

for title in parent:
    # 提取一级标题
    bigTitle = title.xpath("./h3/a/text() | ./h3/span/text()")
    print(bigTitle)


    # 创建一级文件夹
    bigdir = rootdir + "//" + bigTitle[0]

    if not os.path.exists(bigdir):
        os.mkdir(bigdir)

    # 提取二级标题
    for t in title.xpath("./ul//li"):
        secondTitle = t.xpath("./a/text()")
        secondTitleUrl = t.xpath("./a/@href")
        print("----", secondTitle)

        # 创建二级标题
        if len(secondTitle) == 0:
            pass
        else:
            secondDir = bigdir + "//" +secondTitle[0]
            if not os.path.exists(secondDir):
                os.mkdir(secondDir)

        # 三级标题
        url = secondTitleUrl[0]
        data = urllib.request.urlopen(url).read().decode("utf-8", "ignore")
        mytree = lxml.etree.HTML(data)

        threeTitle = mytree.xpath("//div[@class=\"links\"]//a")

        for t in threeTitle:
            print('--------', t.xpath("./text()"))
            baseTitle = t.xpath("./text()")

            if len(baseTitle) == 0:
                pass
            else:
            # 创建三级文件夹
                baseDir = secondDir + "//" + baseTitle[0]
                if not os.path.exists(baseDir):
                    os.mkdir(baseDir)

    print()

```



