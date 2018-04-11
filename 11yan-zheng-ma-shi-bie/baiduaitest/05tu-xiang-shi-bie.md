```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
from aip import AipImageClassify

""" 你的 APPID AK SK """
APP_ID = '9588288'
API_KEY = 'lchwrQNmq2868qS6rNFXYtNG'
SECRET_KEY = 'EvlfpbVnUqWjq22EVpuR6Rjd56xV8sWg'

client = AipImageClassify(APP_ID, API_KEY, SECRET_KEY)

""" 读取图片 """


def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()


image = get_file_content(r'C:\Users\Administrator\Desktop\test\c.jpg')

""" 调用菜品识别 """
client.dishDetect(image);

""" 如果有可选参数 """
options = {}
options["top_num"] = 3

""" 带参数调用菜品识别 """
print(client.dishDetect(image, options))

```



