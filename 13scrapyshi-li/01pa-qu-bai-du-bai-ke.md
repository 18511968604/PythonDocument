```py
# -*- coding: utf-8 -*-

import scrapy
from bs4 import BeautifulSoup
from baidubaike import items
from scrapy.spiders import CrawlSpider, Rule  # 爬取规则
from scrapy.linkextractors import LinkExtractor  # 提取超链接


class MybaikeSpider(CrawlSpider):
    name = 'mybaike'
    allowed_domains = ['baike.baidu.com']
    start_urls = ['https://baike.baidu.com/item/Python/407313']

    rules = [Rule(LinkExtractor(allow=("item/.*")), callback="parse_page", follow=True)]

    # 获取页面信息
    def getInf(self, pagedata):
        soup = BeautifulSoup(pagedata, "lxml")

        # 获取主标题和副标题
        masterTitle = soup.select(".lemmaWgt-lemmaTitle-title > h1")[0].get_text()
        secondTitle = soup.select(".lemmaWgt-lemmaTitle-title > h2")[0].get_text()

        # print(masterTitle, secondTitle)
        # 获取文本
        content = soup.find_all("div", class_="lemma-summary")[0].get_text()
        # print(content)
        if len(masterTitle) == 0:
            masterTitle, secondTitle, content = '没有'

        return masterTitle, secondTitle, content

    def parse_page(self, response):
        result = self.getInf(response.body)
        item = items.BaidubaikeItem()
        item["url"] = response.url
        item["masterTitle"] = result[0]
        item["secondTitle"] = result[1]
        item["content"] = result[2]
        yield item
```

items.py

```py
class BaidubaikeItem(scrapy.Item):
    # define the fields for your item here like:
    url = scrapy.Field()
    masterTitle = scrapy.Field()
    secondTitle = scrapy.Field()
    content = scrapy.Field()
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

middlewares.py

```py
from scrapy import signals
import random
import base64 #编码
from .settings import USER_AGENTS #导入浏览器模拟，导入代理
from .settings import PROXIES
#随机浏览器
class  RandomUserAgent:
    def process_request(self,request,spider):
        useragent=random.choice(USER_AGENTS)#随机选择一个代理
        request.headers.setdefault("User-Agent",useragent) #代理
#随机代理
class  RandomProxy:
    def process_request(self,request,spider):
        proxy=random.choice(PROXIES)
        if proxy["user_passwd"] is None:
            request.meta["proxy"]="http://"+proxy["ip_port"] #没有代理密码就直接登陆
        else:
            #账户密码进程编码
            base64_userpassword=base64.b64encode(proxy["user_passwd"].encode("utf-8") )
            request.headers["Proxy-Authorization"]="Basic "+ base64_userpassword.decode("utf-8")
            request.meta["proxy"]="http://"+proxy["ip_port"]
```



