```py
'''
两条进程通过管道共享数据
'''
import multiprocessing

import time

# 发送数据
def sendData(conn):
    for i in range(5):
        conn.send("good afternoon")
        time.sleep(1)

    # 发送完毕关闭管道
    conn.close()
    pass


# 接收数据
def recvData(conn):
    mlist = []
    while True:
        try:
            # 当管道已关闭时，recv()可能抛出OSError异常，
            data = conn.recv()# 阻塞接收
            print(data)

            mlist.append(data)
            if len(mlist) > 3:
                conn.close()

        # 如果管道已关闭，
        except OSError:
            print("该死的管道已关闭")
            pass

    # 接收完毕关闭管道
    conn.close()

    pass

# 单向管道示例
def pipeDemo():
    # 创建一个单向传输的管道，其返回值是tupe，第0项为接收方Connection对象，第1项为发送方Connection对象
    pipe = multiprocessing.Pipe(duplex=False)
    # Connection().close()
    # 让发送进程持有pipe[1]，接收进程持有pipe[0]
    multiprocessing.Process(target=sendData, args=(pipe[1],)).start()
    multiprocessing.Process(target=recvData, args=(pipe[0],)).start()

def work1(conn):
    for i in range(5):
        # 连发两条信息
        conn.send("来自甲方的问候-%d"%(i))
        conn.send("来自甲方对全家的问候-%d" % (i))

        # 接收对方发来的两条信息
        print("甲方:收到问候{1}{0}".format(conn.recv(),conn.recv()))#format({0}，{1}，{2}...)
        time.sleep(1)

    # 关闭传输管道
    conn.close()
    pass

def work2(conn):
    for i in range(5):
        conn.send("来自乙方的问候-%d"%(i))
        conn.send("来自乙方对全家的问候-%d"%(i))
        print("乙方:收到问候{1}{0}".format(conn.recv(),conn.recv()))
        time.sleep(1)
    conn.close()
    pass

# 全双工管道示例
def duplexPipeDemo():
    # 创建一个全双工传输的管道，其返回值是tupe,内含两个Connection对象，都可以收发
    pipe = multiprocessing.Pipe(duplex=True)
    # 将两端的Connection对象分别传递给两个进程，双方都可以收发
    multiprocessing.Process(target=work1, args=(pipe[0],)).start()
    multiprocessing.Process(target=work2, args=(pipe[1],)).start()


if __name__ == "__main__":

    # pipeDemo()

    duplexPipeDemo()

    print("main over")

```



