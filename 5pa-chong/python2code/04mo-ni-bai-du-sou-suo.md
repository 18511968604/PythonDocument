### urllib.urlencode\(\) {#urlliburlencode}

##### urllib 和 urllib2 都是接受URL请求的相关模块，但是提供了不同的功能。两个最显著的不同如下： {#urllib-和-urllib2-都是接受url请求的相关模块，但是提供了不同的功能。两个最显著的不同如下：}

> * urllib 仅可以接受URL，不能创建 设置了headers 的Request 类实例；
>
> * 但是 urllib 提供`urlencode`方法用来GET查询字符串的产生，而 urllib2 则没有。（这是 urllib 和 urllib2 经常一起使用的主要原因）
>
> * 编码工作使用urllib的`urlencode()`函数，帮我们将`key:value`这样的键值对转换成`"key=value"`这样的字符串，解码工作可以使用urllib的`unquote()`函数。（注意，不是urllib2.urlencode\(\) \)

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import urllib


def getInfo(url):
    request = urllib2.Request(url)
    return urllib2.urlopen(request).read()


if __name__ == '__main__':
    url = "http://www.baidu.com/s"
    keyWord = {"wd": "谭"}
    kw = urllib.urlencode(keyWord)  # 编码
    # print kw
    # print urllib.unquote(kw)  # 解码
    newurl = url + "?" + kw
    print getInfo(newurl)
```



