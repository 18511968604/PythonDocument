```py
#! C:\Python36\python.exe
'''
爬取页面图片
'''
import re
import requests

basepath = "D:/PyDownload/"

if __name__ == "__main__":
    resp = requests.get("http://www.163.com")
    html = resp.text

    # 从html中正则检索<img ... src="https://..." ...>
    pattern = re.compile("<img.* src=\"(https?://.*?)\".*>")
    reslist = pattern.findall(html)

    for i in range(len(reslist)):
        imgUrl = reslist[i]
        print(imgUrl)

        # 下载图片
        try:
            # 拿到图片的字节数组
            resp = requests.get(imgUrl)
            bytes = resp.content

            # 定义文件名
            filename = basepath + str(i) + ".jpg"

            # 写入文件
            file = open(filename, "xb")
            file.write(bytes)
            file.close()
            print(i, "下载完成！")

        # 处理异常
        except Exception as e:
            print(e)
            pass

    print("main over")

```



