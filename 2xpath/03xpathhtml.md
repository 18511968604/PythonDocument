```py
# coding:utf-8
import lxml
import lxml.etree

html = u'''
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>12345</title>
</head>
<body>
<div id="content">
    <ul id="useful">
        <li text="11">a1</li>
        <li text="12">a2</li>
        <li text="13">a3</li>
    </ul>
    <ul id="useless">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>

    <div id="url">
        <a href="http://51job.com">zhiwei</a>
        <a href="http://51job.com/course/" title="python">clickit</a>
    </div>
</div>

</body>
</html>
'''
mytree = lxml.etree.HTML(html)
print(mytree.xpath("//title/text()"))
print(mytree.xpath("//*[@id=\"useful\"]/li/text()"))
print(mytree.xpath("//*[@id=\"useless\"]/li/text()"))
print(mytree.xpath("//*[@id=\"url\"]/a/@href"))
print(mytree.xpath("//*[@id=\"url\"]/a/@title"))
print(mytree.xpath("//*[@id=\"url\"]/a/text()"))
```



