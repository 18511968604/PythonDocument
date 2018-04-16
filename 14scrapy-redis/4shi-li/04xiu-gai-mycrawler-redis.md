### 添加/example/redis\_client.py

```py
#!C:\Python36\python.exe
# -*- coding:utf-8 -*-
import redis

myredis = redis.Redis(host="127.0.0.1", password="123456", port=6379)
print(myredis.info())
url = "https://baike.baidu.com/item/Python/407313"
myredis.lpush("baike_redis:start_urls", url)
```



