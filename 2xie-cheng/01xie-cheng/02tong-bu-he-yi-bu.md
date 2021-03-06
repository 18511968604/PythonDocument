```py
'''
about what
'''
import multiprocessing

import time


def func():
    print("%s开始执行..." % (multiprocessing.current_process().name))
    time.sleep(3)
    print("%s执行结束" % (multiprocessing.current_process().name))
    pass

def workWithLock(lock):

    print("%s开始执行..." % (multiprocessing.current_process().name))

    with lock:
        print("%s正在工作..."% (multiprocessing.current_process().name))
        time.sleep(3)

    print("%s执行结束" % (multiprocessing.current_process().name))

def workWithLock2(lock):

    print("%s开始执行..." % (multiprocessing.current_process().name))

    if lock.acquire():
        print("%s正在工作..."% (multiprocessing.current_process().name))
        time.sleep(3)
        lock.release()

    print("%s执行结束" % (multiprocessing.current_process().name))


def processSync():
    p1 = multiprocessing.Process(target=func, name="小分队1")
    p2 = multiprocessing.Process(target=func, name="小分队2")
    p1.start()
    p1.join()  # 后边的代码给劳资等着
    p2.start()
    p2.join()


def processAsync():
    multiprocessing.Process(target=func, name="小分队1").start()
    multiprocessing.Process(target=func, name="小分队2").start()


def processLock():
    lock = multiprocessing.Lock()
    multiprocessing.Process(target=workWithLock2, name="小分队1", args=(lock,)).start()
    multiprocessing.Process(target=workWithLock2, name="小分队2", args=(lock,)).start()


if __name__ == "__main__":
    # 并发执行
    # processAsync()

    # 进程同步：先后执行
    # processSync()

    # 进程锁
    processLock()

    print("main over")

```



