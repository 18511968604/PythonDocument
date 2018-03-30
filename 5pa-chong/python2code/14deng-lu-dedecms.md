```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import urllib
import cookielib

# 创建一个cookie对象
cookie = cookielib.LWPCookieJar()

# 提取cookie
hander_cookie = urllib2.HTTPCookieProcessor(cookie)

# 创建一个打开启
opener = urllib2.build_opener(hander_cookie)

# 安装一个打开器
urllib2.install_opener(opener)

url1 = "http://v57.demo.dedecms.com/dede/login.php"
url2 = "http://v57.demo.dedecms.com/dede/index.php"
# 打开第一个链接
urllib2.urlopen(url1)

# 请求头，模拟浏览器
header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}

postData = {"dopost": "login",
            "adminstyle": "newdedecms",
            "userid": "admin",
            "pwd": "admin",
            "sm1": ""}



# 编码
postData = urllib.urlencode(postData)

# post请求
request = urllib2.Request(url=url1, data=postData, headers=header)

response = urllib2.urlopen(request)

print response.read()

print "================================================"

responseNew = urllib2.urlopen(url=url2)
print responseNew.read()

```



