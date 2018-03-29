```py
import _thread
import threading
from threading import Thread

import time


def doSth(arg):
    # 拿到当前线程的名称和线程号id
    threadName = threading.current_thread().getName()
    tid = threading.current_thread().ident
    for i in range(5):
        print("%s *%d @%s,tid=%d" % (arg, i, threadName, tid))
        time.sleep(2)


# 使用_thread.start_new_thread开辟子线程
def simpleThread():
    # 创建子线程，执行doSth
    # 用这种方式创建的线程为【守护线程】（主线程死去“护卫”也随“主公”而去）
    _thread.start_new_thread(doSth, ("拍森",))

    mainThreadName = threading.current_thread().getName()
    print(threading.current_thread())
    # 5秒的时间以内，能看到主线程和子线程在并发打印
    for i in range(5):
        print("劳资是主线程@%s" % (mainThreadName))
        time.sleep(1)

    # 阻塞主线程，以使【守护线程】能够执行完毕
    while True:
        pass


# 通过创建threading.Thread对象实现子线程
def threadingThread():
    # 默认不是【守护线程】
    t = threading.Thread(target=doSth, args=("大王派我来巡山",))
    # t.setDaemon(True)  # 设置为守护线程
    t.start()  # 启动线程，调用run()方法

    # 5秒的时间以内，能看到主线程和子线程在并发打印
    # for i in range(5):
    #     print("劳资是大王@%s" % (threading.current_thread().getName()))
    #     time.sleep(1)


# 继承于threading.Thread
class MyThread(threading.Thread):
    # 构造方法一定要执行super方法，否则不能构成事实上的线程类
    # 继承法的优势在于可以灵活自定义（构造方法传参，在类内部封装很多方法）
    # 注意：构造方法是跑在【主调线程】（创建当前实例的线程）
    def __init__(self, name, task, subtask):
        super().__init__()

        self.name = name  # 覆盖了父类的name
        self.task = task  # MyThread自己的属性
        self.subtask = subtask

    # 覆写父类的run方法，
    # run方法以内为【要跑在子线程内的业务逻辑】(thread.start()会触发的业务逻辑)
    def run(self):
        for i in range(5):
            print("【%s】并【%s】 *%d @%s" % (self.task, self.subtask, i, threading.current_thread().getName()))
            time.sleep(2)


# 通过继承threading.Thread类，进而创建对象实现子线程
def classThread():
    mt = MyThread("小分队I", "巡山", "扫黄")
    mt.start()
    for i in range(5):
        tid = threading.current_thread().ident
        print("劳资是大王@%s,tid=%d" % (threading.current_thread().getName(), tid))
        time.sleep(1)
    print("主线程over")


# 几个重要的API
def importantAPI():
    print(threading.currentThread())  # 返回当前的线程变量
    # 创建五条子线程
    t1 = threading.Thread(target=doSth, args=("巡山",))
    t2 = threading.Thread(target=doSth, args=("巡水",))
    t3 = threading.Thread(target=doSth, args=("巡鸟",))

    t1.start()  # 开启线程
    t2.start()
    t3.start()

    print(t1.isAlive())  # 返回线程是否活动的
    print(t2.isDaemon())  # 是否是守护线程
    print(t3.getName())  # 返回线程名
    t3.setName("巡鸟")  # 设置线程名
    print(t3.getName())
    print(t3.ident)  # 返回线程号

    # 返回一个包含正在运行的线程的list
    tlist = threading.enumerate()
    print("当前活动线程：", tlist)

    # 返回正在运行的线程数量（在数值上等于len(tlist)）
    count = threading.active_count()
    print("当前活动线程有%d条" % (count))


if __name__ == '__main__':
    # simpleThread()
    # threadingThread()
    # classThread()
    importantAPI()
    pass

```



