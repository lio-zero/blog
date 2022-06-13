# Mongoose 中的 Promise

[Mongoose 内置了对 promises 的支持](https://mongoosejs.com/docs/promises.html)。在 Mongoose 5.x 以上的异步操作中，如 `.save()` 和 `.find().exec()` 返回一个 Promise，除非您传递一个回调函数。

```js
const Model = mongoose.model(
  'test',
  new mongoose.Schema({
    name: String
  })
)

const doc = new Model({ name: 'O.O' })

const promise = doc.save()
console.log(promise instanceof Promise) // true

// 回调
const res = doc.save(function callback(err) {
  /* ... */
})

console.log(res) // undefined
```

## `mongoose.Promise` 属性

Mongoose 单例有一个 [`Promise` 属性](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-Promise)，可用于设置 Mongoose 使用的 Promise 库。例如，您可以让 Mongoose 使用流行的 [Bluebird Promise 库](https://www.npmjs.com/package/bluebird)：

```js
const Bluebird = require('bluebird')

// 让 Mongoose 使用 Bluebird 而不是内置的 promises。
mongoose.Promise = Bluebird

const doc = new Model({ name: 'D.O' })

const promise = doc.save()
promise instanceof Promise // false
promise instanceof Bluebird // true
```

如果您尚未升级到 Mongoose 5.x 以上版本，您可能会在 Mongoose 4.x 中看到以下弃用警告：

> **WARNING: Mongoose: mpromise (mongoose's default promise library) is deprecated, plug in your own promise library instead**

要解决该弃用警告，请添加以下代码：

```js
mongoose.Promise = global.Promise
```

那是因为 Mongoose 5.x 的重大变化之一是改用 Node.js 的原生 promise。Mongoose 4.x 在 ES6 之前发布，因此它有自己的 promise 实现，与原生 JavaScript promise 略有不同。

如果在使用 Mongoose 5 的代码中看到 `mongoose.Promise = global.Promise`，请将其删除。默认情况下，Mongoose 5.x 使用原生 Promise，因此该代码在 Mongoose 5 中将不起作用。

## `Query` 不是 Promise

`save()` 方法返回一个 Promise，而 Mongoose 的 `find()` 方法返回一个 [Mongoose `Query`](https://mongoosejs.com/docs/queries.html)。

```js
const query = Model.find()

query instanceof Promise // false
query instanceof mongoose.Query // true
```

Mongoose 查询是可选项。换句话说，查询有一个 [`then()` 方法](https://mongoosejs.com/docs/api/query.html#query_Query-then)，其行为类似于 Promise `then()` 方法。因此，您可以使用带有 Promise 链和 `async/await` 的查询。

```js
// 使用带有 promise 链的查询
Model.findOne({ name: 'D.O' })
  .then((doc) => Model.updateOne({ _id: doc._id }, { name: 'O.O' }))
  .then(() => Model.findOne({ name: 'O.O' }))
  .then((doc) => console.log(doc.name)) // O.O

// 使用 async/await 查询
const doc = await Model.findOne({ name: 'O.O' })
console.log(doc.name) // O.O
```
