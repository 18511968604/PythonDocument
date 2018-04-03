```py
littlesisterList = []
with open("./nasa.txt", "rb") as f:
    littlesister = f.readlines()
    for i in littlesister:
        lst = i.decode("gbk", "ignore").split("\t") 
        if len(lst) > 19:   # 筛选非法数据
            if lst[3].isdigit():
                littlesisterList.append(lst)

# for i in littlesisterList:
#     print(i[3])

littlesisterList.sort(key=lambda x: eval(x[3]), reverse=True)
with open("bigmama.txt", "w", encoding="utf-8") as f:
    for data in littlesisterList:
        f.write(str(data) + "\r\n")
```



