

# Handler处理器 和 自定义Opener {#handler处理器-和-自定义opener}

* opener是 urllib2.OpenerDirector 的实例，我们之前一直都在使用的urlopen，它是一个特殊的opener（也就是模块帮我们构建好的）。

* 但是基本的urlopen\(\)方法不支持代理、cookie等其他的HTTP/HTTPS高级功能。所以要支持这些功能：

  1. 使用相关的
     `Handler处理器`
     来创建特定功能的处理器对象；
  2. 然后通过
     `urllib2.build_opener()`
     方法使用这些处理器对象，创建自定义opener对象；
  3. 使用自定义的opener对象，调用
     `open()`
     方法发送请求。

* 如果程序里所有的请求都使用自定义的opener，可以使用`urllib2.install_opener()`将自定义的 opener 对象 定义为 全局opener，表示如果之后凡是调用urlopen，都将使用这个opener（根据自己的需求来选择）

## 简单的自定义opener\(\) {#简单的自定义opener}

```py
import urllib2

# 构建一个HTTPHandler 处理器对象，支持处理HTTP请求
http_handler = urllib2.HTTPHandler()

# 构建一个HTTPHandler 处理器对象，支持处理HTTPS请求
# http_handler = urllib2.HTTPSHandler()

# 调用urllib2.build_opener()方法，创建支持处理HTTP请求的opener对象
opener = urllib2.build_opener(http_handler)

# 构建 Request请求
request = urllib2.Request("http://www.baidu.com/")

# 调用自定义opener对象的open()方法，发送request请求
response = opener.open(request)

# 获取服务器响应内容
print response.read()
```

这种方式发送请求得到的结果，和使用`urllib2.urlopen()`发送HTTP/HTTPS请求得到的结果是一样的。

如果在 HTTPHandler\(\)增加`debuglevel=1`参数，还会将 Debug Log 打开，这样程序在执行的时候，会把收包和发包的报头在屏幕上自动打印出来，方便调试，有时可以省去抓包的工作。

```
# 仅需要修改的代码部分：

# 构建一个HTTPHandler 处理器对象，支持处理HTTP请求，同时开启Debug Log，debuglevel 值默认 0
http_handler = urllib2.HTTPHandler(debuglevel=1)

# 构建一个HTTPHSandler 处理器对象，支持处理HTTPS请求，同时开启Debug Log，debuglevel 值默认 0
https_handler = urllib2.HTTPSHandler(debuglevel=1)
```

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import cookielib

# 创建一个对象存储cookie
cookie = cookielib.CookieJar()
# 提取cookie
header = urllib2.HTTPCookieProcessor(cookie)
# 处理 cookie
opener = urllib2.build_opener(header)
response = opener.open("http://www.baidu.com/")
cookies = ""
for c in cookie:
    print c
for data in cookie:
    cookies = cookies + data.name + "=" + data.value + "\r\n"

print cookies
```



