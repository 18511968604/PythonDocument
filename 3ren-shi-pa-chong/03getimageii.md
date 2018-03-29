```py
#! C:\Python36\python.exe
'''
爬取页面图片2.0
'''
import re
import threading
from urllib import request

import requests

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
            filename = basepath + str(i) + ".jpg"

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

    print("main over")

```



