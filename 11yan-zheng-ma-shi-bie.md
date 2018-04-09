### 1、Tesseract

一款由HP实验室开发由Google维护的开源OCR\(光学字符识别\)引擎

* #### 语法：

```
tesseract imagename outputbase [-l lang] [-psm pagesegmode] [configfile…]
```

 imagename为目标图片文件名，需加格式后缀；

 outputbase是转换结果文件名；

 lang是语言名称（在Tesseract-OCR中tessdata文件夹可看到以eng开头的语言文件eng.traineddata），如不标-l eng则默认为eng。

