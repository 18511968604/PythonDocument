```py
'''
进程间共享队列（先进先出）
'''
import multiprocessing

# 编辑共享的列表和字典（以传参形式让进程持有数据对象（的地址））
from queue import Empty, Full

import time


def writeData(queue, event):
    for i in range(10):
        try:

            # 向队列中放入元素，不写block和timeout时，默认阻塞到能放进去为止
            # 当前逻辑：等待1秒后强行放入，此处有可能抛出Full异常（列表已满放你妹）
            queue.put("hello-%d" % (i), block=False, timeout=1)
            event.set()
            print("hello-%d 已放入" % (i))
            time.sleep(1)

        # 捕获和处理放你妹异常
        except Full:
            print("该死的列表已满")
            pass
    pass


# 读取共享数据（以传参形式让进程持有数据对象（的地址））
def readData(queue, event):
    for i in range(10):
        event.wait()
        try:
            # print("event got")
            # print(queue.get(),"end")

            # 从队列中向外拿取元素,timeout不给时，默认为一直阻塞到能拿到元素为止
            # 当前逻辑：等待1秒后，强行拿取，此处容易抛出Empty异常（都没东西拿你妹）
            print(queue.get(timeout=1), "end")
            time.sleep(2)

        # 捕获和处理拿你妹异常
        except Empty:
            print("该死的列表已空!")
            pass

    pass


if __name__ == "__main__":
    event = multiprocessing.Event()

    # 创建一个最大容量为5的队列（不给maxsize默认无穷大）
    pqueue = multiprocessing.Queue(maxsize=5)
    # pqueue.put(1)
    # pqueue.get()

    multiprocessing.Process(target=writeData, args=(pqueue, event)).start()
    multiprocessing.Process(target=readData, args=(pqueue, event)).start()
    print("main over")

```



