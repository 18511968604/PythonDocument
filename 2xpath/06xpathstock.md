```py
# encoding:utf-8
import urllib
from urllib import request
import lxml
import lxml.etree
import re


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    # data=data.decode("gbk").encode("utf-8")#乱码
    mytree = lxml.etree.HTML(data)
    datalist = mytree.xpath("//*[@id=\"datalist\"]//tr//td//text()")
    print(datalist)
    for linedata in datalist:
        print(linedata)


download("http://quote.stockstar.com/fund/stock_3_1_2.html")

```



