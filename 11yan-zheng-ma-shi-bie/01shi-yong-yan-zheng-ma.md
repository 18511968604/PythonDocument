```py
# coding:utf-8
import os
import subprocess

import time

cmd = r"D:\Tesseract-OCR\tesseract.exe C:\Users\Administrator\Desktop\test\3.jpg test -l normal"
# p = os.popen(cmd)
time.sleep(3)
pp = subprocess.Popen(cmd)
with open("test.txt", 'r')as f:
    print(f.read())
```

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import os
import subprocess

import time

import requests
import lxml
import lxml.etree
from urllib import request

# cmd = r"D:\Tesseract-OCR\tesseract.exe C:\Users\Administrator\Desktop\test\3.jpg test -l normal"
# # p = os.popen(cmd)
# time.sleep(3)
# pp = subprocess.Popen(cmd)
# with open("test.txt", 'r')as f:
#     print(f.read())





html = requests.get("http://www.lmth2013.com/findpwd.aspx").content.decode("utf-8")

data = lxml.etree.HTML(html)
imagesrc = "http://www.lmth2013.com" + data.xpath("//*[@id=\"getcode\"]/@src")[0]

print(imagesrc)
request.urlretrieve(imagesrc, "image.jpg")
```



