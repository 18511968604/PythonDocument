```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import requests
import json


def getMusic(url):
    header = {"Referer": "http://music.163.com/",
              "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"}
    data = {"params": "j/p3BKJKwaM1PUxXTrOgmI7B1CoQPFycLQSyH6JAVpYdMIik+PJwPw+TJ7VRvAy5",
            "encSecKey": "bab0393ac8c89f9c9295f653b32f0c378a8af4f2a6bfede3a7123a0f344c4a271135b6853ef4cd3ebe4176711eb1059004c9d98703c9df586f767612c3eb926b315832a0145e94f513b15975285ac10cecb30f2893796710da0d9d4fa0a491fd9983518949367355bf8f1447d8522b2aa25e88e05a7c90ffc1ad8a8ee6f59f9b"}

    response = requests.post(url, headers=header, data=data)
    return response.content


if __name__ == '__main__':
    url = "http://music.163.com/weapi/v1/resource/comments/R_SO_4_546723152?csrf_token="
    list = getMusic(url)
    json_dict = json.loads(list)
    print list
    hot_comments = json_dict['hotComments']  # 热门评论

    hot_comments_list = []
    print("共有%d条热门评论!" % len(hot_comments))

    for item in hot_comments:

        comment = item['content']  # 评论内容

        likedCount = item['likedCount']  # 点赞总数

        comment_time = item['time']  # 评论时间(时间戳)

        userID = item['user']['userId']  # 评论者id

        nickname = item['user']['nickname']  # 昵称

        avatarUrl = item['user']['avatarUrl']  # 头像地址
        comment_info = (comment, likedCount, comment_time, userID, nickname, avatarUrl)
        hot_comments_list.append(comment_info)

    print hot_comments_list
    for i in hot_comments_list:
        print i

```



