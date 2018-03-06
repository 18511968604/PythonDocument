```py
# coding:utf-8
import lxml
import lxml.etree

text = """
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a> # 注意，此处缺少一个 </li> 闭合标签
     </ul>
 </div>
"""
'''
html=lxml.etree.HTML(text) #html处理文本
print (type(html))
print html
print lxml.etree.tostring(html)
'''

html = lxml.etree.parse("index.html")  # parse处理文件
print(type(html))
print(html)
print(lxml.etree.tostring(html))

```



