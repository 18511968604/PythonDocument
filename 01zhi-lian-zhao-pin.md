```py
# encoding:utf-8
import urllib
from bs4 import BeautifulSoup
from urllib import request


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    soup = BeautifulSoup(data, "html5lib", from_encoding="utf-8")
    # findall
    spanlist = soup.find_all("span", class_="search_yx_tj")
    print(spanlist)
    print(spanlist[0].em.string)
    print(soup.select('.search_yx_tj'))
    print(((soup.select('.search_yx_tj')[0]).select("em")[0]).get_text())
    print(((soup.select('span[class="search_yx_tj"]')[0]).select("em")[0]).get_text())


download("http://sou.zhaopin.com/jobs/searchresult.ashx?jl=%E6%B7%B1%E5%9C%B3&kw=python&sm=0&p=1")

```



