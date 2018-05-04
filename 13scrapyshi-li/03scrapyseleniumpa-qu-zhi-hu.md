```py
# -*- coding: utf-8 -*-
import scrapy
import time
from selenium import webdriver


class MyzhihuSpider(scrapy.Spider):
    name = 'myzhihu'
    allowed_domains = ['zhihu.com']
    start_urls = ['http://www.zhihu.com/']

    def get_cookie(self):
        loginurl = "https://www.zhihu.com/signup?next=%2F"
        driver = webdriver.Chrome(r"C:\Python36\chromedriver.exe")
        driver.get(loginurl)
        # //*[@class=\"SignContainer-switch\"]/span
        driver.find_element_by_xpath("//*[@class=\"SignContainer-switch\"]/span").click()
        # driver.find_element_by_link_text(u"登录").click()
        time.sleep(2)
        # driver.switch_to_frame("")
        phone = driver.find_element_by_name("username")
        pwd = driver.find_element_by_name("password")
        phone.clear()
        phone.send_keys("18588403840")
        pwd.send_keys("Changeme_123")
        # div.Login-content > form > button
        time.sleep(10)
        driver.find_element_by_css_selector("div.Login-content > form > button").click()
        time.sleep(1)
        cookie = driver.get_cookies()
        return cookie

    def parse(self, response):
        return scrapy.Request(url=self.start_urls[0], cookies=self.get_cookie(), callback=self.after_login)

    def after_login(self, response):
        print(response.body)
```

知乎有反爬虫

```
USER_AGENT = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36'

# Obey robots.txt rules
ROBOTSTXT_OBEY = False
```



