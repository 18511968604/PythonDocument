```py
'''
互相锁住对方线程需要的资源，造成死锁局面
递归锁，用于解决死锁的问题,可重复锁
'''

import threading
from threading import Thread
import time

# 自尊锁
honor = threading.RLock()


class Boy(Thread):
    def run(self):
        print("妈蛋气死劳资了战斗开始...")

        # 锁住Girl所需的honor对象
        if honor.acquire():
            print("Boy:Gril必须先道歉!")
            time.sleep(1)

            # 需要honor才能继续执行
            if honor.acquire(timeout=-1):
                honor.release()
                honor.release()
                print("Boy:im sorry too！")
            else:
                print("Boy:....")
        print("Boy战斗结束")


class Girl(Thread):
    def run(self):
        print("妈蛋气死老娘了战斗开始...")

        # 锁住Boy所需的honor对象
        if honor.acquire():
            time.sleep(2)
            print("Gril：Boy必须先道歉!")

            # 需要honor才能继续执行
            if honor.acquire(timeout=-1):
                honor.release()
                honor.release()
                print("Girl:im sorry too！")
            else:
                print("Girl:妈蛋分手！")
        print("Girl战斗结束")


if __name__ == '__main__':
    Boy().start()
    Girl().start()
    pass

```



