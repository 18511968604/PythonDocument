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
```

## Cookie {#cookie}

Cookie 是指某些网站服务器为了辨别用户身份和进行Session跟踪，而储存在用户浏览器上的文本文件，Cookie可以保持登录信息到用户下次与服务器的会话。

### Cookie原理 {#cookie原理}

HTTP是无状态的面向连接的协议, 为了保持连接状态, 引入了Cookie机制 Cookie是http消息头中的一种属性，包括：

```
Cookie名字（Name）
Cookie的值（Value）
Cookie的过期时间（Expires/Max-Age）
Cookie作用路径（Path）
Cookie所在域名（Domain），
使用Cookie进行安全连接（Secure）。

前两个参数是Cookie应用的必要条件，另外，还包括Cookie大小（Size，不同浏览器对Cookie个数及大小限制是有差异的）。
```

Cookie由变量名和值组成，根据 Netscape公司的规定，Cookie格式如下：

`Set－Cookie: NAME=VALUE；Expires=DATE；Path=PATH；Domain=DOMAIN_NAME；SECURE`

### Cookie应用 {#cookie应用}

Cookies在爬虫方面最典型的应用是判定注册用户是否已经登录网站，用户可能会得到提示，是否在下一次进入此网站时保留用户信息以便简化登录手续。

## cookielib库 和 HTTPCookieProcessor处理器 {#cookielib库-和-httpcookieprocessor处理器}

在Python处理Cookie，一般是通过`cookielib`模块和 urllib2模块的`HTTPCookieProcessor`处理器类一起使用。

> `cookielib`模块：主要作用是提供用于存储cookie的对象
>
> `HTTPCookieProcessor`处理器：主要作用是处理这些cookie对象，并构建handler对象。

### cookielib 库 {#cookielib-库}

该模块主要的对象有CookieJar、FileCookieJar、MozillaCookieJar、LWPCookieJar。

> * CookieJar：管理HTTP cookie值、存储HTTP请求生成的cookie、向传出的HTTP请求添加cookie的对象。整个cookie都存储在内存中，对CookieJar实例进行垃圾回收后cookie也将丢失。
>
> * FileCookieJar \(filename,delayload=None,policy=None\)：从CookieJar派生而来，用来创建FileCookieJar实例，检索cookie信息并将cookie存储到文件中。filename是存储cookie的文件名。delayload为True时支持延迟访问访问文件，即只有在需要时才读取文件或在文件中存储数据。
>
> * MozillaCookieJar \(filename,delayload=None,policy=None\)：从FileCookieJar派生而来，创建与`Mozilla浏览器 cookies.txt兼容`的FileCookieJar实例。
>
> * LWPCookieJar \(filename,delayload=None,policy=None\)：从FileCookieJar派生而来，创建与`libwww-perl标准的 Set-Cookie3 文件格式`兼容的FileCookieJar实例。

_**其实大多数情况下，我们只用CookieJar\(\)，如果需要和本地文件交互，就用 MozillaCookjar\(\) 或 LWPCookieJar\(\)**_

我们来做几个案例：

##### 1.获取Cookie，并保存到CookieJar\(\)对象中 {#1）获取cookie，并保存到cookiejar对象中}

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

##### 2. 访问网站获得cookie，并把获得的cookie保存在cookie文件中 {#2-访问网站获得cookie，并把获得的cookie保存在cookie文件中}

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import cookielib

filePath = "cookie.txt"

cookie = cookielib.LWPCookieJar(filePath)  # 设置保存路径
header = urllib2.HTTPCookieProcessor(cookie)
opner = urllib2.build_opener(header)
response = opner.open("http://www.baidu.com/")
cookie.save(ignore_discard=True, ignore_expires=True)  # 忽略错误
```

##### 3. 从文件中获取cookies，做为请求的一部分去访问 {#3-从文件中获取cookies，做为请求的一部分去访问}

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import urllib2
import cookielib

filePath = "cookie.txt"

cookie = cookielib.LWPCookieJar()  # 设置保存路径
cookie.load(filePath, ignore_discard=True, ignore_expires=True)  # 忽略错误
header = urllib2.HTTPCookieProcessor(cookie)
opner = urllib2.build_opener(header)
response = opner.open("http://www.baidu.com/")
print response.read()
```



