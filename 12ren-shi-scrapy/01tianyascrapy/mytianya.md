```py
# -*- coding: utf-8 -*-
import scrapy
import re
from tianya import items


class MytianyaSpider(scrapy.Spider):
    name = 'mytianya'
    allowed_domains = ['bbs.tianya.cn']
    start_urls = ['http://bbs.tianya.cn/post-140-393977-1.shtml']

    def parse(self, response):
        html = response.body.decode()
        # ftsd@21cn.com
        email = re.compile(r"([A-Z0-9_]+@[A-Z0-9]+.[A-Z]{2,4})", re.I)
        emailList = email.findall(html)
        mydict = []
        for e in emailList:
            item = items.TianyaItem()
            item["email"] = e
            # mydict[e] = "http://bbs.tianya.cn/post-140-393977-1.shtml"
            mydict.append(item)
        return mydict

```



