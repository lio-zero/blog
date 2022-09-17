# Mongoose 中的 db 属性

Mongoose 提供了许多强大的功能，如[中间件](https://mongoosejs.com/docs/middleware.html)和[验证](https://mongoosejs.com/docs/validation.html)。但有时您希望绕过 Mongoose 并使用 [Node.js MongoDB 驱动程序](https://www.npmjs.com/package/mongodb)。

Mongoose 连接有一个 `db` 属性，允许您访问 [MongoDB 驱动程序的 db 句柄](http://mongodb.github.io/node-mongodb-native/3.6/api/Db.html)：

```js
// 连接到运行在 localhost:27017 上的 MongoDB 服务器并使用，并使用 test 数据库
await mongoose.connect('mongodb://localhost:27017/test', {
  useNewUrlParser: true
})
```

Mongoose 不支持获取分析级别，而 MongoDB [profilingLevel](https://mongodb.github.io/node-mongodb-native/4.9/modules.html#ProfilingLevel) 方法可以获取当前数据库的分析级别：

```js
const profilingLevel = await mongoose.connection.db.profilingLevel()
console.log(profilingLevel) // off
```

通常情况下 `db` 属性就已足够了，但在某些情况下，您需要使用 [`MongoClient` 实例](https://mongodb.github.io/node-mongodb-native/4.9/classes/MongoClient.html)而不是 `db` 句柄。

```js
const client = mongoose.connection.getClient()
const profilingLevel = await client.db('otherdb').profilingLevel()
console.log(profilingLevel) // off
```
