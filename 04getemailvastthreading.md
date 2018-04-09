```py
#! C:\Python36\python.exe
'''
多线程广度爬虫（含层级控制）
'''
import threading

import time

from SpiderUtil import *

oldlist = []
newlist = []
# checkSet = set()#去重的集合

# 深度字典，url为键，深度为值{url:depth,...}
depthDict = {}

tlist = []


# 向待爬列表中生产url，depth=深度
# 在深度允许的范围内，源源不断向新中国输送人丁
def collectUrls(depth):
    # 旧列表中还有未产子url，或者子线程还没死光，都要继续收集url
    # 还有活动线程说明url并没有收集完毕
    while ((len(oldlist) > 0) or (threading.activeCount() > 1)):
        print("活动线程数：%d" % (threading.activeCount()))

        # 家里没人了，但外面有人，怀着希望等下去
        # 如果oldlist已经弹光，但是有未结束的活动线程（有url正在赶来的路上），原地死等
        # 等到了新来的孩子：继续后续逻辑（向新列表中弹出一个孩子，加入待爬列表，并检测是否需要继续生孩子）
        # 超时且未等到：回到最外层循环重新判断（是否所有线程都已经死了，无论是否提交了结果）
        start = time.time()  # 超时起点
        fuck = False

        # 外面还有人，怀着希望死等
        while len(oldlist) == 0:
            # 判断死等是否超时（如果超时则重新回到外层循环判断【是否所有线程都死光了】）
            if time.time() - start > 15:
                fuck = True  # 回归最外层循环的标记
                break
            continue
        # 如果需要的话，略过本次循环，回归最外层循环
        # fuck=True时，代表良人有可能死在外面了
        if fuck:
            # 去看看良人是否真的死在外面了
            continue

        # 弹出oldlist头部的url，丢入newlist
        url = oldlist.pop(0)
        newlist.append(url)
        print("\t\t\t" * depthDict[url], "线程中爬取%d级页面：%s" % (depthDict[url], url))

        # 如果深度没有达到depth，就...
        if depthDict[url] < depth:
            t = threading.Thread(target=myGetPageUrl, args=(url,))
            tlist.append(t)
            t.start()


# 检查是否所有的线程都死光光
def checkAllThreadDead(tlist):
    for t in tlist:
        if t.is_alive():
            return False
    return True


def myGetPageUrl(url):
    # 向oldlist中生孩子
    sonList = getPageUrl(url)
    if sonList:
        for s in sonList:

            # 不在去重集合中的url才丢入待爬列表
            if s not in depthDict:
                # checkSet.add(url)
                depthDict[s] = depthDict[url] + 1
                oldlist.append(s)


# startUrl = "http://www.baidu.com/s?wd=%E5%B2%9B%E5%9B%BD%20%E9%82%AE%E7%AE%B1"
startUrl = "https://www.baidu.com/s?wd=Python%20邮箱"
if __name__ == "__main__":

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

    # 整理层级关系
    for k, v in depthDict.items():
        pass

    for k in depthDict.keys():
        pass
```



