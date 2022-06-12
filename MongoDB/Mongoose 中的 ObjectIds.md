# Mongoose 中的 ObjectIds

默认情况下，MongoDB 在 [ObjectId](https://docs.mongodb.com/manual/reference/method/ObjectId/) 类型的每个文档上创建一个 `_id` 属性。许多其他数据库默认使用数字 id 属性，但在 MongoDB 和 Mongoose 中，id 默认为对象。

```js
const Model = mongoose.model('Test', mongoose.Schema({ name: String }))
const doc = new Model({ name: 'test' })

doc._id instanceof mongoose.Types.ObjectId // true
typeof doc._id // object
doc._id // 5d6ede6a0ba62570afcedd3a
```

## ObjectIds

MongoDB `ObjectIds` 通常使用 24 个十六进制字符串表示，如 `5d6ede6a0ba62570afcedd3a`。Mongoose 根据模式路径为您的 `ObjectIds` 强制转换 24 个字符字符串。

```js
const schema = mongoose.Schema({ testId: mongoose.ObjectId })
const Model = mongoose.model('Test', schema)

const doc = new Model({ testId: '5d6ede6a0ba62570afcedd3a' })

// testId 是一个 ObjectId，Mongoose 根据您的模式自动将 24 个十六进制字符字符串转换为 ObjectId。
doc.testId instanceof mongoose.Types.ObjectId // true
```

Mongoose 还可以将其他几个值转换为 `ObjectId`。关键的教训是 `ObjectId` 是 12 个任意字节。任何 12 字节的缓冲区或 12 个字符的字符串都是有效的 `ObjectId`。

```js
const schema = mongoose.Schema({ testId: mongoose.ObjectId })
const Model = mongoose.model('Test', schema)

// 任何 12 个字符的字符串都是有效的 ObjectId，因为 ObjectId 的唯一定义特性是它们有 12 个字节。
let doc = new Model({ testId: '12char12char' })
doc.testId instanceof mongoose.Types.ObjectId // true
doc.testId // '313263686172313263686172'

// 类似地，Mongoose 将自动将长度为 12 的缓冲区转换为 ObjectID。
doc = new Model({ testId: Buffer.from('12char12char') })
doc.testId instanceof mongoose.Types.ObjectId // true
doc.testId // 313263686172313263686172
```

## 从 ObjectId 获取时间戳

`ObjectId` 对创建它们的本地时间进行编码。这意味着您通常可以从文档的 `_id` 中提取文档创建的时间。

```js
const schema = mongoose.Schema({ testId: mongoose.ObjectId })
const Model = mongoose.model('Test', schema)

const doc = new Model({ testId: '313263686172313263686172' })
doc.testId.getTimestamp() // 1996-02-27T01:50:32.000Z
doc.testId.getTimestamp() instanceof Date // true
```

## 为什么是 ObjectIds？

假设您正在构建自己的数据库，并且希望在每个新文档上设置一个数字 `id` 属性。`id` 属性应该增加，因此插入的第一个文档的 `id = 0`，然后 `id = 1`，依此类推。

在单个进程中递增计数器是一个容易的问题。但是，如果您有多个进程，比如分片[集群](https://docs.mongodb.com/manual/sharding/)，该怎么办？现在，每个进程都需要能够递增计数器，因此无论何时插入文档，都需要递增分布式计数器。如果两个进程之间存在显著的网络延迟，则可能导致性能不可靠；如果一个进程停机，则可能导致不可预测的结果。

`ObjectId` 旨在解决这个问题。[`ObjectId` 冲突的可能性很小](https://docs.mongodb.com/manual/reference/bson-types/#objectid)，因此 MongoDB 可以分配 ID，这些 ID 在没有进程间通信的分布式系统中可能是唯一的。
