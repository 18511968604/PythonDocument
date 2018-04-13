## 源码自带项目说明： {#源码自带项目说明：}

### 使用scrapy-redis的example来修改 {#使用scrapyredis的example来修改}

先从github上拿到scrapy-redis的示例，然后将里面的example-project目录移到指定的地址：

```
# clone github scrapy-redis源码文件
git clone https://github.com/rolando/scrapy-redis.git

# 直接拿官方的项目范例，改名为自己的项目用（针对懒癌患者)
mv scrapy-redis/example-project ~/scrapyredis-project
```

我们clone到的 scrapy-redis 源码中有自带一个example-project项目，这个项目包含3个spider，分别是dmoz, myspider\_redis，mycrawler\_redis。

### 一、dmoz \(class DmozSpider\(CrawlSpider\)\) {#一、dmoz-class-dmozspidercrawlspider}

这个爬虫继承的是CrawlSpider，它是用来说明Redis的持续性，当我们第一次运行dmoz爬虫，然后Ctrl + C停掉之后，再运行dmoz爬虫，之前的爬取记录是保留在Redis里的。

分析起来，其实这就是一个 scrapy-redis 版`CrawlSpider`类，需要设置Rule规则，以及callback不能写parse\(\)方法。

#### 执行方式：`scrapy crawl dmoz` {#执行方式：scrapy-crawl-dmoz}

```py
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule


class DmozSpider(CrawlSpider):
    """Follow categories and extract links."""
    name = 'dmoz'
    allowed_domains = ['dmoz.org']
    start_urls = ['http://www.dmoz.org/']

    rules = [
        Rule(LinkExtractor(
            restrict_css=('.top-cat', '.sub-cat', '.cat-item')
        ), callback='parse_directory', follow=True),
    ]

    def parse_directory(self, response):
        for div in response.css('.title-and-desc'):
            yield {
                'name': div.css('.site-title::text').extract_first(),
                'description': div.css('.site-descr::text').extract_first().strip(),
                'link': div.css('a::attr(href)').extract_first(),
            }
```

### 二、myspider\_redis \(class MySpider\(RedisSpider\)\) {#二、myspiderredis-class-myspiderredisspider}

这个爬虫继承了RedisSpider， 它能够支持分布式的抓取，采用的是basic spider，需要写parse函数。

其次就是不再有start\_urls了，取而代之的是redis\_key，scrapy-redis将key从Redis里pop出来，成为请求的url地址。

```py
from scrapy_redis.spiders import RedisSpider


class MySpider(RedisSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'myspider_redis'

    # 注意redis-key的格式：
    redis_key = 'myspider:start_urls'

    # 可选：等效于allowd_domains()，__init__方法按规定格式写，使用时只需要修改super()里的类名参数即可
    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('domain', '')
        self.allowed_domains = filter(None, domain.split(','))

        # 修改这里的类名为当前类名
        super(MySpider, self).__init__(*args, **kwargs)

    def parse(self, response):
        return {
            'name': response.css('title::text').extract_first(),
            'url': response.url,
        }
```

#### 注意： {#注意：}

RedisSpider类 不需要写`allowd_domains`和`start_urls`：

1. scrapy-redis将从在构造方法`__init__()`里动态定义爬虫爬取域范围，也可以选择直接写`allowd_domains`。

2. 必须指定redis\_key，即启动爬虫的命令，参考格式：`redis_key = 'myspider:start_urls'`

3. 根据指定的格式，`start_urls`将在 Master端的 redis-cli 里 lpush 到 Redis数据库里，RedisSpider 将在数据库里获取start\_urls。

#### 执行方式： {#执行方式：}

1. 通过runspider方法执行爬虫的py文件（也可以分次执行多条），爬虫（们）将处于等待准备状态：

   #### `scrapy runspider myspider_redis.py` {#scrapy-runspider-myspiderredispy}

2. 在Master端的redis-cli输入push指令，参考格式：

   #### `$redis > lpush myspider:start_urls http://www.dmoz.org/` {#redis--lpush-myspiderstarturls-httpwwwdmozorg}

3. Slaver端爬虫获取到请求，开始爬取。

   #### `lrange  mycrawler:start_url 0 -1`

### 三、mycrawler\_redis \(class MyCrawler\(RedisCrawlSpider\)\) {#三、mycrawlerredis-class-mycrawlerrediscrawlspider}

这个RedisCrawlSpider类爬虫继承了RedisCrawlSpider，能够支持分布式的抓取。因为采用的是crawlSpider，所以需要遵守Rule规则，以及callback不能写parse\(\)方法。

同样也不再有start\_urls了，取而代之的是redis\_key，scrapy-redis将key从Redis里pop出来，成为请求的url地址。

```py
from scrapy.spiders import Rule
from scrapy.linkextractors import LinkExtractor

from scrapy_redis.spiders import RedisCrawlSpider


class MyCrawler(RedisCrawlSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'mycrawler_redis'
    redis_key = 'mycrawler:start_urls'

    rules = (
        # follow all links
        Rule(LinkExtractor(), callback='parse_page', follow=True),
    )

    # __init__方法必须按规定写，使用时只需要修改super()里的类名参数即可
    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('domain', '')
        self.allowed_domains = filter(None, domain.split(','))

        # 修改这里的类名为当前类名
        super(MyCrawler, self).__init__(*args, **kwargs)

    def parse_page(self, response):
        return {
            'name': response.css('title::text').extract_first(),
            'url': response.url,
        }
```

#### 注意： {#注意：}

同样的，RedisCrawlSpider类不需要写`allowd_domains`和`start_urls`：

1. scrapy-redis将从在构造方法`__init__()`里动态定义爬虫爬取域范围，也可以选择直接写`allowd_domains`。

2. 必须指定redis\_key，即启动爬虫的命令，参考格式：`redis_key = 'myspider:start_urls'`

3. 根据指定的格式，`start_urls`将在 Master端的 redis-cli 里 lpush 到 Redis数据库里，RedisSpider 将在数据库里获取start\_urls。

#### 执行方式： {#执行方式：}

1. 通过runspider方法执行爬虫的py文件（也可以分次执行多条），爬虫（们）将处于等待准备状态：

   #### `scrapy runspider mycrawler_redis.py` {#scrapy-runspider-mycrawlerredispy}

2. 在Master端的redis-cli输入push指令，参考格式：

   #### `$redis > lpush mycrawler:start_urls http://www.dmoz.org/` {#redis--lpush-mycrawlerstarturls-httpwwwdmozorg}

3. 爬虫获取url，开始执行。

## 总结： {#总结：}

1. 如果只是用到Redis的去重和保存功能，就选第一种；

2. 如果要写分布式，则根据情况，选择第二种、第三种；

3. 通常情况下，会选择用第三种方式编写深度聚焦爬虫。



