```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import json

url = "https://movie.douban.com/j/new_search_subjects?sort=T&range=0,10&tags=&start=0"
data = urllib2.urlopen(url).read()
print data
mydict = json.loads(data)
print mydict["data"]
for i in mydict["data"]:
    print i["star"]
    print i["title"]

```



