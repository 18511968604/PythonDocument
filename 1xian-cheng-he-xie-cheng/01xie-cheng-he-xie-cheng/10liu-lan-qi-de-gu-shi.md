```py
import gevent

answers = {
    "我们是什么": "浏览器",
    "我们要什么": "速度",
    "什么时候要": "现在",
    "还有问题吗": None,
}


def firefox(question):
    gevent.sleep(1)
    print("firefox:", answers[question])
    pass


def opera(question):
    gevent.sleep(1)
    print("opera:", answers[question])
    pass


def safari(question):
    gevent.sleep(1)
    print("safari:", answers[question])
    pass


def ie(question):
    gevent.sleep(10)
    print("ie:", answers[question])
    pass


if __name__ == '__main__':
    q1 = "我们是什么"
    q2 = "我们要什么"
    q3 = "什么时候要"
    q4 = "还有问题吗"

    print(q1)
    gevent.joinall([
        gevent.spawn(ie, q1),
        gevent.spawn(firefox, q1),
        gevent.spawn(opera, q1),
        gevent.spawn(safari, q1)
    ], timeout=2)
    print("----------\n")

    print(q2)
    gevent.joinall([
        gevent.spawn(ie, q2),
        gevent.spawn(firefox, q2),
        gevent.spawn(opera, q2),
        gevent.spawn(safari, q2)
    ], timeout=2)
    print("----------\n")

    print(q3)
    gevent.joinall([
        gevent.spawn(ie, q3),
        gevent.spawn(firefox, q3),
        gevent.spawn(opera, q3),
        gevent.spawn(safari, q3)
    ], timeout=2)
    print("----------\n")

    print(q4)
    gevent.joinall([
        gevent.spawn(ie, q4),
        gevent.spawn(firefox, q4),
        gevent.spawn(opera, q4),
        gevent.spawn(safari, q4)
    ], timeout=5)
    print("----------\n")
    pass

```



