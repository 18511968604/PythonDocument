## picklecompat.py {#picklecompatpy}

```py
"""A pickle wrapper module with protocol=-1 by default."""

try:
    import cPickle as pickle  # PY2
except ImportError:
    import pickle


def loads(s):
    return pickle.loads(s)


def dumps(obj):
    return pickle.dumps(obj, protocol=-1)

```

这里实现了loads和dumps两个函数，其实就是实现了一个序列化器。

因为redis数据库不能存储复杂对象（key部分只能是字符串，value部分只能是字符串，字符串列表，字符串集合和hash），所以我们存啥都要先串行化成文本才行。
