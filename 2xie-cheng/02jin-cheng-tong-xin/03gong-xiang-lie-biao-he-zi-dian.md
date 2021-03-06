```py
'''
共享列表和字典
'''
import multiprocessing


# 编辑共享的列表和字典（以传参形式让进程持有数据对象（的地址））
def writeData(plist, pdict, event):
    plist += ["Python", "Java"]
    plist.append("C++")
    pdict["name"] = "冷锋"
    event.set()
    print("event set")
    pass


# 读取共享数据（以传参形式让进程持有数据对象（的地址））
def readData(plist, pdict, event):
    event.wait()
    print("event got")
    print(plist)
    print(pdict)
    event.clear()
    pass


if __name__ == "__main__":
    event = multiprocessing.Event()

    # 共享字典和列表的前提：业务完成以前，【生产字典和列表的multiprocessing.Manager对象】不能释放
    with multiprocessing.Manager() as pm:
        # 创建可以被进程共享的列表和字典对象（不是普通的列表和字典）
        plist = pm.list()
        pdict = pm.dict()
        print(type(plist))

        # 不要重新赋值，以免改变对象类型
        # plist = []
        # pdict = {}
        # print(type(plist))

        # 让读写双方进程持有共享的数据对象
        wp = multiprocessing.Process(target=writeData, args=(plist, pdict, event))
        rp = multiprocessing.Process(target=readData, args=(plist, pdict, event))

        wp.start()
        rp.start()

        # 阻塞主线程以免pm对象被释放
        wp.join()
        rp.join()

    print("main over")

```



