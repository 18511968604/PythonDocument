```py
#! C:\Python36\python.exe
'''
爬取股票基金
'''

# 股票基金表格
import csv
import re

from SpiderUtil import *

baseUrl = "http://quote.stockstar.com/fund/stock_3_1_1.html"

if __name__ == "__main__":

    # 先挖出tbody的字符串
    html = getHtml(baseUrl)
    # print(html)
    reslist = re.findall("<tbody[\s\S]*</tbody>", html)
    tbody = reslist[0]

    # 在tbody中检索所有文本节点
    reslist = re.findall(">(\S+?)</", tbody)
    # printList(reslist)

    # 写入CSV
    with open(r"C:\Users\idea\Desktop\股票基金.csv", "a+", newline="") as file:
        writer = csv.writer(file)

        # 每一个元素为一行，打包成一个列表
        for i in range(0, len(reslist), 10):
            rowlist = []
            for j in range(10):
                # print(reslist[i+j],end=",")
                rowlist.append(reslist[i + j])
            print(rowlist)
            writer.writerow(rowlist)

    print("main over")

```



