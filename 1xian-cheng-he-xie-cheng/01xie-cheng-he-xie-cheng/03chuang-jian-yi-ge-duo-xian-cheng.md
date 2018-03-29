```py
'''
少林功夫+唱歌+跳舞
'''
import threading
import random

# 功夫、铁头功+金刚腿
import time


def kongfu(kind):
    print("我是%s" % kind)


# 唱歌
def sing(who, what):
    # time.sleep(3)
    print("%s:%s" % (who, what))


# 跳舞
def dance():
    time.sleep(1)
    print("蹦瞎卡拉卡,蹦瞎卡拉卡")


if __name__ == '__main__':
    singList = ["少林功夫好耶", "真滴好啊", "少林功夫够劲", "系好劲", "我系铁头功", "无敌铁头功",
                "我系金刚腿", "金刚腿", "他系铁头功", "哦哇,哦哇........"]
    t1 = threading.Thread(target=kongfu, args=("铁头功",))
    t2 = threading.Thread(target=kongfu, args=("金刚腿",))
    for i in range(len(singList)):
        t3 = threading.Thread(target=sing, args=("周星星", random.choice(singList)))
        t4 = threading.Thread(target=sing, args=("大师兄", random.choice(singList)))

        t3.start()
        # t3.join()
        t4.start()
        # t4.join()
    t5 = threading.Thread(target=dance)

    # 线程同步（依次执行）
    t1.start()
    # t1.join()
    t2.start()
    # t2.join()

    t5.start()
    # t5.join()

    # join()等待线程执行完毕，
    # 线程异步（并发执行）
    t1.join()
    t2.join()
    t3.join()
    t4.join()
    t5.join()
    pass

```



