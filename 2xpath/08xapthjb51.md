```py
# encoding:utf-8
import urllib
from urllib import request
import lxml
import lxml.etree
import re


def makeurllist(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    mytree = lxml.etree.HTML(data)
    mylist = mytree.xpath("//*[@class=\"dxypage clearfix\"]//text()")
    lastmystr = mylist[0]
    restr = u"页次：1/(\d+) 每页"
    regex = re.compile(restr, re.IGNORECASE)
    mylastlist = regex.findall(lastmystr)
    numbers = eval(mylastlist[0])  # 整数
    urllist = []
    for i in range(1, numbers + 1):
        urllist.append("http://www.jb51.net/list/list_97_" + str(i) + ".htm")
    return urllist


def gettitilefromurl(url):
    lastlist = []  # [(title,url),(title,url),(title,url),(title,url)]
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    mytree = lxml.etree.HTML(data)
    urllist = mytree.xpath("//*[@class=\"artlist clearfix\"]/dl//dt/a/@href")
    print(urllist)
    titlelist = mytree.xpath("//*[@class=\"artlist clearfix\"]/dl//dt/a/@title")
    print(titlelist)
    for title in titlelist:
        print(title)


# print (makeurllist("http://www.jb51.net/list/list_97_1.htm"))
print(gettitilefromurl("http://www.jb51.net/list/list_97_4.htm"))

```



