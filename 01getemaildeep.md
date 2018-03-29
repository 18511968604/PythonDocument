```py
#! C:\Python36\python.exe
'''
about what
'''
from SpiderUtil import getPageUrl

# 深度字典
depthDict = {}


# 递归方法（先把自己的邮箱爬了，再找出孩子页面，对孩子页面递归）
# 递归终止条件：递归层级已达到阈值
def getEmailsDeep(url, depth):
    print("\t\t\t" * depthDict[url], "%d级页%s把自己给爬了" % (depthDict[url], url))

    # 递归终止条件：
    if (depthDict[url] >= depth):
        return  # 撞墙递归终止，结果返给老爸，老爸开始递归其弟弟

    # 获得子页面
    slist = getPageUrl(url)
    for s in slist:
        if s not in depthDict:
            # 递归前给子页面层级赋值
            depthDict[s] = depthDict[url] + 1

            # 递归子页面
            res = getEmailsDeep(s, depth)  # 当前s递归有了结果，就对下一个s进行递归


if __name__ == "__main__":
    startUrl = "http://www.baidu.com/s?wd=%E5%B2%9B%E5%9B%BD%20%E9%82%AE%E7%AE%B1"
    depthDict[startUrl] = 1

    # 从一级页面开始递归
    getEmailsDeep(startUrl, 4)

    print("main over")

```



