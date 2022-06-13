# Mongoose create() 方法

Mongoose `model` 有一个 [`create()` 方法](https://mongoosejs.com/docs/api/model.html#model_Model.create)通常用于创建新文档。

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String
  })
)

const doc = await User.create({ name: 'O.O' })

console.log(doc instanceof User) // true
console.log(doc.name) // 'O.O'
```

`create()` 方法是 `save()` 方法的封装。上述 `create()` 调用相当于：

```js
const doc = new User({ name: 'O.O' })
await doc.save()
```

使用 `create()` 最常见的原因是，通过传递一个对象数组，您可以通过单个方法调用方便地 `save()` 多个文档：

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String
  })
)

const docs = await User.create([{ name: 'O.O' }, { name: 'K.O' }])
console.log(docs[0] instanceof User) // true
console.log(docs[0].name) // 'O.O'
console.log(docs[1].name) // 'K.O'
```

## 使用会话和事务

除了传递对象数组之外，`create()` 还支持传入单个对象或对象的扩展。例如，下面是创建多个文档的另一种方法。

```js
// 保存两个新文档。
await User.create({ name: 'O.O' }, { name: 'D.O' })
```

不幸的是，如果您想将选项传递给 `create()` 方法，比如您想使用 [transactions](https://mongoosejs.com/docs/transactions.html)，扩展语法会导致语法歧义。例如，下面的代码将尝试创建两个文档，而不是将第二个参数视为 `options` 对象。

```js
const session = await User.startSession()

await session.withTransaction(async () => {
  // 注意，以下内容将不工作！它不是创建一个带有关联 session 的文档，而是创建两个没有 session 的文档！
  await User.create({ name: 'D.O' }, { session })
})
```

因此，如果要在事务中使用 `create()`，则**必须**将文档作为数组传递，即使只创建一个文档也是如此。

```js
const session = await User.startSession()

await session.withTransaction(async () => {
  // 使用给定 session 创建一个文档。
  await User.create([{ name: 'D.O' }], { session })
})
```

## 与 `insertMany()`

`model` 还有一个 [`insertMany()` 方法](https://mongoosejs.com/docs/api/model.html#model_Model.insertMany)在语法上类似于 `create()`。

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String
  })
)

const [doc] = await User.insertMany([{ name: 'O.O' }])

console.log(doc instanceof User) // true
console.log(doc.name) // 'O.O'
```

最大的区别是，`insertMany()` 最终作为一个原子 `insertMany()` 命令，Mongoose 将它发送给 MongoDB 服务器，而 `create()` 最终作为一组单独的 `insertOne()` 调用。虽然这意味着 `insertMany()` 通常更快，但也意味着 `insertMany()` 更容易受到[**慢查询**](https://docs.mongodb.com/manual/tutorial/manage-the-database-profiler/)的影响。因此，建议使用 `create()` 而不是 `insertMany()`，除非您愿意冒险减慢其他操作以加快批量插入。

另一个区别是 `create()` 触发 `save()` 中间件，因为 `create()` 在内部调用 `save()`。`insertMany()` 不会触发`save()` 中间件，但它会触发 `insertMany()` 中间件。
