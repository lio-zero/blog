# Mongoose 查询文档

## 查找所有文档

Mongoose [`Model.find(filter, callback)` 方法](https://mongoosejs.com/docs/api/model.html#model_Model.find)允许您查询具有给定键/值的文档，它将返回与给定过滤器匹配的文档数组。

Mongoose 的 Model.find()函数是一个重要的理解方法。您可以不带任何参数调用它，它将返回该模型中的所有文档。你可以传入一个 filter 告诉 Mongoose 在数据库中查找什么的参数。这 filter 可能是一个 objectId 或一个 object。使用时 Model.find()，您应该明确列出您在模型中搜索的参数。这在从查询字符串中提取过滤器参数时很重要。

猫鼬模型。find（）函数是一个重要的理解方法。您可以不带任何参数调用它，它将返回该模型中的所有文档。您可以传入一个过滤器，告诉 Mongoose 在数据库中查找什么。此筛选器可以是 objectId 或 object。

假设你有一个 [mongoose 模型](https://mongoosejs.com/docs/models.html) `User`，其中包含应用程序的所有用户。要获取集合中所有用户的列表，可以调用 `User.find()` 将空对象作为第一个参数：

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String,
    age: String
  })
)

await User.create([
  { name: 'D.O', age: 20 },
  { name: 'O.O', age: 19 },
  { name: 'K.O', age: 18 },
  { name: 'O.K', age: 17 },
  { name: 'O.O', age: 22 }
])

// 空的 filter 表示匹配所有文档
const filter = {}
const all = await User.find(filter)
```

同样地，你可以调用 `User.find()` 不带参数，也就是省略 `filter`，您将得到相同的结果。

```js
await User.find() // 返回上面带有 _id 属性和 __v 属性的数组
```

`Model.find()` 的默认行为是返回模型中的所有文档，因此如果传递的属性都不存在，它将所有文档。

但需要注意的是，不要直接查询字符串 `req.query` 直接传递给 `find` 方法，

```js
// 不要这样做，req.query 可能是空对象
// 在这种情况下，查询将返回每个文档。
await Model.find(req.query)
```

### sanitizeFilter 选项

Mongoose 6 引入了一个新 `sanitizeFilter` 选项，可以防御[查询选择器注入攻击](https://thecodebarbarian.com/2014/09/04/defending-against-query-selector-injection-attacks.html)。它只是将过滤器包装在 `$eq` 标签中，从而防止查询选择器注入攻击。

```js
// 使用 sanitizeFilter，Mongoose 将下面的查询转换为 { age, hashedPassword: { $eq: { $ne: null } } }
const user = await User.find({
  email: 'test@163.com',
  hashedPassword: { $ne: null }
}).setOptions({ sanitizeFilter: true })
```

### cursor

假设你的应用程序很受欢迎，你有数百万用户。一次将所有用户加载到内存是行不通的。要一次遍历所有用户，而不将其全部加载到内存中，请使用 [`cursor`](https://mongoosejs.com/docs/api/query.html#query_Query-cursor)。

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String,
    email: String
  })
)

// 注意此处没有 await
const cursor = User.find().cursor()

for (let doc = await cursor.next(); doc != null; doc = await cursor.next()) {}
```

或者，您可以使用异步迭代器。

```js
for await (const doc of User.find()) {
}
```

## 查询某些字段

要过滤 mongoose 中的对象属性，可以对查询使用 `select()` 方法。其作用是：选择要返回的字段。

```js
//将返回仅包含文档的年龄、名称和id属性的所有文档
await Model.find({}).select('name age')
```

### `_id` 属性

默认情况下，MongoDB 包含 `_id`。要在选择字段时排除 `_id`，可以使用 `.find().select({ name: 1, _id: 0 })` 或 `.find().select('name -_id')`。`0` 和 `-` 告诉 Mongoose 和 MongoDB 服务器显式排除 `_id`。

```js
await Model.find().select({ name: 1, _id: 0 });
// or
await Model.find().select({'name -_id'});
```

## 按 ID 查找

在 Mongoose 中，[`Model.findById()` 方法](https://mongoosejs.com/docs/api/model.html#model_Model.findById)用于根据文档的 `_id` 查找一个文档。`findById()` 方法接受单个参数，即文档 `id`。如果 MongoDB 找到具有给定 `id` 的文档，则返回解析为 Mongoose 文档的 Promise；如果未找到任何文档，则返回 `null`。

```js
const schema = new mongoose.Schema({ _id: Number }, { versionKey: false })
const Model = mongoose.model('MyModel', schema)

await Model.create({ _id: 1 })

await Model.findById(1) // { _id: 1 }
await Model.findById(2) // null，因为找不到任何文档
```

当您调用 `findById(_id)` 时，Mongoose 会在后台调用 `findOne({ _id })`。这意味着 `findById()` 触发 `findOne()` [中间件](https://mongoosejs.com/docs/middleware.html)。

```js
const schema = new mongoose.Schema({ _id: Number }, { versionKey: false })

schema.pre('findOne', function () {
  console.log('调用 findOne()')
})

const Model = mongoose.model('MyModel', schema)
await Model.create({ _id: 1 })

// 打印 findOne()，因为 findById() 调用 findOne()
await Model.findById(1)
```

[Mongoose 会强制转换查询以匹配您的模式](https://mongoosejs.com/docs/tutorials/query_casting.html)。这意味着如果您的 `_id` 是一个 [MongoDB ObjectId](https://docs.mongodb.com/manual/reference/method/ObjectId/)，您可以将 `_id` 作为字符串传递，Mongoose 将为您将其转换为 ObjectId。

```js
const _id = '9d641f2ed75f4e2513b90abc'
const schema = new mongoose.Schema(
  { _id: mongoose.ObjectId },
  { versionKey: false }
)
const Model = mongoose.model('MyModel', schema)

await Model.create({ _id: new mongoose.Types.ObjectId(_id) })

typeof _id // 'string'
// { _id: '9d641f2ed75f4e2513b90abc' }
const doc = await Model.findById(_id)

typeof doc._id // 'object'
doc._id instanceof mongoose.Types.ObjectId // true
```

## 链式

许多 Mongoose 模型函数，例如 [`find()`](https://thecodebarbarian.com/how-find-works-in-mongoose)，返回一个 [Mongoose Query](https://mongoosejs.com/docs/queries.html)。[Mongoose Query 类](https://mongoosejs.com/docs/api/query.html)提供了一个用于查找、更新和删除文档的[链式接口](https://schier.co/blog/2013/11/14/method-chaining-in-javascript.html)。

`Model.find()` 的第一个参数称为查询**过滤器**。当您调用 `find()` 时，MongoDB 将返回与查询过滤器匹配的所有文档。您可以使用 Mongoose 的[众多查询](https://mongoosejs.com/docs/api/query.html)来构建查询过滤器。只需确保使用 [`where()`](https://mongoosejs.com/docs/api/query.html#query_Query-where) 指定要添加到过滤器中的属性名即可。

```js
let docs = await User.find()
  // where 指定属性的名称，in() 指定 name 必须是数组中的两个值之一
  .where('name')
  .in(['D.O', 'O.K'])

// 相同的查询，但过滤器表示为对象，而不是使用链式
docs = await User.find({
  name: { $in: ['D.O', 'O.K'] }
})
```

可链式操作允许添加到当前查询筛选器。可以使用 [`query.getFilter()` 方法](https://mongoosejs.com/docs/api/query.html#query_Query-getFilter)获取查询的当前筛选器。

```js
const query = User.find().where('name').in(['D.O', 'O.K'])

// { name: { $in: ['D.O', 'O.K'] } }
query.getFilter()
```

以下是几个有用的查询方法列表：

- `lt(value)` 和 `gt(value)` — 指定一个属性必须小于（`lt()`）或大于（`gt()`）一个值。`value` 可以是数字、字符串或日期。
- `lte(value), gte(value)` — 指定一个属性必须大于或等于（`gte()`）或小于或等于（`gte()`）一个值。
- `in(arr)` — 指定一个属性必须等于 `arr` 中指定的值之一
- `nin(arr)` — 指定一个属性不能等于 `arr` 中指定的任何值
- `eq(val)` — 指定一个属性必须等于 `val`
- `ne(val)` — 指定一个属性不能等于 `val`
- `regex(re)` — 指定一个属性必须是 `re` 匹配的字符串

您可以链式调用任意多个 `where()` 和查询方法来建立查询。例如：

```js
const docs = await User.find()
  // name 必须与正则表达式匹配，age 必须在 29 到 59 岁之间
  .where('name')
  .regex(/o/i)
  .where('age')
  .gte(29)
  .lte(59)

docs.map((doc) => doc.name) // [ 'D.O', 'O.O', 'O.K' ]
```

## 使用 LIKE 查询

使用 [SQL LIKE 运算符](https://use-the-index-luke.com/sql/where-clause/searching-for-ranges/like-performance-tuning)允许您搜索带有通配符的字符串。MongoDB 没有类似的运算符，[`$text` 运算符](https://docs.mongodb.com/manual/reference/operator/query/text/)执行更复杂的文本搜索。但是 MongoDB 确实支持与 LIKE 类似的正则表达式查询。

例如，假设您想要查找 `email` 包含 `gmail` 的所有用户。您可以简单地通过 JavaScript 正则表达式 `/gmail/` 进行搜索：

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    email: String
  })
)

await User.create([
  { email: 'yyds@163.com' },
  { email: 'tbds@qq.com' },
  { email: 'test@163.com' },
  { email: '163@qq.com' }
])

const docs = await User.find({ email: /163/ })
docs.length // 3
docs.map((doc) => doc.email).sort() // ['yyds@163.com', 'test@163.com', '163@qq.com']
```

同样，您可以使用 `$regex` 运算符。

```js
const docs = await User.find({ email: { $regex: '163' } })
```

需要注意的是 mongoose 不会为您转义 regexp 中的特殊字符。如果要对用户输入的数据使用 `$regexp`，应首先使用 [escape-string-regexp](https://www.npmjs.com/package/escape-string-regexp) 或用于转义正则表达式特殊字符的类似库来清理字符串。

```js
const escapeStringRegexp = require('escape-string-regexp')

const User = mongoose.model(
  'User',
  new mongoose.Schema({
    email: String
  })
)

await User.create([
  { email: 'yyds@163.com' },
  { email: 'tbds@qq.com' },
  { email: 'test+foo@163.com' }
])

const $regex = escapeStringRegexp('+foo')
const docs = await User.find({ email: { $regex } })

docs.length // 1
docs[0].email // 'test+foo@163.com'

// Throws: MongoError: Regular expression is invalid: nothing to repeat
await User.find({ email: { $regex: '+foo' } })
```

## 查询运算符

在 Mongoose 中，Model.find() 函数是查询数据库的主要工具。find()的第一个参数是一个筛选器对象。MongoDB 将搜索与过滤器匹配的所有文档。如果传入一个空过滤器，MongoDB 将返回所有文档。

我们可以使用 [MongoDB 查询运算符](https://docs.mongodb.com/manual/reference/operator/query/)构造过滤器对象，从而在 Mongoose 中执行常见查询。

### 相等检查

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String,
    age: String
  })
)

await User.create([
  { name: 'D.O', age: 30 },
  { name: 'O.O', age: 29 },
  { name: 'K.O', age: 18 },
  { name: 'O.K', age: 40 },
  { name: 'O.O', age: 22 }
])
```

假设您要查找所有 `name` 为 O.O 的用户。可以将 `{ age: 'O.O' }` 作为 `filter` 传递。

```js
const docs = await User.find({ name: 'O.O' })

// MongoDB 可以按任何顺序返回文档，除非您明确排序
docs.map((doc) => doc.age).sort() // [29, 22]
```

你还可以按年龄查询。例如，下面的查询将查找 `age` 为 29 岁的所有字符。

```js
const docs = await User.find({ age: 29 })

docs.map((doc) => doc.name).sort() // ['O.O', 'O.K']
```

以上示例不使用任何查询运算符。如果将 `name` 的值设置为具有 [`$eq` 属性](https://docs.mongodb.com/manual/reference/operator/query/eq/#op._S_eq)的对象，则会得到一个等效的查询，但需要使用**查询运算符**。

```js
const docs = await User.find({ name: { $eq: 'O.O' } })

docs.map((doc) => doc.age).sort() // [29, 22]
```

### 比较查询运算符

`$eq` 查询运算符检查完全相等。还有一些[比较查询运算符](https://docs.mongodb.com/manual/reference/operator/query/#comparison)，比如 `$gt` 和 `$lt`。例如，假设您想查找年龄严格小于 29 岁的所有字符。您可以使用 `$lt` 查询运算符，如下所示。

```js
const docs = await User.find({ age: { $lt: 29 } })

docs.map((doc) => doc.name).sort() // ['K.O', 'O.O']
```

假设你想找到所有年龄至少为 29 岁的用户。您可以使用 [`$gte` 查询运算符](https://docs.mongodb.com/manual/reference/operator/query/gte/#op._S_gte)。

```js
const docs = await User.find({ age: { $gte: 29 } })

docs.map((doc) => doc.name).sort() // ['D.O', 'O.K', 'O.O']
```

比较运算符 `$lt`、[`$gt`](https://docs.mongodb.com/manual/reference/operator/query/gt/#op._S_gt)、[`$lte`](https://docs.mongodb.com/manual/reference/operator/query/lte/#op._S_lte) 和 `$gte` 不仅可以处理数字。您还可以在字符串、日期和其他类型上使用它们。MongoDB 使用 [unicode](https://www.w3.org/TR/xml-entity-names/bycodes.html) 顺序比较字符串。如果该顺序不适用于您，您可以使用 [MongoDB collations](https://thecodebarbarian.com/a-nodejs-perspective-on-mongodb-34-collations) 对其进行配置。

```js
const docs = await User.find({ name: { $lte: 'K.O' } })

docs.map((doc) => doc.name).sort() // [ 'D.O', 'K.O' ]
```

### 正则表达式

假设您要查找 `name` 包含 `K` 的用户。在 SQL 中，可以使用 [`LIKE` 运算符](https://www.w3schools.com/sql/sql_like.asp)。在 Mongoose 中，您可以简单地通过正则表达式进行查询，如下所示。

```js
const docs = await User.find({ name: /K/ })

docs.map((doc) => doc.name).sort() // ['K.O', 'O.K']
```

同样，您可以使用 [`$regex` 查询运算符](https://docs.mongodb.com/manual/reference/operator/query/regex/#op._S_regex)。这使您能够将正则表达式作为字符串传递，如果您是从 HTTP 请求中获取查询，这很方便。

```js
const docs = await User.find({ name: { $regex: 'K' } })

docs.map((doc) => doc.name).sort() // ['K.O', 'O.K']
```

### 包含 `$and` 和 `$or` 的组合

如果设置了多个 `filter` 属性，MongoDB 将查找与所有过滤器属性匹配的文档。例如，以下的查询将查找 `age` 至少为 29 岁且 `name` 等于 `'K.O'` 的所有用户。

```js
const docs = await User.find({
  name: 'K.O',
  age: { $gte: 29 }
})

docs.map((doc) => doc.name) // ['O.O']
```

假设您要查找 `age` 至少为 29 岁或 `name` 等于 `O.O` 的用户 。您需要 [`$or` 查询运算符](https://docs.mongodb.com/manual/reference/operator/query/or/#op._S_or)。

```js
const docs = await User.find({
  $or: [{ age: { $gte: 29 } }, { name: 'O.O' }]
})

docs.map((doc) => doc.name).sort() // [ 'D.O', 'O.O', 'O.K', 'O.O' ]
```

还有一个 [`$and` 查询运算符](https://docs.mongodb.com/manual/reference/operator/query/and/#op._S_and)。您很少需要使用 `$and` 查询运算符。`$and` 的主要用例是组合多个 `$or` 运算符。例如，假设您要查找满足以下两个条件的字符：

- `age` 至少 29 或 `name` 等于 `'O.O'`
- `name` 以字母开头，在 `O` 之前或 `O` 之后。

```js
const docs = await User.find({
  $and: [
    {
      $or: [{ age: { $gte: 29 } }, { rank: 'O.O' }]
    },
    {
      $or: [{ name: { $lte: 'O' } }, { name: { $gte: 'K' } }]
    }
  ]
})

docs.map((doc) => doc.name).sort() // [ 'D.O', 'O.O', 'O.K' ]
```
