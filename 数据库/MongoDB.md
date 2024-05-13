
# 连接数据库
```python
from pymongo.errors import ConnectionFailure
from pymongo import MongoClient

mongo_url = "mongodb://localhost:27017/"
database_name = "db_test"
collection_name = "coll_a"

try:
    client = MongoClient(mongo_url)
    client.server_info()
    print("Connected to MongoDB successfully!")
except ConnectionFailure as e:
    print(f"Could not connect to MongoDB: {e}")
finally:
    client.close()
```


# 插入一条文档
```python
document = {"id": "1001", "name": "polo", "age": 10}
res = collection.insert_one(document)

print(res)  # InsertOneResult(ObjectId('663f60e154717a764b3e0988'), acknowledged=True)
print(res.inserted_id)  # 663f60e154717a764b3e0988
print(res.acknowledged)  # True
```


# 插入多条文档
```python
document_list = [{"name": "peter", "age": 15}, {"name": "henry", "age": 20}]
res = collection.insert_many(document_list)

print(res)  # InsertManyResult([ObjectId('663f61284e3765677ee84b84'), ObjectId('663f61284e3765677ee84b85')], acknowledged=True)
print(res.inserted_ids)  # [ObjectId('663f61284e3765677ee84b84'), ObjectId('663f61284e3765677ee84b85')]
print(res.acknowledged)  # True
```



