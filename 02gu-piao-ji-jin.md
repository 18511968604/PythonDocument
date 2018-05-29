```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
'''
@Time    : 2018/5/29 17:47
@Author  : Fate
@File    : aaa.py
'''
import urllib
from urllib import request
from bs4 import BeautifulSoup

stockList = []


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    soup = BeautifulSoup(data, "html5lib", from_encoding="gb2312")
    mytable = soup.select("#datalist")
    for line in mytable[0].find_all("tr"):
        print(line.get_text())  # 提取每一个行业

        print(line.select("td:nth-of-type(3)")[0].text) # 提取具体的某一个


if __name__ == '__main__':
    download("http://quote.stockstar.com/fund/stock_3_1_2.html")

```



