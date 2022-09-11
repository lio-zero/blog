# 使用 Mongoose 验证数据唯一性

在使用 Mongoose 时，如果您想防止数据库中的值重复，有以下几种方法：

- 在 `SchemaType` 中定义 `validation`
- **schema** 中创建自定义验证
- Mongoose 的内置验证
- `unique` 属性（推荐）

您可以使用 [validation](https://mongoosejs.com/docs/validation.html) 防止数据库中的重复。`validation` 在 `SchemaType` 中定义，是一个中间件。您还可以在 schema 中创建自定义验证，也可以使用 Mongoose 的内置验证。

防止重复最简单的方式是使用 `unique` 属性，因为它将告诉 Mongoose 每个文档都应该有一个 [`unique`](https://masteringjs.io/tutorials/mongoose/unique) 给定路径的值。

例如，我们有一个 `email` 字段，它在文档中是不允许有重复的：

```js
const User = mongoose.model(
  'User',
  mongoose.Schema({
    email: {
      type: String,
      required: true,
      match: /.+\@.+\..+/,
      unique: true
    }
  })
)
```

如果等待索引生成，则可以使用 Mongoose Promise 基于事件的 `Model.init()`，如下所示：

```js
// 提前创建一些数据
await User.create([
  { email: 'xyz@163.com' },
  { email: '669@qq.com' },
  { email: 'test@gmail.com' }
])

await User.init()
```

这时，如果我们为 `User` 集合内记录一条新的文档，将引发错误：

```js
try {
  await User.create({ email: 'test@163.com' })
} catch (err) {
  console.log(err.message) // 'E11000 duplicate key error...'
}
```

需要注意的是，[`unique` 属性](https://mongoosejs.com/docs/validation.html#the-unique-option-is-not-a-validator)不是验证器。

如果您想了解其他验证数据唯一性的方法，请查阅文档 [validation](https://mongoosejs.com/docs/validation.html) 章节。

以上这种方式我在 [Mongoose 唯一索引（unique）](https://github.com/lio-zero/blog/blob/main/MongoDB/Mongoose%20%E5%94%AF%E4%B8%80%E7%B4%A2%E5%BC%95%EF%BC%88unique%EF%BC%89.md) 这篇文章也有介绍到，可以看看。
