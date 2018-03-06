```py
#! C:\Python36\python.exe
# -*- coding:utf-8 -*-

import re
import threading
from urllib import request
import time

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.select import Select


# 打开URL
def openURL():
    driver = webdriver.PhantomJS()
    driver.get("http://www.baidu.com")
    print(driver.page_source)


def sendSearch():
    # 打开浏览器
    driver = webdriver.Chrome()
    driver.get("http://www.python.org")

    # 查找网页元素，譬如 find_element_by_* 的方法
    assert "Python" in driver.title
    elem = driver.find_element_by_name("q")
    # 发送
    elem.send_keys("pycon")
    # 利用Keys 这个类来模拟键盘输入。
    elem.send_keys(Keys.RETURN)
    # 获取网页渲染后的源代码
    print(driver.page_source)


# 页面操作
def getElement():
    # 查找网页元素，譬如 find_element_by_* 的方法
    # <input type="text" name="passwd" id="passwd-id" />
    driver = webdriver.Chrome()
    driver.get("http://www.baidu.com")
    element = driver.find_element_by_id("kw")
    # element = driver.find_element_by_name("passwd")
    # element = driver.find_elements_by_tag_name("input")
    # element = driver.find_element_by_xpath("//input[@id='passwd-id']")

    # 向文本输入内容
    # element.send_keys("海贼王")
    # 利用 Keys 这个类来模拟点击某个按键
    element.send_keys("海贼王", Keys.ARROW_DOWN)
    # 清除文本
    # element.clear()


def getForm():
    driver = webdriver.Chrome()
    driver.get("http://www.baidu.com")

    # 可以根据索引来选择，可以根据值来选择，可以根据文字来选择
    select = Select(driver.find_element_by_name('name'))
    select.select_by_index(1)
    # select.select_by_visible_text("text")
    # select.select_by_value(value)
    # 全部取消选择
    select.deselect_all()
    # 提交表单
    driver.find_element_by_id("submit").click()
    # 单独提交某个元素
    # element.submit()


# Cookies处理
def Cookies():
    driver = webdriver.Chrome()
    driver.get("http://www.example.com")
    # 为页面添加 Cookies
    cookie = {"name": "foo", "value": "bar"}
    driver.add_cookie(cookie)

    # 获取页面Cookies
    driver.get_cookies()


# 下载图片
def downImg(imgUrl, filename):
    request.urlretrieve(imgUrl, filename)
    print(filename, "下载完成")


def getImage():
    driver = webdriver.Chrome()
    driver.maximize_window()
    # 使用PhantomJS
    # driver = webdriver.PhantomJS()
    driver.get("https://tieba.baidu.com/f?kw=%E6%B5%B7%E8%B4%BC%E7%8E%8B&ie=utf-8&pn=550")

    # 下拉滚动条，使浏览器加载出动态加载的内容
    while True:
        # 可能像这样要拉很多次，中间要适当的延时。
        # 如果说说内容都很长，就增大下拉的长度。
        for i in range(10):
            driver.execute_script("window.scrollBy(0,1000)")
            time.sleep(3)
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight)")
        break

    print(driver.page_source)
    html = driver.page_source
    pattern = re.compile("<img.* src=\"(https?://.*?)\".*>")
    reslist = pattern.findall(html)
    print(reslist)
    tlist = []
    basepath = "E:/PyDownload/"

    for i in range(len(reslist)):
        imgUrl = reslist[i]
        print(imgUrl)

        # 下载图片
        try:
            # 从图片地址中捕获图片名称
            try:
                filename = re.search(".*/(.*\.((jpg)|(jpeg)|(png)|(gif)|(bmp)))", imgUrl).group(1)
            # 对于不标准的图片地址使用自定义的时间戳名称
            except Exception as e:
                print(e)
                filename = "未命名-" + str(int(time.time())) + ".jpg"
            print(filename)
            filename = basepath + filename

            t = threading.Thread(target=downImg, args=(imgUrl, filename))
            tlist.append(t)
            t.start()

        except Exception as e:
            print(e)

    for t in tlist:
        t.join()
        pass


if __name__ == '__main__':
    openURL()
    sendSearch()
    getElement()
    getImage()
    pass

```



