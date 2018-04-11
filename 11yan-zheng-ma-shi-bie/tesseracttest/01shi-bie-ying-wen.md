```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import pytesseract
import PIL
import PIL.Image

image = PIL.Image.open(r"C:\Users\Administrator\Desktop\test\3.jpg")
# print(image)
pytesseract.pytesseract.tesseract_cmd = r"D:\Tesseract-OCR\tesseract.exe"
print(pytesseract.image_to_string(image, lang="normal"))

```



