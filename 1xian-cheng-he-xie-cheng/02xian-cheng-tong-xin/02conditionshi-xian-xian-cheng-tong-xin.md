```py
'''
通过threading.Condition实现线程通信：
生产消费模型
'''
import random
import threading

import time

# 线程间交替通信的信物
condition = threading.Condition()

# 产品容器
plist = []


# 产品
class Product:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return "Product:%s" % (self.name)


# 生产者线程
class Producer(threading.Thread):
    def run(self):
        while True:
            # 阻塞等待获得condition对象
            with condition:
                # 此处已经获取到condition对象（condition.acquire()）
                print("生产者拿到condition")

                p = Product(random.randint(100, 999))
                plist.append(p)
                print("生产者生产了", p)
                time.sleep(1)

                condition.notify()  # 通知消费者，谁wait()了就通知谁
                condition.wait()  # 监听消费者通知，谁wait代表谁希望被notify（wait中会释放condition）

                # with走完，交出condition
                # 此处condition已释放（condition.release()）


# 消费者线程
class Consumer(threading.Thread):
    def run(self):
        while True:

            # 阻塞等待获得condition对象(本例中一直等到生产者with condition内的代码走完)
            with condition:
                # 此处已经获取到condition对象（condition.acquire()）
                print("消费者拿到conditon")
                try:
                    p = plist.pop()
                    print("%s消费者消费了" % (self.name), p)

                    condition.notify()  # 通知生产者生产,谁with了相同condition且wait就通知谁
                    condition.wait()  # 等候生产者消息（wait中会释放condition）
                    print("fuck")
                except IndexError:
                    print("消费者：冰箱里空空如也")
                    # 此处condition已释放（condition.release()）


if __name__ == '__main__':
    Producer().start()
    Consumer().start()
    pass

```



