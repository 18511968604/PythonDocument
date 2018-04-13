## dupefilter.py {#dupefilterpy}

负责执行requst的去重，实现的很有技巧性，使用redis的set数据结构。但是注意scheduler并不使用其中用于在这个模块中实现的dupefilter键做request的调度，而是使用queue.py模块中实现的queue。

当request不重复时，将其存入到queue中，调度时将其弹出。

