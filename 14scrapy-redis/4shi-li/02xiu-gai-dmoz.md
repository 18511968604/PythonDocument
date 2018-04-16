### 1、修改items

```py
class BaidubaikeItem(Item):
    # define the fields for your item here like:
    url = Field()
    masterTitle = Field()
    secondTitle = Field()
    content = Field()
    # crawled = Field()  # 什么时间抓取的
    # spider = Field()  # 谁抓取的
```

### 2、修改pipelines

```py
class ExamplePipeline(object):
    def __init__(self):
        self.file = open("tencent.txt", "w", encoding="utf-8")

    def process_item(self, item, spider):

        self.file.write(str(item) + "\r\n")
        self.file.flush()
        print(item)

        return item

    def __del__(self):
        self.file.close()
```

### 3、修改setting

## 4/

```py
# -*- coding: utf-8 -*-

import scrapy
from bs4 import BeautifulSoup
from example import items
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
        masterTitle = soup.select(".lemmaWgt-lemmaTitle-title > h1")

        if len(masterTitle) == 0:
            masterTitle = soup.select(".lemma-title-container > span")[0].get_text()
        else:
            masterTitle = masterTitle[0].get_text()
        secondTitle = soup.select(".lemmaWgt-lemmaTitle-title > h2")

        if len(secondTitle) == 0:
            secondTitle = "锁定"
        else:
            secondTitle = secondTitle[0].get_text()

        # print(masterTitle, secondTitle)
        # 获取文本
        content = soup.find_all("div", class_="lemma-summary")
        if len(content) == 0:
            content = soup.find_all("div", class_="summary-content")[0].get_text()
        else:
            content = content[0].get_text()
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



