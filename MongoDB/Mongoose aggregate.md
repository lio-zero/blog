# Mongoose aggregate

[Mongoose 的 `aggregate()` 方法](https://mongoosejs.com/docs/api/model.html#model_Model.aggregate)是如何将 [MongoDB 的聚合框架](https://docs.mongodb.com/manual/aggregation/)与 Mongoose 一起使用的。Mongoose `aggregate()` 是一个小的包装器，因此任何在 [MongoDB shell（`mongo`）](https://docs.mongodb.com/manual/mongo/)中工作的任何聚合查询都应该在 Mongoose 中工作，而不做任何更改。

## 什么是聚合框架？

从语法上讲，聚合框架查询是一个阶段数组。阶段是 MongoDB 应该如何转换进入该阶段的任何文档的对象描述。第一阶段将文档馈送到第二阶段，依此类推，这样您就可以使用阶段组合转换。传递给 `aggregate()` 方法的阶段数组称为聚合管道。

### `$match` 阶段

`$match` 阶段过滤掉与给定 `filter` 参数不匹配的文档 ，类似于 Mongoose `find()` 方法的过滤器。

```js
await User.create([
  { name: 'O.O', age: 18 },
  { name: 'D.O', age: 19 },
  { name: 'K.O', age: 31 },
  { name: 'O.K', age: 19 },
  { name: 'LAY', age: 28 }
])

const filter = {
  age: {
    $gte: 30
  }
}

let docs = await User.aggregate([{ $match: filter }])

console.log(docs.length) // 1
console.log(docs[0].name) // K.O
console.log(docs[0].age) // 31

// $match 类似于 find()
docs = await User.find(filter)
console.log(docs.length) // 1
console.log(docs[0].name) // 'K.O'
console.log(docs[0].age) // 31
```

### `$group` 阶段

聚合可以做的不仅仅是过滤文档。您还可以使用聚合框架来转换文档。例如，`$group` 阶段的行为类似于 `reduce()` 方法。例如，`$group` 阶段允许您计算给定 `age` 的用户数量。

```js
let docs = await User.aggregate([
  {
    $group: {
      // 每个 _id 都必须是唯一的，因此如果有多个文档具有相同的期限，MongoDB 将增加 count。
      _id: '$age',
      count: {
        $sum: 1
      }
    }
  }
])

console.log(docs.length) // 4
docs.sort((d1, d2) => d1._id - d2._id)
console.log(docs[0]) // { _id: 18, count: 1 }
console.log(docs[1]) // { _id: 19, count: 2 }
console.log(docs[2]) // { _id: 28, count: 1 }
console.log(docs[3]) // { _id: 31, count: 1 }
```

### 结合多个阶段

聚合管道的优势在于其**可组合性**。例如，结合前两个例子，仅按 `age` 对用户进行分组，条件为 `age < 30`：

```js
let docs = await User.aggregate([
  {
    $match: {
      age: {
        $lt: 30
      }
    }
  },
  {
    $group: {
      _id: '$age',
      count: {
        $sum: 1
      }
    }
  }
])

console.log(docs.length) // 3
docs.sort((d1, d2) => d1._id - d2._id)
console.log(docs[0]) // { _id: 18, count: 1 }
console.log(docs[1]) // { _id: 19, count: 2 }
console.log(docs[2]) // { _id: 28, count: 1 }
```

## Mongoose `Aggregate` 类

Mongoose 的 `aggregate()` 方法返回 Mongoose [`Aggregate` 类](https://mongoosejs.com/docs/api/aggregate.html)的实例 。`Aggregate` 聚合实例是可聚合的，因此您可以将它们与 `await` 和 Promise 链一起使用。

`Aggregate` 类还支持用于构建聚合管道的链接接口。例如，下面的代码显示了另一种用于构建聚合管道的语法，该聚合管道后跟 `$match` 和 `$group`。

```js
let docs = await User.aggregate()
  .match({ age: { $lt: 30 } })
  .group({ _id: '$age', count: { $sum: 1 } })

console.log(docs.length) // 3
docs.sort((d1, d2) => d1._id - d2._id)
console.log(docs[0]) // { _id: 18, count: 1 }
console.log(docs[1]) // { _id: 19, count: 2 }
console.log(docs[2]) // { _id: 28, count: 1 }
```

[Mongoose 中间件](https://mongoosejs.com/docs/middleware.html)也支持 `pre('aggregate')` 和 `post('aggregate')` 钩子。您可以使用聚合中间件来转换聚合管道。

```js
const userSchema = new mongoose.Schema({ name: String, age: Number })
userSchema.pre('aggregate', function () {
  // 将 $match 添加到管道的开头
  this.pipeline().unshift({ $match: { age: { $lt: 30 } } })
})
const User = mongoose.model('User', userSchema)

// pre('aggregate') 向管道中添加一个 $match。
let docs = await User.aggregate().group({
  _id: '$age',
  count: { $sum: 1 }
})

console.log(docs.length) // 3
docs.sort((d1, d2) => d1._id - d2._id)
console.log(docs[0]) // { _id: 18, count: 1 }
console.log(docs[1]) // { _id: 19, count: 2 }
console.log(docs[2]) // { _id: 28, count: 1 }
```
