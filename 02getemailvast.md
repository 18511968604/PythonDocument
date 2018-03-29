```py
#! C:\Python36\python.exe
'''
about what
'''
from SpiderUtil import *

ulist = []
# checkSet = set()#去重的集合

# 深度字典，url为键，深度为值{url:depth,...}
depthDict = {}

if __name__ == "__main__":
    startUrl = "http://www.baidu.com/s?wd=%E5%B2%9B%E5%9B%BD%20%E9%82%AE%E7%AE%B1"

    # 将起始页丢入待爬列表
    # checkSet.add(startUrl)#先丢入去重集合
    depthDict[startUrl] = 1
    ulist.append(startUrl)

    # 将二级页全部丢入待爬列表
    html = getHtml(startUrl)
    secondList = getPageUrl(startUrl, html)
    # ulist += secondList
    for url in secondList:

        # 不在去重集合中的url才丢入待爬列表
        if url not in depthDict:
            # checkSet.add(url)
            depthDict[url] = 2
            ulist.append(url)

    # 将三级页全部丢入待爬列表
    for url in secondList:
        thirdList = getPageUrl(url)
        # ulist.extend(thirdList)

        # 不在去重集合中的url才丢入待爬列表
        for url in thirdList:
            if url not in depthDict:
                # checkSet.add(url)
                depthDict[url] = 3
                ulist.append(url)

    # 爬之
    for url in ulist:
        print("\t\t\t" * depthDict[url], "爬取%d级页面：%s" % (depthDict[url], url))

    print("main over")

```



