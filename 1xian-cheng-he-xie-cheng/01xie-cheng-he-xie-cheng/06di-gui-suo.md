```py
'''
互相锁住对方线程需要的资源，造成死锁局面
'''

import threading
from threading import Thread
import time

# 自尊锁
boyHonor = threading.Lock()
girlHonor = threading.Lock()
honor = threading.RLock()


class Boy(Thread):
    def run(self):
        print("妈蛋气死劳资了战斗开始...")

        # 锁住Girl所需的boyHonor对象
        if boyHonor.acquire():
            print("Boy:Gril必须先道歉!")
            time.sleep(1)

            # 需要girlHonor才能继续执行
            if girlHonor.acquire(timeout=-1):
                girlHonor.release()
                boyHonor.release()
                print("Boy:im sorry too！")
            else:
                print("Boy:....")
        print("Boy战斗结束")


class Girl(Thread):
    def run(self):
        print("妈蛋气死老娘了战斗开始...")

        # 锁住Boy所需的girlHonor对象
        if girlHonor.acquire():
            print("Gril：Boy必须先道歉!")

            # 需要boyHonor才能继续执行
            if boyHonor.acquire(timeout=-1):
                boyHonor.release()
                girlHonor.release()
                print("Girl:im sorry too！")
            else:
                print("Girl:妈蛋分手！")
        print("Girl战斗结束")


if __name__ == '__main__':
    Boy().start()
    Girl().start()
    pass

```



