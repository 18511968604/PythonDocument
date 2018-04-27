```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import requests


url = "http://www.hshfy.sh.cn/shfy/gweb2017/ktgg_search_content.jsp?"

# print(requests.get(url).content.decode("gbk"))

data = {
    "yzm": " eHep",
    "ft": "",
    "ktrqks": " 2018 - 04 - 26",
    "ktrqjs": " 2018 - 05 - 26",
    "spc": "",
    "yg": "",
    "bg": "",
    "ah": "",
    "pagesnum": "2"}

header = {
    "Referer": "http://www.hshfy.sh.cn/shfy/gweb2017/channel_xw_list.jsp?pa=abG1kbT1MTTAxMDEmbG1tYz23qNS6tq/MrAPdcssPdcssz&zd=xwzx",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"}


page = requests.post(url=url, headers=header, data=data)
print(page.content.decode("gbk"))


```



