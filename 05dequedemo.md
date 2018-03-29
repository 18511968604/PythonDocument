```py
#! C:\Python36\python.exe
'''
about what
'''
from collections import deque

if __name__ == "__main__":
    dq = deque([])
    dq.append(1)
    dq.append(2)
    dq.append(3)

    # 从头部弹出一个元素
    print(dq.popleft())

    # 从尾部弹出一个元素
    print(dq.pop())

    print(dq)
    print("main over")
```



