### 1、定义items

```py
import scrapy


class SinaItem(scrapy.Item):

    # 文件夹层级
    level1 = scrapy.Field()
    level2 = scrapy.Field()
    level3 = scrapy.Field()

    title = scrapy.Field()  # 文章标题
    content = scrapy.Field()  # 文章内容
```

### 2、改写pipeline

```py
# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: http://doc.scrapy.org/en/latest/topics/item-pipeline.html


class SinaPipeline(object):
    def process_item(self, item, spider):
        filepath = "C:\\Users\\Administrator\\Desktop\\image\\data" + "\\" \
                   + item["level1"].strip() + "\\" + item["level2"].strip() + "\\" + item["level3"].strip()

        filename = item["title"] + ".txt"
        filename = filepath + "\\" + filename
        with open(filename, 'w', encoding='utf-8', errors='ignore') as f:
            f.write(str(item["content"]))
            f.flush()

        return item

```

### 3、重写scrapy

```py
# -*- coding: utf-8 -*-
import scrapy
import urllib
import urllib.request
import lxml
import lxml.etree
from sina import items


class MysinaSpider(scrapy.Spider):
    name = 'mysina'
    allowed_domains = ['news.sina.com.cn']
    start_urls = ['http://news.sina.com.cn/guide/']

    def parse(self, response):
        parent = response.xpath("//div[@class=\"clearfix\"]")

        for title in parent:
            # 创建sinaItems
            sinaItems = items.SinaItem()
            level1 = ""
            level2 = ""
            level3 = ""

            # 提取一级标题
            level1 = title.xpath("./h3/a/text() | ./h3/span/text()").extract()[0]
            print(level1)

            # 提取二级标题
            for t in title.xpath("./ul//li"):
                level2 = t.xpath("./a/text()").extract()[0]
                secondTitleUrl = t.xpath("./a/@href").extract()[0]
                print("----", level2)

                # 三级标题
                url = secondTitleUrl
                data = urllib.request.urlopen(url).read().decode("utf-8", "ignore")
                mytree = lxml.etree.HTML(data)

                threeTitle = mytree.xpath("//div[@class=\"links\"]//a")

                for t in threeTitle:
                    level3 = t.xpath("./text()")[0]
                    newurl = t.xpath("./@href")[0]
                    print('--------', level3)

                    sinaItems["level1"] = level1
                    sinaItems["level2"] = level2
                    sinaItems["level3"] = level3

                    # 使用meta传送数据

                    yield scrapy.Request(newurl, meta={"meta1": sinaItems}, callback=self.parse_leve3)



    def parse_leve3(self, response):

        # 接受数据
        sianItems = response.meta['meta1']

        fourTitle = response.xpath("//ul[@class=\"list_009\"]//li")
        if len(fourTitle) != 0:
            for t in fourTitle:
                title = t.xpath("./a/text()").extract()[0]
                url = t.xpath("./a/@href").extract()[0]
                times = t.xpath("./span/text()").extract()[0]
                print(title, url, times)
                yield scrapy.Request(url, meta={"meta2": sianItems}, callback=self.get_content)

        else:
            pass

        # 实现翻页
        nexturl = response.xpath("//span[@class=\"pagebox_next\"]/a/@href").extract()[0]
        nexturl = nexturl[1:]
        print(nexturl)

        newUrl = "http://roll.news.sina.com.cn/news/gnxw/gdxw1" + nexturl
        print(newUrl)
        yield scrapy.Request(newUrl, meta={"meta1": sianItems}, callback=self.parse)

    def get_content(self, response):

        # 接受数据
        sianItems = response.meta['meta2']

        title = response.xpath("//h1[@class=\"main-title\"]/text() | //h1[@id=\"artibodyTitle\"]/text()").extract()[0]
        print(title)
        # 提取正文
        sianItems["title"] = title

        content = response.xpath("//*[@id=\"article\"]//p/text()").extract()
        line = ""
        for c in content:
            line += str(c)
        print(line)
        sianItems["content"] = line

        yield sianItems

```



