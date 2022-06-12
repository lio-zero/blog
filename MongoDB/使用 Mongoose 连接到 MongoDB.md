# 使用 Mongoose 连接到 MongoDB

本文带你了解如何使用 Mongoose 连接到 MongoDB 服务器。

> 官方介绍：[Mongoose](https://www.npmjs.com/package/mongoose) 是一个 MongoDB 对象建模工具，设计用于异步环境。它支持 Promise 和回调。

`mongoose.connect()` 方法是使用 mongoose 连接 MongoDB 的最简单方法。一旦连接成功，就可以创建 [Mongoose 模型](https://mongoosejs.com/docs/models.html)并开始与 MongoDB 交互。

```js
// 连接到运行在 localhost:27017 上的 MongoDB 服务器，并使用 test 数据库。
await mongoose.connect('mongodb://localhost:27017/test', {
  useNewUrlParser: true
})

// 一旦连接到 MongoDB，就可以创建用户模型并使用它将用户保存到数据库中。
const userSchema = new mongoose.Schema({ name: String })
const UserModel = mongoose.model('User', userSchema)

await UserModel.create({ name: 'test' })
```

如果 Mongoose 成功连接到 MongoDB，[`mongoose.connect()` 方法将返回一个 Promise](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-connect)，如果 Mongoose 无法连接，则 Promise 将被拒绝。

```js
const options = { useNewUrlParser: true }
// 尝试连接到 domain，该操作将失败
const err = await mongoose
  .connect('mongodb://domain:27017/test', options)
  .catch((err) => err)
// 第一次连接时未能连接到服务器 [domain:27017]
console.log(err.message)
```

有许多旧教程建议监听连接事件，如下：

```js
// 成功连接时
mongoose.connection.on('connected', function () {
  console.log('成功连接到 ' + dbURI)
})
// 如果连接抛出错误
mongoose.connection.on('error', function () {
  console.log('连接错误：' + err)
})

// 当连接断开时
mongoose.connection.on('disconnected', function () {
  console.log('连接断开')
})
```

这种方法并不是绝对必要的，如果 mongoose 最初连接到 MongoDB 时出现错误，则 `mongoose.connect()` 返回的 Promise 为 `rejects`。一旦 Mongoose 成功连接，它会在失去连接时自动处理重新连接。

## `reconnectFailed` 事件

Mongoose 会自动重新连接到 MongoDB。在内部，如果您连接到单个服务器，则基础 MongoDB 驱动程序会尝试每 `reconnectInterval` 毫秒重新连接 `reconnectTries` 次。您可以在 [`mongoose.connect()` 选项](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-connect)中设置 `ReconnectTrys` 和 `reconnectInterval`。

```js
mongoose.connect('mongodb://localhost:27017/test', {
  useNewUrlParser: true, // Boilerplate // 如果失去连接，请尝试每 2 秒重新连接一次。尝试 60 次后，放弃并发出 reconnectFailed。
  reconnectTries: 60,
  reconnectInterval: 2000
})
```

当 Mongoose 放弃时，它会在[连接](https://mongoosejs.com/docs/api/connection.html)上发出 `reconnectFailed` 事件。

```js
// 如果猫鼬放弃了重新连接的尝试，请终止该过程。
mongoose.connection.on('reconnectFailed', () => {
  process.nextTick(() => {
    throw new Error('Mongoose 无法重新连接到 MongoDB 服务器')
  })
})
```

如果您已连接到副本集，则 `reconnectTries` 和 `reconnectTries` 不会执行任何操作。如果 Mongoose 在初始连接后失去与副本集的连接，它将继续无限期地重新连接。
