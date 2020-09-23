# python操作mongo

安装依赖包：pip install pymongo

```python
# -*- coding:utf-8 -*-
import pymongo


class MongoDemo(object):

    def __init__(self):
        self.client = pymongo.MongoClient("mongodb://app:123456@10.255.175.224:27017/")
        self.db = self.client.app
        self.col = self.db.test

    def __del__(self):
        self.client.close()

    def insert(self):
        print("---------------插入---------------")
        data = {
            "name": "mongo1",
            "type": "test",
            "num": 1,
            "tags": [
                "demo1",
                "demo2"
            ],
            "info": {
                "field1": {
                    "value": 1
                }
            }
        }
        data_id = self.col.insert_one(data)
        print(data_id.inserted_id)
        data_list = [
            {
                "_id": 2,
                "name": "mongo2",
                "type": "test",
                "num": 2
            },
            {
                "_id": 3,
                "name": "mongo3",
                "type": "test",
                "num": 3
            }
        ]
        data_id = self.col.insert_many(data_list)
        print(data_id.inserted_ids)

    def find(self):
        print("---------------查找---------------")
        for i in self.col.find():
            print(i)

        for i in self.col.find({"name": "mongo2"}):
            print(i)

    def update(self):
        print("---------------更新---------------")
        self.col.update_one({"type": "test"}, {"$set": {"type": "test1"}})
        for i in self.col.find():
            print(i)
        self.col.update_many({"type": "test"}, {"$set": {"type": "test1"}})
        for i in self.col.find().sort("num", -1):
            print(i)

    def delete(self):
        print("---------------删除---------------")
        self.col.delete_one({"type": "test1"})
        for i in self.col.find():
            print(i)
        self.col.delete_many({"type": "test1"})
        for i in self.col.find():
            print(i)

    def run(self):
        self.insert()
        self.find()
        self.update()
        # self.delete()


def main():
    client = MongoDemo()
    client.run()


if __name__ == '__main__':
    main()

```
