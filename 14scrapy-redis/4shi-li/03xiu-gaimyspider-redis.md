```py
from scrapy_redis.spiders import RedisSpider
from example import items


class TencentSpider(RedisSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'myTencent_redis'
    redis_key = 'myTencent_redis:start_urls'


    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('http://hr.tencent.com', '')
        self.allowed_domains = filter(None, domain.split(','))
        super(TencentSpider, self).__init__(*args, **kwargs)

    def parse(self, response):
        for data in response.xpath("//tr[@class=\"even\"] | //tr[@class=\"odd\"]"):
            item = items.TencentItem()
            item["jobTitle"] = data.xpath("./td[1]/a/text()")[0].extract()
            item["jobLink"] = "https://hr.tencent.com/" + data.xpath("./td[1]/a/@href")[0].extract()
            item["jobCategories"] = data.xpath("./td[1]/a/text()")[0].extract()
            item["number"] = data.xpath("./td[2]/text()")[0].extract()
            item["location"] = data.xpath("./td[3]/text()")[0].extract()
            item["releasetime"] = data.xpath("./td[4]/text()")[0].extract()

            yield item

```



