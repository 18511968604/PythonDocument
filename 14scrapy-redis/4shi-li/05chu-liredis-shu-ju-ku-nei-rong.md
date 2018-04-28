### 存入mysql

#### baike\_mysql.py

```py
import json
import redis
import MySQLdb


def main():
    # 指定redis数据库信息
    rediscli = redis.StrictRedis(host='127.0.0.1', port=6379, password='123456', db=0)
    # 指定mysql数据库
    mysqlcli = MySQLdb.connect(host='127.0.0.1', user='root', passwd='123456', db='fate', port=3306,
                               charset='utf8')
    print(rediscli)
    while True:
        # FIFO模式为 blpop，LIFO模式为 brpop，获取键值
        source, data = rediscli.blpop(["mybaike_redis:items"])
        item = json.loads(data)
        print(item)

        try:
            # 使用cursor()方法获取操作游标
            cur = mysqlcli.cursor()
            sql = 'INSERT INTO BAIKE(url,masterTitle,secondTitle,content)  \
                         VALUES("%s","%s","%s","%s")' % (
            item["url"], item["masterTitle"], item["secondTitle"], item["content"])
            print(sql)
            cur.execute(sql)
            mysqlcli.commit()
            # 关闭本次操作
            cur.close()
            print("inserted %s" % item['source_url'])
        except MySQLdb.Error as e:
            print("Mysql Error %d: %s" % (e.args[0], e.args[1]))


if __name__ == '__main__':
    main()
```

### 存入MongoDB

#### **baike\_mongodb.py**

```py
# -*- coding: utf-8 -*-

import json
import redis
import pymongo

def main():

    # 指定Redis数据库信息
    rediscli = redis.StrictRedis(host='192.168.199.108', port=6379, db=0)
    # 指定MongoDB数据库信息
    mongocli = pymongo.MongoClient(host='localhost', port=27017)

    # 创建数据库名
    db = mongocli['baidu']
    # 创建表名
    sheet = db['baike']

    while True:
        # FIFO模式为 blpop，LIFO模式为 brpop，获取键值
        source, data = rediscli.blpop(["mybaike_redis:items"])

        item = json.loads(data)
        sheet.insert(item)

        try:
            print("inserted %s" % item['source_url'])

        except KeyError:
            print("Error procesing: %r" % item)

if __name__ == '__main__':
    main()
```



