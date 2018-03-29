```py
#! C:\Python36\python.exe
'''
爬取页面图片3.0
'''
import re
import threading
from urllib import request

import requests
import time

basepath = "D:/PyDownload/"


# 下载图片
def downImg(imgUrl, filename):
    request.urlretrieve(imgUrl, filename)
    print(filename, "下载完成")


if __name__ == "__main__":
    resp = requests.get("http://www.163.com")
    html = resp.text

    # 从html中正则检索<img ... src="https://..." ...>
    pattern = re.compile("<img.* src=\"(https?://.*?)\".*>")
    reslist = pattern.findall(html)

    tlist = []
    for i in range(len(reslist)):
        imgUrl = reslist[i]
        print(imgUrl)

        # 下载图片
        try:
            # 定义文件名
            # http://img1.cache.netease.com/f2e/include/common_nav/images/topapp.jpg
            # http://cms-bucket.nosdn.127.net/2afa1dc81e634b30aabc457e438f95d720170829110850.jpeg?imageView&thumbnail=453y225&quality=85

            # 从图片地址中捕获图片名称
            try:
                filename = re.search(".*/(.*\.((jpg)|(jpeg)|(png)|(gif)|(bmp)))", imgUrl).group(1)

            # 对于不标准的图片地址使用自定义的时间戳名称
            except Exception as e:
                print(e)
                filename = "未命名-" + str(int(time.time())) + ".jpg"
            print(filename)
            filename = basepath + filename

            # 多线程下载图片
            t = threading.Thread(target=downImg, args=(imgUrl, filename))
            tlist.append(t)  # 加入到线程列表
            t.start()

        # 处理异常
        except Exception as e:
            print(e)
            pass

    # 让主线程等待所有下载线程
    for t in tlist:
        t.join()
        pass

    print("main over")

```



