```py
#! C:\Python36\python.exe
'''
从Json中爬取图片
'''
import json
import os
import threading

import re
import requests

from demos.W5.day2.SpiderUtil import *

basepath = "D:\\PyDownload\\"
if __name__ == "__main__":
    # 得到json字符串
    resp = requests.get("http://www.toutiao.com/search_content/?offset=0&format=json&keyword=美女&autoload=true&count=20&cur_tab=3")
    jsonStr = resp.text

    # 得到json字符串对应的字典
    jdict = json.loads(jsonStr)
    # print(type(jdict),jdict)
    jlist = jdict["data"]
    print(type(jlist))

    # 预编译图片名称表达式，以便反复复用
    filenamePattern = re.compile(".*/(.*)")
    tlist = []#下载线程容器

    # 遍历组图数据
    for item in jlist:

        # 从json结构中获取标题
        title = item["title"]
        print("\n-----",title,"-----")

        # 创建组图标题对应的文件夹
        dirPath = basepath + title
        if not os.path.isdir(dirPath):#判断文件夹是否存在，不存在则创建
            os.makedirs(dirPath)

        # 获得组图下的图片地址列表
        ilist = item["image_detail"]
        for x in ilist:
            iurl = x["url"]
            print(iurl)

            # 构建文件路径
            # http://p3.pstatp.com/large/320700044e624f91d6ca
            # http://p1.pstatp.com/large/320900044bf85fbf27ca
            filename = filenamePattern.search(iurl).group(1)+".jpg"
            filename = basepath + title + "\\" + filename
            print(filename)

            # 开辟线程进行下载
            t = threading.Thread(target=downloadFile,args=(iurl,filename))
            tlist.append(t)
            t.start()

    # 主线程等待所有下载线程
    for t in tlist:
        t.join(timeout=30)

    print("main over")

```



