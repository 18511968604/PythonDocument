# 入门案例 {#入门案例}

## 学习目标 {#学习目标}

* 创建一个Scrapy项目
* 定义提取的结构化数据\(Item\)
* 编写爬取网站的 Spider 并提取出结构化数据\(Item\)
* 编写 Item Pipelines 来存储提取到的Item\(即结构化数据\)

## 一. 新建项目\(scrapy startproject\) {#一-新建项目scrapy-startproject}

* 在开始爬取之前，必须创建一个新的Scrapy项目。进入自定义的项目目录中，运行下列命令：

```
scrapy startproject mySpider
```

* 其中， mySpider 为项目名称，可以看到将会创建一个 mySpider 文件夹，目录结构大致如下：

![](../images/7.2.png)![](/assets/myscrapy.png)

下面来简单介绍一下各个主要文件的作用：

> scrapy.cfg ：项目的配置文件
>
> mySpider/ ：项目的Python模块，将会从这里引用代码
>
> mySpider/items.py ：项目的目标文件
>
> mySpider/pipelines.py ：项目的管道文件
>
> mySpider/settings.py ：项目的设置文件
>
> mySpider/spiders/ ：存储爬虫代码目录

## 二、明确目标\(mySpider/items.py\) {#二、明确目标myspideritemspy}

我们打算抓取：[http://bbs.tianya.cn/post-140-393977-1.shtml网站里的邮箱。](http://bbs.tianya.cn/post-140-393977-1.shtml网站里的邮箱。)

1. 打开mySpider目录下的items.py

2. Item 定义结构化数据字段，用来保存爬取到的数据，有点像Python中的dict，但是提供了一些额外的保护减少错误。

3. 可以通过创建一个 scrapy.Item 类， 并且定义类型为 scrapy.Field的类属性来定义一个Item（可以理解成类似于ORM的映射关系）。

4. 接下来，创建一个ItcastItem 类，和构建item模型（model）。

```py
import scrapy


class TianyaItem(scrapy.Item):
    email = scrapy.Field()
```

## 三、制作爬虫 （spiders/itcastSpider.py） {#三、制作爬虫-（spidersitcastspiderpy）}

**爬虫功能要分两步：**

### 1. 爬数据 {#1-爬数据}

* 在当前目录下输入命令，将在
  `mySpider/spider`
  目录下创建一个名为
  `itcast`
  的爬虫，并指定爬取域的范围：

```
scrapy genspider mytianya "bbs.tianya.cn"
```

* 打开 mySpider/spider目录里的 mytianya .py，默认增加了下列代码:

```py
import scrapy
import re
from tianya import items


class MytianyaSpider(scrapy.Spider):
    name = 'mytianya'
    allowed_domains = ['bbs.tianya.cn']
    start_urls = ['http://bbs.tianya.cn/post-140-393977-1.shtml']

    def parse(self, response):
        pass
```

###### 其实也可以由我们自行创建itcast.py并编写上面的代码，只不过使用命令可以免去编写固定代码的麻烦 {#其实也可以由我们自行创建itcastpy并编写上面的代码，只不过使用命令可以免去编写固定代码的麻烦}

要建立一个Spider， 你必须用scrapy.Spider类创建一个子类，并确定了三个强制的属性 和 一个方法。

* `name = ""`：这个爬虫的识别名称，必须是唯一的，在不同的爬虫必须定义不同的名字。

* `allow_domains = []`是搜索的域名范围，也就是爬虫的约束区域，规定爬虫只爬取这个域名下的网页，不存在的URL会被忽略。

* `start_urls = ()`：爬取的URL元祖/列表。爬虫从这里开始抓取数据，所以，第一次下载的数据将会从这些urls开始。其他子URL将会从这些起始URL中继承性生成。

* `parse(self, response)`：解析的方法，每个初始URL完成下载后将被调用，调用的时候传入从每一个URL传回的Response对象来作为唯一参数，主要作用如下：

  1. 负责解析返回的网页数据\(response.body\)，提取结构化数据\(生成item\)
  2. 生成需要下一页的URL请求。

##### 将start\_urls的值修改为需要爬取的第一个url {#将starturls的值修改为需要爬取的第一个url}

##### 修改parse\(\)方法 {#修改parse方法}

```py
    def parse(self, response):
        html = response.body.decode()
        # ftsd@21cn.com
        email = re.compile(r"([A-Z0-9_]+@[A-Z0-9]+.[A-Z]{2,4})", re.I)
        emailList = email.findall(html)
        mydict = []
        for e in emailList:
            item = items.TianyaItem()
            item["email"] = e
            # mydict[e] = "http://bbs.tianya.cn/post-140-393977-1.shtml"
            mydict.append(item)
        return mydict
```

然后运行一下看看，在mySpider目录下执行：

```
scrapy crawl mytianya
```

### 2. 取数据 {#2-取数据}

* 我们暂时先不处理管道，后面会详细介绍。

## 保存数据 {#保存数据}

##### scrapy保存信息的最简单的方法主要有四种，-o 输出指定格式的文件，，命令如下： {#scrapy保存信息的最简单的方法主要有四种，o-输出指定格式的文件，，命令如下：}

```
scrapy crawl mytianya-o mytianya.json



scrapy crawl mytianya-o mytianya.jsonl



scrapy crawl mytianya-o mytianya.csv



scrapy crawl mytianya-o mytianya.xml
```

---

## 思考 {#思考}

#### 如果将代码改成下面形式，结果完全一样。 {#如果将代码改成下面形式，结果完全一样。}

#### 请思考 yield 在这里的作用： {#请思考-yield-在这里的作用：}

```py
    def parse(self, response):
        html = response.body.decode()
        # ftsd@21cn.com
        email = re.compile(r"([A-Z0-9_]+@[A-Z0-9]+.[A-Z]{2,4})", re.I)
        emailList = email.findall(html)
        mydict = []
        for e in emailList:
            item = items.TianyaItem()
            item["email"] = e
            # mydict[e] = "http://bbs.tianya.cn/post-140-393977-1.shtml"

            # mydict.append(item)

            #将获取的数据交给pipelines
            yield mydict

        # 返回数据，不经过pipeline
        return mydict
```



