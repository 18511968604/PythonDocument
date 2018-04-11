```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
from aip import AipNlp

""" 你的 APPID AK SK """
APP_ID = '9588288'
API_KEY = 'lchwrQNmq2868qS6rNFXYtNG'
SECRET_KEY = 'EvlfpbVnUqWjq22EVpuR6Rjd56xV8sWg'

client = AipNlp(APP_ID, API_KEY, SECRET_KEY)

text = "百度是一家高科技公司"

""" 调用词法分析 """
result = client.lexer(text)
print(result)


word = "兄弟"

""" 调用词向量表示 """
print(client.wordEmbedding(word))

text = "iloveyou"

""" 调用DNN语言模型 """
print(client.dnnlm(text))


word1 = "爱"

word2 = "喜欢"

""" 调用词义相似度 """
client.wordSimEmbedding(word1, word2)

""" 如果有可选参数 """
options = {}

""" 带参数调用词义相似度 """
print(client.wordSimEmbedding(word1, word2, options))




text1 = "你吃了吗"

text2 = "你真的吃了吗"

""" 调用短文本相似度 """
client.simnet(text1, text2)

""" 如果有可选参数 """
options = {}
options["model"] = "CNN"

""" 带参数调用短文本相似度 """
print(client.simnet(text1, text2, options))



```



