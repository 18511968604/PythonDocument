```py
#! C:\Python36\python.exe
'''
爬取个股列表
'''

# 东方财富网个股列表
import re
import requests

from SpiderUtil import *

# 东方财富网个股列表
stockUrl = "http://quote.eastmoney.com/stocklist.html"

if __name__ == "__main__":
    # 对方网站的编码为gb2312
    html = requests.get(stockUrl).content.decode("gbk")

    # <li><a target="_blank" href="http://quote.eastmoney.com/sh501001.html">财通精选(501001)</a></li>
    # 当findall时，只有括号内的部分（组）会被结果收录，如果有多个组，则结果集中的每个元素都是tuple
    pattern = "<li><a.*>(\w*)\((\d{6})\)</a></li>"
    reslist = re.findall(pattern,html)

    # 清洗数据（去掉非个股）（个股代码以沪市6开头、深市0开头、创业板3开头）
    thelist = reslist[:]#先复制一份
    # 一边遍历，一边从副本中删除非个股项目
    for item in reslist:
        if not (item[1].startswith("6") or item[1].startswith("3") or item[1].startswith("0")):
            thelist.remove(item)
    printList(thelist)
    print(len(thelist))

    # 作业：写入CSV


    print("main over")
```



