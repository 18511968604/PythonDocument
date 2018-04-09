### 1、Tesseract

一款由HP实验室开发由Google维护的开源OCR\(光学字符识别\)引擎

* #### 语法：

```
tesseract imagename outputbase [-l lang] [-psm pagesegmode] [configfile…]
```

imagename为目标图片文件名，需加格式后缀；

outputbase是转换结果文件名；

lang是语言名称（在Tesseract-OCR中tessdata文件夹可看到以eng开头的语言文件eng.traineddata），如不标-l eng则默认为eng。

```
例：tesseract test.png output_1 –l eng
```

#### 字库

https://github.com/tesseract-ocr/tessdata/find/master

    下载chi\_sim.traindata字库。要有这个才能识别中文。下好后，放到Tesseract-OCR项目的tessdata文件夹里面。

    tesseract test.jpg result -l chi\_sim

