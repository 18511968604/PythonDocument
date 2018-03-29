```py
import threading
from threading import Thread
import time

'''
这个东西可以用在那些地方呢，比如下载，现在都是多线程下载了，
就像酷狗那样，可以同时下载很多首歌曲，那么就可以利用这个方法
来保存每个下载线程的数据，比如下载进度，下载速度之类的
'''
# threading.local()这个方法的特点用来保存一个全局变量，
# 但是这个全局变量只有在当前线程才能访问，
data = threading.local()
print(type(data))


def doSomething(num):
    tName = threading.current_thread().getName()
    '''
    localVal.val = name这条语句可以储存一个变量到当前线程，
    如果在另外一个线程里面再次对localVal.val进行赋值，
    那么会在另外一个线程单独创建内存空间来存储，
    也就是说在不同的线程里面赋值不会覆盖之前的值，
    因为每个线程里面都有一个单独的空间来保存这个数据,
    而且这个数据是隔离的，其他线程无法访问
    '''
    data.num = num
    for i in range(5):
        data.num += 1
        time.sleep(1)
        print(tName, "data.num=", data.num)
    pass


def doSomethingII(num):
    tName = threading.current_thread().getName()
    data.num = num
    for i in range(5):
        data.num += "1"
        time.sleep(1)
        print(tName, "data.num=", data.num)
    pass


if __name__ == '__main__':
    # threading.local的例子
    t1 = Thread(target=doSomething, args=(1,), name="foo")
    t2 = Thread(target=doSomethingII, args=("1",), name="bas")
    t1.start()
    t2.start()

    print("over")

```



