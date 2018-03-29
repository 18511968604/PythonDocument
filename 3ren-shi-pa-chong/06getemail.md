```py
#! C:\Python36\python.exe
'''
爬取页面邮箱
'''
import csv
import re

import time

from SpiderUtil import *

if __name__ == "__main__":

    # 获取页面html
    html = getHtml("http://tieba.baidu.com/p/2544042204")
    # print(html)
    # 使用正则获取页面邮箱列表
    reslist = re.findall(PATTERN_EMAIL, html, flags=re.A)

    '''
    写入CSV
    '''
    # 以读写模式打开文件（\u有特殊含义，此处声明不转义）
    with open(r"C:\Users\idea\Desktop\岛民邮箱.csv", "w+", newline="") as file:

        # 写入CSV
        writer = csv.writer(file)
        writer.writerow(("邮箱", "爬取时间", "状态"))
        for email in reslist:
            # print(email)
            writer.writerow([email, time.ctime(), "未发送"])

        # 读取CSV
        file.seek(0)
        print("----------")
        reader = csv.reader(file)
        for item in reader:
            print(item)

    print("main over")

```



