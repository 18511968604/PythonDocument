# Scrapy Shell {#scrapy-shell}

Scrapy终端是一个交互终端，我们可以在未启动spider的情况下尝试及调试代码，也可以用来测试XPath或CSS表达式，查看他们的工作方式，方便我们爬取的网页中提取的数据。

如果安装了 IPython ，Scrapy终端将使用 IPython \(替代标准Python终端\)。 IPython 终端与其他相比更为强大，提供智能的自动补全，高亮输出，及其他特性。（推荐安装IPython）

## 启动Scrapy Shell {#启动scrapy-shell}

进入项目的根目录，执行下列命令来启动shell:

```
scrapy shell "https://hr.tencent.com/position.php?&start=0#a"
```



Scrapy Shell根据下载的页面会自动创建一些方便使用的对象，例如 Response 对象，以及`Selector 对象 (对HTML及XML内容)`。

* 当shell载入后，将得到一个包含response数据的本地 response 变量，输入`response.body`将输出response的包体，输出`response.headers`可以看到response的包头。

* 输入`response.selector`时， 将获取到一个response 初始化的类 Selector 的对象，此时可以通过使用`response.selector.xpath()`或`response.selector.css()`来对 response 进行查询。

* Scrapy也提供了一些快捷方式, 例如`response.xpath()`或`response.css()`同样可以生效（如之前的案例）。

## Selectors选择器 {#selectors选择器}

###### Scrapy Selectors 内置 XPath 和 CSS Selector 表达式机制 {#scrapy-selectors-内置-xpath-和-css-selector-表达式机制}

Selector有四个基本的方法，最常用的还是xpath:

* xpath\(\): 传入xpath表达式，返回该表达式所对应的所有节点的selector list列表
* extract\(\): 序列化该节点为Unicode字符串并返回list
* css\(\): 传入CSS表达式，返回该表达式所对应的所有节点的selector list列表，语法同 BeautifulSoup4
* re\(\): 根据传入的正则表达式对数据进行提取，返回Unicode字符串list列表

```
response.xpath('//title')
```



