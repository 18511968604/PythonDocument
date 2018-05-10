```py
# -*- coding: utf-8 -*-
import scrapy


class MysinaSpider(scrapy.Spider):
    name = 'mysina'
    allowed_domains = ['news.sina.com.cn']
    start_urls = ['http://roll.news.sina.com.cn/news/gnxw/gdxw1/index_1.shtml']

    def parse(self, response):

        fourTitle = response.xpath("//ul[@class=\"list_009\"]//li")
        if len(fourTitle) != 0:
            for t in fourTitle:
                title = t.xpath("./a/text()").extract()[0]
                url = t.xpath("./a/@href").extract()[0]
                times = t.xpath("./span/text()").extract()[0]
                print(title, url, times)
                yield scrapy.Request(url, callback=self.get_content)

        else:
            pass

        # 实现翻页
        nexturl = response.xpath("//span[@class=\"pagebox_next\"]/a/@href").extract()[0]
        nexturl = nexturl[1:]
        print(nexturl)

        newUrl = "http://roll.news.sina.com.cn/news/gnxw/gdxw1" + nexturl
        print(newUrl)
        yield scrapy.Request(newUrl, callback=self.parse)


    def get_content(self,response):
        title = response.xpath("//h1[@class=\"main-title\"]/text()").extract()[0]
        print(title)
        # 提取正文
        content = response.xpath("//*[@id=\"article\"]//p/text()").extract()
        line = ""
        for c in content:
            line += str(c)
        print(line)
```



