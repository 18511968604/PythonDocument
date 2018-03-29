```py
#! C:\Python36\python.exe
'''
爬取页面链接
'''
import re
import requests

if __name__ == "__main__":
    # 获取页面html
    resp = requests.get("http://www.163.com")
    html = resp.text
    # print(html)

    # 正则检索http://...
    # 表达式中的第二个？代表非贪婪模式，即：【.】的范围不包括【"】
    reslist = re.findall("(https?://.*?)\"", html)
    for url in reslist:
        print(url)

    print("main over")

```



