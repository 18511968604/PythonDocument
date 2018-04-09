```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import urllib2
import urllib
import re
import lxml
import lxml.etree
import threading

header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}


# 获得岗位数量
def getJobNum(url):
    request = urllib2.Request(url, headers=header)
    response = urllib2.urlopen(request).read()
    # print response
    # 数量
    myre = "<em>(\d+)</em>"
    regex = re.compile(myre)
    res = regex.findall(response)
    print res
    return res[0]


# 获取岗位详细信息
def getWorkMessage(url):
    request = urllib2.Request(url, headers=header)
    response = urllib2.urlopen(request).read()
    myre = "<!-- SWSStringCutStart -->[\s\S](.*)<!-- SWSStringCutEnd -->"
    regex = re.compile(myre, re.S)
    res = regex.findall(response)
    return res[0].replace("\r\n", "").replace("<p>", "").replace("</p>", "").replace("<br/>", "")


# 获取岗位
def getJob(url):
    request = urllib2.Request(url, headers=header)
    response = urllib2.urlopen(request).read()
    # print response
    mytree = lxml.etree.HTML(response)
    joblist = mytree.xpath("//table[@class=\"newlist\"]")
    # //*[@id="newlist_list_content_table"]/table[2]/tbody/tr[1]/td[3]/a[1]
    # //*[@id="newlist_list_content_table"]/table[2]/tbody/tr[1]/td[3]
    print joblist
    print len(joblist)
    jobname = []  # 职位名称
    jobLink = []  # 链接
    companyName = []  # 公司名称
    salary = []  # 薪水
    workplace = []  # 工作地点
    for job in joblist:
        jobname = job.xpath("//tr/td[1]/div/a[1]/text()")
        jobLink = job.xpath("//tr/td[1]/div/a[1]/@href")
        companyName = job.xpath("//tr/td[3]/a[1]/text()")
        salary = job.xpath("//tr/td[4]/text()")
        workplace = job.xpath("//tr/td[5]/text()")
        try:
            for i in range(len(jobname)):
                print jobname[i], getWorkMessage(jobLink[i]), companyName[i], salary[i], workplace[i]
        except:
            pass


if __name__ == '__main__':
    url = "https://sou.zhaopin.com/jobs/searchresult.ashx?jl=%E6%B7%B1%E5%9C%B3&kw=python&sm=0&p=1"
    # num = getJobNum(url)
    num = 2910
    page = 0
    if num % 60 == 0:
        page = num // 60
    else:
        page = num // 60 + 1

    myList = ["https://sou.zhaopin.com/jobs/searchresult.ashx?jl=%E6%B7%B1%E5%9C%B3&kw=python&sm=0&p=" + str(i) for i in
              range(1, page + 1)]

    # for i in myList:
    #     print i

    # getWorkMessage("http://jobs.zhaopin.com/216494030250784.htm?ssidkey=y&ss=201&ff=03&sg=a039dd863e79428fbf07568274f6f927&so=9")

    # getJob("https://sou.zhaopin.com/jobs/searchresult.ashx?jl=%E6%B7%B1%E5%9C%B3&kw=python&sm=0&p=1")
    for i in myList:
        # print getJob(i)
        threading.Thread(target=getJob, args=(i,)).start()
```



