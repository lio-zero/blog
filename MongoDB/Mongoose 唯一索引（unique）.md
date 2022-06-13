# Mongoose 唯一索引（unique）

Mongoose 的唯一索引 `unique` 选项都作用是，对于给定的路径，每个文档必须具有唯一的值。例如，下面是如何告诉 Mongoose 用户的 `email` 必须是唯一的。

```js
const mongoose = require('mongoose')

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    unique: true // email 必须是唯一的
  }
})
const User = mongoose.model('User', userSchema)
```

如果您尝试使用相同的 `name` 创建两个用户，您将得到一个重复密钥错误。

```js
// 抛出 MongoError:E11000 重复密钥错误集合
await User.create([{ email: 'test@163.com' }, { email: 'test2@163.com' }])

const doc = new User({ email: 'test@163.com' })
// 抛出 MongoError:E11000 重复密钥错误集合
await doc.save()
```

更新还可能引发重复的密钥错误。例如，如果您创建了一个具有唯一电子邮件地址的用户，然后将其电子邮件地址更新为非唯一值，您将得到相同的错误。

```js
await User.create({ email: 'test2@163.com' })

// 抛出 MongoError:E11000 重复密钥错误集合
await User.updateOne({ email: 'test2@163.com' }, { email: 'test@163.com' })
```

## `unique` 定义索引，而不是验证器

一个常见的问题是 `unique` 选项告诉 Mongoose 定义一个[唯一索引](https://docs.mongodb.com/manual/core/index-unique/)。这意味着当您使用 `validate()` 时，Mongoose 不会检查唯一性。

```js
await User.create({ email: 'test@163.com' })

const doc = new User({ email: 'test@163.com' })
await doc.validate() // 不会抛出错误
```

在编写自动测试时，[`unique` 定义索引而不是验证器](https://mongoosejs.com/docs/validation.html#the-unique-option-is-not-a-validator)这一点很重要。如果删除 `User` 模型所连接的数据库，还将删除 `unique` 索引，并且可以保存重复的索引。

```js
await mongoose.connection.dropDatabase()

// 成功，因为 unique 索引已消失！
await User.create([{ email: 'test@163.com' }, { email: 'test@163.com' }])
```

在生产环境中，通常不会删除数据库，因此这在生产环境中很少成为问题。

编写 Mongoose 测试时，通常建议使用 [`deleteMany()`](https://mongoosejs.com/docs/api/model.html#model_Model.deleteMany) 清除测试之间的数据，而不是 [`dropDatabase()`](https://mongoosejs.com/docs/api/connection.html#connection_Connection-dropDatabase)。这样可以确保删除所有文档，而无需清除数据库级别的配置，如索引和排序规则 [`deleteMany()` 也比 `dropDatabase()` 快得多](https://mongoosejs.com/docs/api/connection.html#connection_Connection-dropDatabase)。

但是，如果选择在测试之间删除数据库，则可以使用 [`Model.syncIndexes()`](https://mongoosejs.com/docs/api.html#model_Model.syncIndexes) 方法重新生成所有唯一索引。

```js
await mongoose.connection.dropDatabase()

// 重新生成所有索引
await User.syncIndexes()

// 抛出 MongoError:E11000 重复密钥错误集合
await User.create([{ email: 'test@163.com' }, { email: 'test@163.com' }])
```

## 处理 `null` 值

`null` 是一个不同的值，您不能保存两个具有 `null` 的 `email` 用户。同样，不能保存两个没有 `email` 属性的用户。

```js
// 抛出，因为两个文档都有 undefined
await User.create([{}, {}])

// 抛出，因为两个文档都有 null
await User.create([{ email: null }, { email: null }])
```

一种解决方法是使用 `required` 属性 ，这将不允许 `null` 和 `undefined` 的值存在：

```js
const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true // email 必须是唯一的
  }
})
```

如果您需要 `email` 是唯一的，除非它没有定义，您可以改为定义一个 **[Sparse Indexes（稀疏索引）](https://docs.mongodb.com/manual/core/index-sparse/)** 在 `email` 上，如下所示。

```js
const userSchema = new mongoose.Schema({
  email: {
    type: String, // email 必须是唯一的，除非没有定义
    index: {
      unique: true,
      sparse: true
    }
  }
})
```

## 用户友好的重复键错误

要使 `MongoDB E11000` 错误消息对用户友好，您可以使用 [mongoose-beautiful-unique-validation](https://www.npmjs.com/package/mongoose-beautiful-unique-validation) 包。

```js
const schema = new mongoose.Schema({ name: String })
schema.plugin(require('mongoose-beautiful-unique-validation'))

const UserModel = mongoose.model('User', schema)

const doc = await UserModel.create({ name: 'O.O' })

try {
  // 尝试创建具有相同 _id 的文档。这将始终失败，因为 MongoDB 集合在 _id 上总是有唯一的索引。
  await UserModel.create(Object.assign({}, doc.toObject()))
} catch (err) {
  // _id 不是唯一的。
  console.log(err.errors['_id'].message)
}
```
