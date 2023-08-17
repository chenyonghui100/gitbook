---
description: 默认返回 更新前的数据
---

# FindOneAndUpdate

### 问题描述

在 golang 中使用 monogo 作为数据库，在对数据进行更新的时候，使用 `FindOneAndUpdate`   返回的结果是更新之前的数据，而预期的是更新之后的数据。

```go
err = monogoCollection.FindOneAndUpdate(
    ctx, 
    bson.M{"customerId": customerId, "_id": objectId}, 
    bson.M{"$set": updates}, 
).Decode(&equip)
```

FindOneAndUpdate 默认返回更新之前的数据。如果需要返回修改之后的值， 需要修改 `returnDocument`  参数的值为 `after`&#x20;

```go
types := options.After
opt := options.FindOneAndUpdateOptions{
	ReturnDocument: &types,
}
err = monogoCollection.FindOneAndUpdate(
	ctx, 
	bson.M{"customerId": customerId, "_id": objectId}, 
	bson.M{"$set": updates}, 
	&opt
o).Decode(&equip)
```

### 参考文档

{% embed url="https://www.mongodb.com/docs/manual/reference/method/db.collection.findOneAndUpdate/" %}
