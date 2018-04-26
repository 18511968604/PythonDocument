```py
# encoding:utf-8
import urllib
from urllib import request
import lxml
import lxml.etree
import re


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    # data=data.decode("gbk").encode("utf-8")#乱码
    mytree = lxml.etree.HTML(data)
    datalist = mytree.xpath("//*[@class=\"piclist longList\"]//a[@href!=\"/article/\"]//text()")
    print(datalist)
    for linedata in datalist:
        print(linedata)

    datalist = mytree.xpath("//*[@class=\"piclist longList\"]//div[@class=\"f18 mb20\"]//p//text()")
    print(datalist)
    for linedata in datalist:
        print(linedata)

    datalist = mytree.xpath("//*[@class=\"piclist longList\"]//div[@class=\"f18 mb20\"]//text()")
    print(datalist)
    for linedata in datalist:
        print(linedata)


download("http://www.neihan8.com/article/list_5_10.html")
```

```py
# encoding:utf-8
import urllib
from urllib import request

from bs4 import BeautifulSoup


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    # data=data.decode("gbk").encode("utf-8")#乱码

    html = BeautifulSoup(data, "lxml")

    # 存储段子
    titleList = []
    contentList = []
    dz = {}

    t = html.select("ul.piclist.longList > li > h4 > a")
    for i in t:
        titleList.append(i.get_text())

    c = html.select("div.f18.mb20")
    for i in c:
        contentList.append(i.get_text().replace("\r\n", "").strip())

    dz = dict(zip(titleList, contentList))

    for k, v in dz.items():
        print(k, v)

    print(dz)


download("http://www.neihan8.com/article/list_5_10.html")

```



