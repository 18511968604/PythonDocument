```py
'''
1、协程

    协程，又称微线程，纤程。英文名Coroutine。

2、协程是啥 ？？

首先我们得知道协程是啥？协程其实可以认为是比线程更小的执行单元。
为啥说他是一个执行单元，因为他自带CPU上下文。
这样只要在合适的时机，我们可以把一个协程切换到另一个协程,
只要这个过程中保存或恢复 CPU上下文那么程序还是可以运行的。

通俗的理解：在一个线程中的某个函数，可以在任何地方保存当前函数的一些临时变量等信息，
然后切换到另外一个函数中执行，注意不是通过调用函数的方式做到的，
并且切换的次数以及什么时候再切换到原来的函数都由开发者自己确定

3、协程和线程差异

最大的优势就是协程极高的执行效率,因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销
线程切换从系统层面远不止保存和恢复 CPU上下文这么简单。
操作系统为了程序运行的高效性每个线程都有自己缓存Cache等等数据，
操作系统还会帮你做这些数据的恢复操作。所以线程的切换非常耗性能。
但是协程的切换只是单纯的操作CPU的上下文，所以一秒钟切换个上百万次系统都抗的住。

第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，

利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核
Python对协程的支持是通过generator(生成器)实现的。

4、协程的问题

但是协程有一个问题，就是系统并不感知，所以操作系统不会帮你做切换。
那么谁来帮你做切换？让需要执行的协程更多的获得CPU时间才是问题的关键。

'''

import threading
# 实现A、B交替打印
def A():
    while True:
        print("=====A=====")
        time.sleep(1)
    # B()

def B():
    while True:
        print("=====B=====")
        time.sleep(1)
    # A()

# 协程一个简单实现
import time


def C():
    while True:
        print("----C---")
        yield
        time.sleep(0.5)


def D(c):
    while True:
        print("----D---")
        next(c)
        time.sleep(0.5)


# 生产消费者模型
'''
传统的生产者-消费者模型是一个线程写消息，一个线程取消息，
通过锁机制控制队列和等待，但一不小心就可能死锁
'''


def consumer():  # consumer函数是一个generator
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[消费者] 消费了 %s...' % n)
        r = 'OK'


def produce(c):
    c.send(None)  # 启动生成器
    n = 0
    while n < 5:
        n = n + 1
        print('[生产者] 生产了 %s...' % n)
        r = c.send(n)
        print('[生产者] 消费者结果: %s' % r)
    c.close()


if __name__ == '__main__':
    # 传统
    # A()
    # B()  # 超出最大递归深度

    # a = threading.Thread(target=A)
    # b = threading.Thread(target=B)
    # a.start()
    # b.start()

    # 简单协程
    # c = C()
    # D(c)

    # c = consumer()
    # produce(c)
    pass
```



