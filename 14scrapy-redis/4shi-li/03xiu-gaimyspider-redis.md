```py
from scrapy_redis.spiders import RedisSpider
from example import items


class TencentSpider(RedisSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'mybaike'
    redis_key = 'baike:start_urls'


    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('https://baike.baidu.com', '')
        self.allowed_domains = filter(None, domain.split(','))
        super(TencentSpider, self).__init__(*args, **kwargs)

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

    def parse(self, response):
        result = self.getInf(response.body)
        item = items.BaidubaikeItem()
        item["url"] = response.url
        item["masterTitle"] = result[0]
        item["secondTitle"] = result[1]
        item["content"] = result[2]
        yield item
```

## 2、在Master端的redis-cli输⼊push指令，参考格式

```
$redis> lpush baike:start_urls https://baike.baidu.com/item/Python/407313

$redis> lrange baike:start_urls 0 -1

$redis> keys *
```



