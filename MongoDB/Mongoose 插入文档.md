# Mongoose 插入文档

在 MongoDB 中，[upsert](https://en.wiktionary.org/wiki/upsert) 是指在没有文档匹配 `filter` 的情况下插入新文档的更新。要在 Mongoose 中插入文档，应将 `upsert` 选项设置为 [`Model.updateOne()`](https://mongoosejs.com/docs/api.html#model_Model.updateOne) 方法：

```js
const res = await Character.updateOne(
  { name: 'O.O' },
  { $set: { age: 18 } },
  { upsert: true } // 将此更新设置为 upsert
)

// 如果 MongoDB 修改了现有文档，则为 1；如果 MongoDB 插入了新文档，则为 0。
console.log(res.nModified)
// 包含插入文档的描述数组，包括所有插入文档的 _id。
console.log(res.upserted)
```

要获取更新插入的文档，应该使用 [`Model.findOneAndUpdate()`](https://mongoosejs.com/docs/api.html#model_Model.findOneAndUpdate) 方法，而不是 `Model.updateOne()`。

```js
const doc = await Character.findOneAndUpdate(
  { name: 'O.O' },
  { $set: { age: 18 } },
  { upsert: true, new: true }
)

console.log(doc.name) // O.O
console.log(doc.age) // 18
```

Mongoose 将最多插入一个文档。即使将 [`Model.updateMany()`](https://mongoosejs.com/docs/api.html#model_Model.updateMany) 与 `upsert` 一起使用，Mongoose 最多也会插入一个文档。要批量插入多个文档，应使用 [`Model.bulkWrite()`](https://mongoosejs.com/docs/api.html#model_Model.bulkWrite) 方法。

```js
const res = await Character.bulkWrite([
  {
    updateOne: {
      filter: { name: 'D.O' },
      update: { age: 20 },
      upsert: true
    }
  },
  {
    updateOne: {
      filter: { name: 'KAI' },
      update: { age: 20 },
      upsert: true
    }
  }
])

// 包含由于 upsert 而插入的文档数
console.log(res.upsertedCount)
// 包含已更新的现有文档数。
console.log(res.modifiedCount)
```
