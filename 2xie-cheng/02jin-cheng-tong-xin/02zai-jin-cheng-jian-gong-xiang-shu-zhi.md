```py
'''
在进程间共享数值
'''
import multiprocessing
import threading

import time

# data = multiprocessing.Value("i",0)

# 在进程间共享一个数值
pValue = multiprocessing.Value("d", 0)

# 在进程间共享一组数值
pArray = multiprocessing.Array("i",[0,1,2,3,4])

def sendEvent(event, pValue,pArray):
    for i in range(5):
        pValue.value += 1.5
        pArray[0] = 99999

        event.set()
        print("事件已发送*", pValue.value)
        time.sleep(1)
    pass

def handleEvent(event, pValue,pArray):
    for i in range(5):
        event.wait()
        print("事件已处理*", pValue.value, pArray[0])
        event.clear()
    pass


if __name__ == "__main__":
    event = multiprocessing.Event()
    p1 = multiprocessing.Process(target=sendEvent, args=(event, pValue,pArray))
    p2 = multiprocessing.Process(target=handleEvent, args=(event, pValue,pArray))
    p1.start()
    p2.start()
    p2.join()

    print("data=", pValue)
    print("main over")
```



