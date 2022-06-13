# Mongoose 解决 Query was already executed 问题

当给定查询执行两次时，Mongoose 会抛出 "Query was already executed"（查询已执行）错误。对此最常见的解释是您正在混合 `await` 和回调。

```js
await Model.updateMany({}, { $inc: { count: 1 } }, function (err) {})
// "MongooseError: Query was already executed"
```

这是因为 Mongoose 在收到回调或 `await` 时执行查询。如果您使用 `await` 并传递回调，则此查询将执行两次。

或者：

```js
Model.updateMany({}, { $inc: { count: 1 } }, function (err) {}).then(() => {})
```

此查询执行两次。一次是因为回调，一次是因为 `then()` 方法。

解决方案是跳过传递回调。在 Mongoose 中不需要回调，因为 Mongoose 支持 `promises` 和 `async/await`。

```js
await Model.updateMany({}, { $inc: { count: 1 } })
// or
Model.updateMany({}, { $inc: { count: 1 } }).then(() => {})
```

但如果我们想执行两次查询呢？可以使用 [`clone()`](https://mongoosejs.com/docs/api.html#query_Query-clone) 方法：

```js
let query = Model.findOne()

await query

// 抛出 "MongooseError: Query was already executed" 错误
await query

// ✅
await query.clone()
```
