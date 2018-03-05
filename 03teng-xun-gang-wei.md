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
    print(soup)
    data = soup.find_all("ul", class_="squareli")
    for dataline in data:
        for linedata in dataline.find_all("li"):
            print(linedata.string)

    data = soup.select('ul[class="squareli"]')
    for dataline in data:
        for linedata in dataline.select("li"):
            print(linedata.get_text())


download("http://hr.tencent.com/position_detail.php?id=32347&keywords=python&tid=0&lid=0")

```



