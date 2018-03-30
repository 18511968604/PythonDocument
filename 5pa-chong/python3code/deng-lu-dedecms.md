```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import urllib
from urllib import request, parse
from http import cookiejar

# 创建一个cookie对象
cookie = cookiejar.CookieJar()

# 提取cookie
hander_cookie = urllib.request.HTTPCookieProcessor(cookie)

# 创建一个打开启
opener = urllib.request.build_opener(hander_cookie)

# 安装一个打开器
urllib.request.install_opener(opener)

url1 = "http://v57.demo.dedecms.com/dede/login.php"
url2 = "http://v57.demo.dedecms.com/dede/index.php"
# 打开第一个链接
urllib.request.urlopen(url1)

# 请求头，模拟浏览器
header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}

postData = {"dopost": "login",
            "adminstyle": "newdedecms",
            "userid": "admin",
            "pwd": "admin",
            "sm1": ""}



# 编码
postData = urllib.parse.urlencode(postData).encode('utf-8')

# post请求
req = urllib.request.Request(url=url1, data=postData, headers=header)

response = urllib.request.urlopen(req)

print(response.read().decode("utf-8"))

print("================================================")

responseNew = urllib.request.urlopen(url=url2)
print(responseNew.read().decode("utf-8"))

```



