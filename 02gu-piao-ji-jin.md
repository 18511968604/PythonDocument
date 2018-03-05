```py
# encoding:utf-8
import urllib
from urllib import request
from bs4 import BeautifulSoup


def download(url):
    headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);"}
    request = urllib.request.Request(url, headers=headers)  # 请求，修改，模拟http.
    data = urllib.request.urlopen(request).read()  # 打开请求，抓取数据
    soup = BeautifulSoup(data, "html5lib", from_encoding="gb2312")
    # mytable=soup.find_all(id="datalist")
    mytable = soup.select("#datalist")
    # mytable[0]表格
    # for line  in  mytable[0].select("tr"):
    for line in mytable[0].find_all("tr"):
        print(line)  # 提取每一个行业
        # for  mydata  in line.select("td"):
        for mydata in line.find_all("td"):
            print(mydata.string)
            # print mydata.get_text()


if __name__ == '__main__':
    download("http://quote.stockstar.com/fund/stock_3_1_2.html")

```



