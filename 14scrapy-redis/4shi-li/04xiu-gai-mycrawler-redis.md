### 添加/example/redis\_client.py

### 使用带密码访问redis数据库

修改setting文件

```
REDIS_URL = "redis://:123456@localhost:6379"

```

```py

#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import redis

myredis = redis.Redis(host="127.0.0.1", password="123456", port=6379)
print(myredis.info())
url = "https://baike.baidu.com/item/Python/407313"
myredis.lpush("baike_redis:start_urls", url)
```

```py
from bs4 import BeautifulSoup
from scrapy.spiders import Rule
from scrapy.linkextractors import LinkExtractor
from scrapy_redis.spiders import RedisMixin
from scrapy.spiders import CrawlSpider
from scrapy_redis.spiders import RedisCrawlSpider
from example import items


class MyCrawler(RedisCrawlSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'mybaike_redis'
    redis_key = 'baike:start_urls'

    rules = [Rule(LinkExtractor(allow=("item/.*")), callback="parse_page", follow=True)]

    def set_crawler(self, crawer):
        CrawlSpider.set_crawler(self, crawer)  # 设置默认爬去
        RedisMixin.setup_redis(self)  # url由redis

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



