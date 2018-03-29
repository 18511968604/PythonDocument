```py
#! C:\Python36\python.exe
'''
广度爬取层级控制
'''
from SpiderUtil import *

oldlist = []
newlist = []
# checkSet = set()#去重的集合

# 深度字典，url为键，深度为值{url:depth,...}
depthDict = {}

# 向待爬列表中生产url，depth=深度
def collectUrls(depth):
    while (len(oldlist) > 0):

        # 弹出oldlist头部的url，丢入newlist
        url = oldlist.pop(0)
        newlist.append(url)
        print("\t\t\t" * depthDict[url], "线程中爬取%d级页面：%s" % (depthDict[url], url))

        # 如果深度没有达到depth，就...
        if depthDict[url] < depth:
            # 向oldlist中生孩子
            sonList = getPageUrl(url)
            if sonList:
                for s in sonList:

                    # 不在去重集合中的url才丢入待爬列表
                    if s not in depthDict:
                        # checkSet.add(url)
                        depthDict[s] = depthDict[url]+1
                        oldlist.append(s)


if __name__ == "__main__":
    # startUrl = "http://www.baidu.com/s?wd=%E5%B2%9B%E5%9B%BD%20%E9%82%AE%E7%AE%B1"
    startUrl = "https://www.baidu.com/s?wd=Python%20邮箱"

    # 将起始页丢入待爬列表
    # checkSet.add(startUrl)#先丢入去重集合
    depthDict[startUrl] = 1
    oldlist.append(startUrl)

    # 循环向待爬列表中生产url
    collectUrls(4)

    # # 爬之
    # for url in newlist:
    #     print("\t\t\t" * depthDict[url], "爬取%d级页面：%s" % (depthDict[url], url))

    print("main over")

```



