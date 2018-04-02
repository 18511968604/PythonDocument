```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import matplotlib
from matplotlib import pyplot as plt

# 配置字体
matplotlib.rcParams["font.sans-serif"] = ["simhei"]  # 黑体
matplotlib.rcParams["font.family"] = "sans-serif"

plt.bar([1], [123], label=u"广州", color="r")
plt.bar([2], [141], label=u"北京")
plt.bar([3], [11], label=u"上海")
plt.bar([4], [41], label=u"深圳")
plt.bar([5], [181], label=u"香港")
plt.legend()  # 绘图
plt.show()
plt.savefig("1.jpg")  # 保存图片
```



