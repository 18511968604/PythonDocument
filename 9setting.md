# Settings {#settings}

Scrapy设置\(settings\)提供了定制Scrapy组件的方法。可以控制包括核心\(core\)，插件\(extension\)，pipeline及spider组件。比如 设置Json Pipeliine、LOG\_LEVEL等。

参考文档：[http://scrapy-chs.readthedocs.io/zh\_CN/1.0/topics/settings.html\#topics-settings-ref](http://scrapy-chs.readthedocs.io/zh_CN/1.0/topics/settings.html#topics-settings-ref)

## 内置设置参考手册 {#内置设置参考手册}

* **`BOT_NAME`**

  * 默认: 'scrapybot'

  * 当您使用 startproject 命令创建项目时其也被自动赋值。

* **`CONCURRENT_ITEMS`**

  * 默认: 100

  * Item Processor\(即 Item Pipeline\) 同时处理\(每个response的\)item的最大值。

* ##### `CONCURRENT_REQUESTS` {#concurrentrequests}

  * 默认: 16

  * Scrapy downloader 并发请求\(concurrent requests\)的最大值。
* **`REACTOR_THREADPOOL_MAXSIZE = 20`**

  * ##### 增加线程池数量 {#defaultrequestheaders}

* ##### `DEFAULT_REQUEST_HEADERS` {#defaultrequestheaders}

  * 默认: 如下

    ```
    {
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'en',
    }
    ```

    Scrapy HTTP Request使用的默认header。
* **`DEPTH_LIMIT`**

  * 默认: 0

  * 爬取网站最大允许的深度\(depth\)值。如果为0，则没有限制。

* ##### `DOWNLOAD_DELAY` {#downloaddelay}

  * 默认: 0

  * 下载器在下载同一个网站下一个页面前需要等待的时间。该选项可以用来限制爬取速度， 减轻服务器压力。同时也支持小数:

  `DOWNLOAD_DELAY = 0.25 # 250 ms of delay`

  * 默认情况下，Scrapy在两个请求间不等待一个固定的值， 而是使用0.5到1.5之间的一个随机值 \* DOWNLOAD\_DELAY 的结果作为等待间隔。

* **`DOWNLOAD_TIMEOUT`**

  * 默认: 180

  * 下载器超时时间\(单位: 秒\)。

* ##### `ITEM_PIPELINES` {#itempipelines}

  * 默认: {}

  * 保存项目中启用的pipeline及其顺序的字典。该字典默认为空，值\(value\)任意，不过值\(value\)习惯设置在0-1000范围内，值越小优先级越高。

    ```
    ITEM_PIPELINES = {
    'mySpider.pipelines.SomethingPipeline': 300,
    'mySpider.pipelines.ItcastJsonPipeline': 800,
    }
    ```
* **`LOG_FILE = "logname.txt"`**

* **`LOG_ENABLED`**

  * 默认: True

  * 是否启用logging。

* **`LOG_ENCODING`**

  * 默认: 'utf-8'

  * logging使用的编码。

* **`LOG_LEVEL`**

  * 默认: 'DEBUG'

  * log的最低级别。可选的级别有: CRITICAL、 ERROR、WARNING、INFO、DEBUG 。

* ##### `USER_AGENT` {#useragent}

  * 默认: "Scrapy/VERSION \(+[http://scrapy.org](http://scrapy.org)\)"

  * 爬取的默认User-Agent，除非被覆盖。
* ##### `PROXIES`： 代理设置 {#proxies：-代理设置}

  * 示例：

    ```
    PROXIES = [
      {'ip_port': '111.11.228.75:80', 'password': ''},
      {'ip_port': '120.198.243.22:80', 'password': ''},
      {'ip_port': '111.8.60.9:8123', 'password': ''},
      {'ip_port': '101.71.27.120:80', 'password': ''},
      {'ip_port': '122.96.59.104:80', 'password': ''},
      {'ip_port': '122.224.249.122:8088', 'password':''},
    ]
    ```
* ##### `COOKIES_ENABLED = False` {#cookiesenabled--false}

  * 禁用Cookies
* `IMAGES_STORE = '/path'`：下载图片，图片存储在文件中（一个图片一个文件），并使用它们URL的[SHA1 hash](http://en.wikipedia.org/wiki/SHA_hash_functions)作为文件名
* `IMAGES_EXPIRES = 30`：图片失效时间（天）,避免下载最近已经下载的图片

* ```
  # 图片缩略图
  IMAGES_THUMBS = {
      'samll' : (50, 50),
      'big' : (270, 270)
  }
  MEDIA_ALLOW_REDIRECTS = True # 重定向
  ```

```py
import scrapy
from scrapy.contrib.pipeline.images import ImagesPipeline
from scrapy.exceptions import DropItem

class MyImagesPipeline(ImagesPipeline):

    def get_media_requests(self, item, info):
        for image_url in item['image_urls']:
            yield scrapy.Request(image_url)

    def item_completed(self, results, item, info):
        image_paths = [x['path'] for ok, x in results if ok]
        if not image_paths:
            raise DropItem("Item contains no images")
        item['image_paths'] = image_paths
        return item


class MzituScrapyPipeline(ImagesPipeline):

    def file_path(self, request, response=None, info=None):
        """
        :param request: 每一个图片下载管道请求
        :param response:
        :param info:
        :param strip :清洗Windows系统的文件夹非法字符，避免无法创建目录
        :return: 每套图的分类目录
        """
        item = request.meta['item']
        folder = item['name']
        folder_strip = strip(folder)
        image_guid = request.url.split('/')[-1]
        filename = u'full/{0}/{1}'.format(folder_strip, image_guid)
        return filename

    def get_media_requests(self, item, info):
        """
        :param item: spider.py中返回的item
        :param info:
        :return:
        """
        for img_url in item['image_urls']:
            referer = item['url']
            yield Request(img_url, meta={'item': item})


    def item_completed(self, results, item, info):
        image_paths = [x['path'] for ok, x in results if ok]
        if not image_paths:
            raise DropItem("Item contains no images")
        return item
```



