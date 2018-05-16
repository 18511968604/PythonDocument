## 1.items.py

```py
import scrapy


class BiqugeItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    level1 = scrapy.Field()
    level1Url = scrapy.Field()
    level2 = scrapy.Field()
    level2Url = scrapy.Field()
    chapter = scrapy.Field()
    chapterUrl = scrapy.Field()
    content = scrapy.Field()
```

## 2.pipelines.py

```py
import os

rootdir = r"C:\Users\Administrator\Desktop\my\szpython1801\spiderDemo\w2\biquge5200\data"


class BiqugePipeline(object):
    def process_item(self, item, spider):
        level1dir = rootdir + "\\" + item['level1']

        if not os.path.exists(level1dir):
            os.mkdir(level1dir)
        else:
            level2dir = level1dir + "\\" + item["level2"]
            if not os.path.exists(level2dir):
                os.mkdir(level2dir)
            else:
                filepath = level2dir + '\\' + item['chapter'] + '.txt'
                with open(filepath, 'w', encoding='utf-8', errors='ignore') as f:
                    f.write(str(item['content']))
                    f.flush()

        return item
```

3.spider

```py
# -*- coding: utf-8 -*-
import scrapy
from scrapy.spiders import CrawlSpider, Rule
from scrapy.linkextractors import LinkExtractor
import urllib
import urllib.request

import lxml
import lxml.etree

from biquge import items


# class MybiqugeSpider(CrawlSpider):
#     name = 'mybiquge'
#     allowed_domains = ['biquge5200.cc']
#     start_urls = ['http://www.biquge5200.cc/2_2598/1847694.html']
#
#     rules = [Rule(LinkExtractor(allow=("(\d+).html")), callback="get_c", follow=True)]
#
#     def get_c(self, response):
#         title = response.xpath("//div[@class=\"bookname\"]/h1/text()").extract()[0]
#         print(title)
#         contentList = response.xpath("//*[@id=\"content\"]//p")
#         content = ''
#         for i in contentList:
#             content += i.xpath("./text()").extract()[0].replace("\u3000", '')
#         print(content)

class MybiqugeSpider(scrapy.Spider):
    name = 'mybiquge'
    allowed_domains = ['biquge5200.cc']
    start_urls = ['http://www.biquge5200.cc/']

    def parse(self, response):
        level = response.xpath("//*[@class=\"nav\"]/ul//li")

        itemList = []

        for i in level:
            item = items.BiqugeItem()

            print(i.xpath("./a/text()").extract()[0])
            print(i.xpath("./a/@href").extract()[0][2:])
            url = "http://" + i.xpath("./a/@href").extract()[0][2:]
            level1 = i.xpath("./a/text()").extract()[0]
            item["level1"] = level1
            item['level1Url'] = url
            itemList.append(item)
            for item in itemList:
                yield scrapy.Request(url=item['level1Url'], meta={"meta1": item}, callback=self.get_level2)

    def get_level2(self, response):

        level = response.xpath("//div[@class=\"r\"]/ul//li")
        item = response.meta['meta1']
        itemList = []
        for i in level:
            print("----", i.xpath("./span/a/text()").extract()[0])
            print("----", i.xpath("./span/a/@href").extract()[0])
            print("----", i.xpath("./span[2]/text()").extract()[0])
            url = i.xpath("./span/a/@href").extract()[0]

            item['level2'] = i.xpath("./span/a/text()").extract()[0]
            item['level2Url'] = url
            itemList.append(item)
            for item in itemList:
                yield scrapy.Request(url=item['level2Url'], meta={"meta2": item}, callback=self.get_chapter)

    def get_chapter(self, response):
        item = response.meta['meta2']
        itemList = []
        chapter = response.xpath("//*[@id=\"list\"]/dl//dd")
        for i in chapter:
            print("--------", i.xpath("./a/text()").extract()[0])
            print("--------", i.xpath("./a/@href").extract()[0])
            url = i.xpath("./a/@href").extract()[0]

            item['chapter'] = i.xpath("./a/text()").extract()[0]
            item['chapterUrl'] = url
            itemList.append(item)

            for item in itemList:
                yield scrapy.Request(url=item['chapterUrl'], meta={"meta3": item}, callback=self.get_content)

    def get_content(self, response):
        print("============")
        item = response.meta['meta3']
        title = response.xpath("//div[@class=\"bookname\"]/h1/text()").extract()[0]
        print(title)
        contentList = response.xpath("//*[@id=\"content\"]//p")
        content = ''
        for i in contentList:
            content += i.xpath("./text()").extract()[0].replace("\u3000", '')
        print(content)
        item['content'] = content
        yield item

```



