## pymongo



### 更新数据

```python
filter = {"name": "John"}  # 指定要更新的文档条件
update = {"$set": {"age": 30}}  # 更新的字段和值
result = collection.update_one(filter, update)
```

### 遍历数据

```python
collection = db['mycollection']

# 查询_idzui'xiao一条数据
data = collection.find_one({}, sort=[("_id", pymongo.ASCENDING)])
while data:
    # 处理数据
    print(data)

    # 查询下一条数据
    data = collection.find_one({"_id": {"$gt": data["_id"]}})
```

