```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import urllib
from urllib import request
import json


# https://movie.douban.com/tag/#/
jsonurl = "https://movie.douban.com/j/new_search_subjects?sort=T&range=0,10&tags=&start="

urllist = []
for i in range(100):
    url = jsonurl + str(i * 20)
    urllist.append(url)

header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"}

movie = []
for url in urllist:

    req = urllib.request.Request(url=url, headers=header)

    data = urllib.request.urlopen(req).read()
    data = json.loads(data)
    for d in data["data"]:
        # print(d["directors"][0])
        # print(d["rate"])
        # print(d["url"])
        # print(d["title"])
        print((d["directors"][0], d["rate"], d["url"], d["title"]))
        movie.append((d["directors"][0], d["rate"], d["url"], d["title"]))

for m in movie:
    print(m)

```



