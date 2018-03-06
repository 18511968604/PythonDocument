```py
# encoding:utf-8
import urllib
from urllib import request
import lxml
import lxml.etree


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    mytree = lxml.etree.HTML(data)
    print(mytree.xpath("//*[@class=\"search_yx_tj\"]/em/text()"))


download("http://sou.zhaopin.com/jobs/searchresult.ashx?jl=%E6%B7%B1%E5%9C%B3&kw=python&sm=0&p=1")

```



