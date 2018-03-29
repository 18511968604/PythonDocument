```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-

import re

mystr = """<span class=\"search_yx_tj\">
        共<em>7287</em>个职位满足条件
    </span>"""

myre = "<em>(\d+)</em>"
regex = re.compile(myre, re.I)

mylist = regex.findall(mystr)

print mylist[0]

```



