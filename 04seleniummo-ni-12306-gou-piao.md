```py
#! C:\Python36\python.exe
# -*- coding:utf-8 -*-

import time
import smtplib
from email.mime.text import MIMEText
from selenium import webdriver
from selenium.webdriver.support.select import Select

"""      
--1>验证码有一定规律和数量，可以利用脚本获取所有图片，并加上相应标签   
--2>将页面的文字和标签相匹配，再将图片进行相似度计算，对对应图片进行点击操作   
--3>或是训练深度学习的图片识别模型，通过算法识别   
"""


# 抢票
def byTrain():
    browser = webdriver.Chrome()
    browser.get("https://kyfw.12306.cn/otn/login/init")
    browser.find_element_by_id('username').clear()
    # 用户名和密码
    browser.find_element_by_id('username').send_keys('****')
    browser.find_element_by_id('password').send_keys('******')
    time.sleep(10)
    browser.find_element_by_id('loginSub').click()  # 跳转到车票预定页面  
    time.sleep(6)
    browser.find_element_by_link_text('车票预订').click()
    while True:
        # 出发地点和到达地点设置
        '''
        车站车次查询：https://kyfw.12306.cn/otn/czxx/init中js加载
        车站代码
        https://kyfw.12306.cn/otn/resources/js/framework/station_name.js?station_version=1.9042
        '''
        # 刷新
        browser.refresh()
        # 出发地
        jsf = 'var a = document.getElementById("fromStation");a.value = "SZQ"'
        browser.execute_script(jsf)
        # 目的地
        jst = 'var a = document.getElementById("toStation");a.value = "CZQ"'
        browser.execute_script(jst)
        js = "document.getElementById('train_date').removeAttribute('readonly')"
        browser.execute_script(js)
        browser.find_element_by_id('train_date').clear()
        # 出发时间
        browser.find_element_by_id('train_date').send_keys('2018-02-02')
        browser.find_element_by_id('query_ticket').click()
        time.sleep(2)
        # 选择车次
        try:
            browser.find_element_by_xpath('//tr[@id="ticket_69000K90640E"]/td[13]/a').click()
            # browser.find_element_by_xpath('//*[@id="ticket_6i000G614207"]/td[13]/a').click()
            break
        except:
            pass

    time.sleep(4)

    # 选择乘客
    browser.find_element_by_id('normalPassenger_0').click()
    time.sleep(2)

    # 选择车座类型
    # s = Select(browser.find_element_by_id('seatType_1'))
    # s.select_by_value('O')

    # 取消购买学生票
    browser.find_element_by_id("dialog_xsertcj_cancel").click()
    time.sleep(2)
    browser.find_element_by_id('submitOrder_id').click()  # 提交订单
    time.sleep(2)
    browser.find_element_by_id('qr_submit_id').click()


# 发送邮件提醒
def sendEmail():
    # 用户名和密码
    sender = '**@163.com'
    password = '**'
    # 接收邮件，可设置为你的QQ邮箱或者其他邮箱
    receivers = ["1687458949@qq.com"]

    # 三个参数：第一个为文本内容，第二个 plain 设置文本格式，第三个 utf-8 设置编码
    text = '麻痹的，终于抢到了！！！'
    message = MIMEText(text)
    message['From'] = sender
    message['To'] = receivers[0]
    message['Subject'] = '最近还好吗？'

    try:
        smtpObj = smtplib.SMTP('smtp.163.com', 25)
        smtpObj.login(sender, password)
        smtpObj.sendmail(sender, receivers, message.as_string())
        smtpObj.quit()
        print("邮件发送成功")
    except smtplib.SMTPException as e:
        print("Error: 无法发送邮件", e)


def main():
    byTrain()
    # sendEmail()


if __name__ == '__main__':
    main()
```



