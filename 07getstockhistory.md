```py
#! C:\Python36\python.exe
'''
爬取个股历史数据
'''
import re
import threading

import requests
from SpiderUtil import *

# 东方财富网个股列表
stockUrl = "http://quote.eastmoney.com/stocklist.html"

# 个股CSV下载地址
# 'http://quotes.money.163.com/service/chddata.html?code=0' + '600123' + '&end=20170705&fields=TCLOSE;HIGH;LOW;TOPEN;LCLOSE;CHG;PCHG;TURNOVER;VOTURNOVER;VATURNOVER;TCAP;MCAP'
downUrlStart = 'http://quotes.money.163.com/service/chddata.html?code='
downUrlEnd = '&end=20170830&fields=TCLOSE;HIGH;LOW;TOPEN;LCLOSE;CHG;PCHG;TURNOVER;VOTURNOVER;VATURNOVER;TCAP;MCAP'


# 获取个股列表
def getStockList(url):
    # 对方网站的编码为gb2312
    html = requests.get(url).content.decode("gbk")

    # 使用正则获取个股列表
    # <li><a target="_blank" href="http://quote.eastmoney.com/sh501001.html">财通精选(501001)</a></li>
    pattern = "<li><a.*>(\w*)\((\d{6})\)</a></li>"
    reslist = re.findall(pattern, html)

    # 清洗数据（去掉非个股）（个股代码以沪市6开头、深市0开头、创业板3开头）
    thelist = reslist[:]  # 先复制一份
    # 一边遍历，一边从副本中删除非个股项目
    for item in reslist:
        if not (item[1].startswith("6") or item[1].startswith("3") or item[1].startswith("0")):
            thelist.remove(item)

    return thelist


sem = threading.Semaphore(100)


def myDownloadFile(url, filepath):
    # 使用Semaphore控制并发量
    with sem:
        downloadFile(url, filepath)


if __name__ == "__main__":

    # 先拿到个股列表
    slist = getStockList(stockUrl)

    # 遍历下载全部个股
    for i in range(len(slist)):
        # 从结果元素中抠出个股名称和代码
        sname = slist[i][0]
        scode = slist[i][1]

        # 构造下载路径，要注意沪深两市市场代码（沪市0深市1）
        url = downUrlStart + ("0" if scode.startswith("6") else "1") + scode + downUrlEnd
        filepath = "D:\PyDownload\csv\\" + (scode + "_" + sname) + ".csv"

        # 多线程下载（此处函数内使用了Semaphore来控制并发量，并发量太高可能会导致下载失败）
        threading.Thread(target=myDownloadFile, args=(url, filepath)).start()

    print("main over")

```



