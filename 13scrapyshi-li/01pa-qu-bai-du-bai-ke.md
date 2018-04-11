```py
# -*- coding: utf-8 -*-
import scrapy
import baidubaike.items
import re
from bs4 import BeautifulSoup
from  scrapy.spiders import CrawlSpider,Rule #提取超链接的规则
from  scrapy.linkextractors  import LinkExtractor #提取超链接

class BaikeSpider(CrawlSpider):
    name = 'baike'
    allowed_domains = ['baike.baidu.com']
    start_urls = ['https://baike.baidu.com/item/Python/407313']
    # response提取超链接,
    # response提取超链接,
    pagelinks = LinkExtractor(allow=("/item/.*"))
    # 根据规则提取的链接，用一个函数处理，follow.是否一直循环下去
    rules = [Rule(pagelinks, callback="parse_item", follow=True)]  # 返回的urllist


    def gettitle(self,pagedata):
        soup = BeautifulSoup(pagedata, "html.parser")  # 解析
        list1 = soup.find_all("h1")
        list2 = soup.find_all("h2")
        if len(list1) != 0 and len(list2) != 0:
            return (list1[0].text, list2[0].text)
        elif len(list1) != 0 and len(list2) == 0:
            return list1[0].text
        else:
            return None
    def getcontent(self,pagedata):
        soup = BeautifulSoup(pagedata, "html.parser")  # 解析
        summary = soup.find_all("div", class_="lemma-summary")
        if len(summary) != 0:
            return summary[0].get_text()
        else:
            return None
    def parse_item(self, response):
        pagedata=response.body
        url=response.url   #body代表数据，url当前连接
        baikeitem=baidubaike.items.BaidubaikeItem()
        baikeitem["name"]= str(self.gettitle(pagedata))
        baikeitem["content"] =str(self.getcontent(pagedata))
        baikeitem["url"] =response.url
        yield  baikeitem
```

items.py

```py
class BaidubaikeItem(scrapy.Item):
    # define the fields for your item here like:
    name = scrapy.Field()
    content=scrapy.Field()
    url=scrapy.Field()
```

setting.py

```py
DOWNLOADER_MIDDLEWARES = {
    'baidubaike.middlewares.RandomUserAgent': 100,  #配置好代理
    'baidubaike.middlewares.RandomProxy': 200,
}


USER_AGENTS = [
    'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0)',
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.2)',
    'Opera/9.27 (Windows NT 5.2; U; zh-cn)',
    'Opera/8.0 (Macintosh; PPC Mac OS X; U; en)',
    'Mozilla/5.0 (Macintosh; PPC Mac OS X; U; en) Opera 8.0',
    'Mozilla/5.0 (Linux; U; Android 4.0.3; zh-cn; M032 Build/IML74K) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30',
    'Mozilla/5.0 (Windows; U; Windows NT 5.2) AppleWebKit/525.13 (KHTML, like Gecko) Chrome/0.2.149.27 Safari/525.13'
]


# 开代理服务器CCpory
PROXIES = [
    { "ip_port":"10.36.132.187:808","user_passwd" :None}
        #{"ip_prot" :"121.42.140.113:16816", "user_passwd" : ""}
        #{"ip_prot" :"121.42.140.113:16816", "user_passwd" : ""}
        #{"ip_prot" :"121.42.140.113:16816", "user_passwd" : ""}
]
```



