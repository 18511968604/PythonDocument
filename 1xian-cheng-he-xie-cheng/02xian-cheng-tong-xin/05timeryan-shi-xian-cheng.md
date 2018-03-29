```py
'''
Timer延时线程
'''
import threading
import time


def doSth(arg):
    tname = threading.current_thread().getName()
    print("%s正在执行【%s】" % (tname, arg))
    time.sleep(1)
    print("-----%s执行完毕!-----\n" % (tname))


if __name__ == '__main__':
    # 延时5秒执行doSth（"巡山"）
    threading.Timer(5, doSth, args=["巡山"]).start()
    pass

```



