```py
# encoding:utf-8
import urllib
from urllib import request
from bs4 import BeautifulSoup


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    soup = BeautifulSoup(data, "html5lib")

    data = soup.find_all("table", class_="tablelist")
    for line in data[0].find_all("tr", class_=["even", "odd"]):
        print(line.find_all("td")[0].a["href"])
        for data in line.find_all("td"):
            print(data.string)


download("http://hr.tencent.com/position.php?keywords=python&lid=0&tid=0&start=100#a")
```



