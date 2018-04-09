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



