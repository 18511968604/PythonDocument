```py
#! C:\Python36\python.exe
'''
爬虫工具类
'''
from urllib import request

import re
import requests

# 23456789fghjkl@ghjkl.com
PATTERN_EMAIL = "\w*@\w*\.\w*"

# 超链接样式：<a...href="http://..."...>摸我以跳转到百度</a>
PATTERN_URL = "<a.*href=\"(https?://.*?)[\"\'].*>"


def getHtml(url):
    try:
        html = requests.get(url, timeout=10, headers={
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"}).text
        return html
    except Exception as e:
        print("访问%s时出现异常" % (url))
        print(e)


def getPageUrl(url, html=None):
    try:
        if html == None:
            html = getHtml(url)

        if html:
            urlList = re.findall(PATTERN_URL, html)
            return urlList
    except Exception as e:
        print("%s生孩子时出现异常" % (url))
        print(e)


def downloadFile(url, filepath):
    try:
        request.urlretrieve(url, filepath)
    except Exception as e:
        print(e)
    print(filepath, "下载成功！")
    pass


def printList(mlist):
    for item in mlist:
        print(item)


if __name__ == "__main__":
    print("main over")

```



