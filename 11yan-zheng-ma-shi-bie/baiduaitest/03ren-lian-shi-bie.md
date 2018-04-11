```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
from aip import AipFace

""" 你的 APPID AK SK """
APP_ID = '9588288'
API_KEY = 'lchwrQNmq2868qS6rNFXYtNG'
SECRET_KEY = 'EvlfpbVnUqWjq22EVpuR6Rjd56xV8sWg'
""" 读取图片 """

client = AipFace(APP_ID, API_KEY, SECRET_KEY)


def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()


image = get_file_content(r'C:\Users\Administrator\Desktop\test\wu.jpg')

""" 调用人脸检测 """
client.detect(image)

""" 如果有可选参数 """
options = {}

options["face_fields"] = "beauty"
options["face_fields"] = "age"

""" 带参数调用人脸检测 """
result = client.detect(image, options)
print(result)

```



