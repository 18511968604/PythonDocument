```py
class TianyaPipeline(object):
    def __init__(self):
        self.f = open("tianya.txt", "w", encoding="utf-8")
    def process_item(self, item, spider):
        self.f.write(str(item))
        # return item
    def __del__(self):
        self.f.close()
```



