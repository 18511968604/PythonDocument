```py
#! C:\Python36\python.exe
# -*- coding:utf-8 -*-

from selenium import webdriver
import time

# http://demo.smeoa.com/
def openURL():
    driver = webdriver.Chrome()
    driver.get("https://user.qzone.qq.com")
    time.sleep(6)
    login = driver.find_element_by_id('login_frame')
    driver.switch_to_frame(login)
    time.sleep(3)
    driver.find_element_by_id('switcher_plogin').click()

    username = driver.find_element_by_id('u')
    password = driver.find_element_by_id('p')
    username.send_keys('*****')
    password.send_keys('*****')
    time.sleep(3)
    driver.find_element_by_id('login_button').click()
    print("OK")

if __name__ == '__main__':
    openURL()
```



