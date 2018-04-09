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

* #### 字库

[https://github.com/tesseract-ocr/tessdata/find/master](https://github.com/tesseract-ocr/tessdata/find/master)

下载chi\\_sim.traindata字库。要有这个才能识别中文。下好后，放到Tesseract-OCR项目的tessdata文件夹里面。

```
tesseract test.jpg result -l chi_sim
```

#### 但是还是有点不够准确，那么我们有没有什么办法能提高tesseract识别字符准确率呢？

#### 接下来，我们将使用配套训练工具jTessBoxEditor来训练样本，来提高我们的准确率！

大体流程为：安装jTessBoxEditor -&gt; 获取样本文件 -&gt; Merge样本文件 –&gt; 生成BOX文件 -&gt; 定义字符配置文件 -&gt; 字符矫正 -&gt; 执行批处理文件 -&gt; 将生成的traineddata放入tessdata中

[https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/](https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/)

参考

[http://blog.sina.com.cn/s/blog\_660e521d0102uyaq.html](http://blog.sina.com.cn/s/blog_660e521d0102uyaq.html)

* 获取样本文件

  1、将图片转换成tif格式，用于后面生成box文件。可以通过画图，然后另存为tif即可。

  更改图片名字，这个是有要求的=。=

  tif文面命名格式\[lang\].\[fontname\].exp\[num\].tif  
  lang是语言 fontname是字体  
  比如我们要训练自定义字库 mjorcen字体名normal  
  那么我们把图片文件重命名 mjorcen.normal.exp0.jpg在转tif。

  2、使用jTessBoxEditor

  点击train.bat----&gt;tool-----&gt;Merga TIFF-----&gt;选择图片-----&gt;保存为---名字.语言.exp0，在修改另存为.jpg

  ![](/assets/traintif.png)

* Merge样本文件

* 生成BOX文件

  tesseract mjorcen.normal.exp0.jpg mjorcen.normal.exp0 -l eng batch.nochop makebox

  ![](/assets/trainBox.png)

* 定义字符配置文件

  **新建一个font\_properties文件**

  里面内容写入

  normal 0 0 0 0 0 表示默认普通字体

* 字符矫正

   打开.tif文件，逐个矫正

![](/assets/trainchange.png)

* 执行批处理文件
* 将生成的traineddata放入tessdata中



