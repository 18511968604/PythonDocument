```py
#! C:\Python36\python.exe
'''
爬取页面邮箱2.0
爬取当前页面和子页面的邮箱（深度爬取）
'''
import csv
import re
import threading

import time

# from SpiderUtil import *
from w4.day2.SpiderUtil import *
# 已爬过的链接列表
tempset = set()

# 获取一个页面内的所有邮箱
def getPageEmails(url):

    # 判断是否已爬过
    if url in tempset:
        return

    print("\n-----",url,"-----")
    # 获取页面html
    html = getHtml(url)
    # 使用正则获取页面邮箱列表
    reslist = re.findall(PATTERN_EMAIL, html, flags=re.A)

    # 写入CSV
    writer = csv.writer(file)
    # writer.writerow(("邮箱", "爬取时间", "状态"))
    for email in reslist:
        print(email)
        writer.writerow([email, time.ctime(), "未发送"])

def getEmailsInfinite(url):
    # 爬取【自身页面】的邮箱
    html = getHtml(url)
    # threading.Thread(target=getPageEmails,args=(url,)).start()
    getPageEmails(url)
    tempset.add(url)

    # 爬取【自身页面】超链接，丢入容器
    ulist = re.findall(PATTERN_URL, html)

    # 遍历容器中的url，递归爬取
    for url in ulist:
        if url not in tempset:
            print("recursion")
            getEmailsInfinite(url)

file = open(r"C:\Users\idea\Desktop\岛民邮箱.csv", "a+", newline="")
if __name__ == "__main__":
    getEmailsInfinite("http://www.baidu.com/s?wd=%E5%B2%9B%E5%9B%BD%20%E9%82%AE%E7%AE%B1")

    print("main over")

```



