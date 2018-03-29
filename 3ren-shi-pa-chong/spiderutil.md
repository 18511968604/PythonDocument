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


# 获取html
def getHtml(url):
    html = requests.get(url).text
    return html


# 下载文件
def downloadFile(url, filepath):
    try:
        request.urlretrieve(url, filepath)
    except Exception as e:
        print(e)
    print(filepath, "下载成功！")
    pass


if __name__ == "__main__":
    print("main over")

```



