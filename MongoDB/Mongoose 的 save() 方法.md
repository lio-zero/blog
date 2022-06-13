# Mongoose 的 save() 方法

Mongoose 的 [`save()` 方法](https://mongoosejs.com/docs/api/model.html#model_Model-save)是将文档更改保存到数据库的一种方法。在 Mongoose 中更新文档有几种方法，如：[`update`、`updateOne`](https://mongoosejs.com/docs/api.html#document_Document-update)。但 `save()` 是功能最齐全的方法。除非有充分理由不更新文档，否则应使用 `save()` 更新文档。

## 使用 `save()` 进行操作

`save()` 是 [Mongoose documents](https://mongoosejs.com/docs/documents.html) 上的一个方法。`save()` 方法是异步的，因此它返回一个可以 `await` 执行的 Promise。

当您使用 `new` 创建 [Mongoose 模型](https://mongoosejs.com/docs/models.html)的实例时，调用 `save()` 会使 Mongoose 插入一个新文档。

```js
const Person = mongoose.model(
  'Person',
  new mongoose.Schema({
    name: String,
    rank: Number
  })
)

const doc = new Person({
  name: 'O.O',
  age: 18
})
// 插入一个 name = O.O, age = 18 的新文档
await doc.save()

const person = await Person.findOne()
console.log(person.name) // O.O
```

如果从数据库加载现有文档并对其进行修改，则 `save()` 会更新现有文档。

```js
const person = await Person.findOne()
person.name // O.O

// Mongoose 跟踪文档的更改。Mongoose 跟踪您设置的 age 属性，并将更改持久化到数据库中。
person.age = 18
await person.save()

// 从数据库加载文档并查看更改
const docs = await Person.find()

console.log(docs.length) // 1
console.log(docs[0].age) // 18
```

Mongoose 的更改跟踪根据您对文档所做的更改向 MongoDB 发送最小更新。您可以设置 [Mongoose 的调试模式](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-set)，以查看 Mongoose 发送给 MongoDB 的操作。

```js
mongoose.set('debug', true)

person.age = 18
// Mongoose: people.updateOne({ _id: ObjectId("...") }, { '$set': { age: 18 } })
await person.save()
```

## 验证

Mongoose 在保存之前验证修改后的路径。如果将字段设置为无效值，Mongoose 将在尝试 `save()` 该文档时抛出错误。

```js
const Person = mongoose.model(
  'Person',
  new mongoose.Schema({
    name: String,
    age: Number
  })
)

const doc = await Person.create({ name: 'Will Riker', age: 18 })

// 将 name 设置为无效值是可以的。。。
doc.age = 'lotto'

// 但尝试 save() 时，文档会出错
const err = await doc.save().catch((err) => err)
err // 路径 age 处的值 lotto 的强制转换为数字失败

// 但是，如果将 age 设置为有效值，则 save() 将成功。
doc.age = 20
await doc.save()
```

## 中间件

[Mongoose 中间件](https://mongoosejs.com/docs/middleware.html)允许您在每次调用 `save()` 时告诉 Mongoose 执行一个方法。例如，调用 `pre('save')` 告诉 Mongoose 在执行 `save()` 之前先执行一个方法。

```js
const schema = new mongoose.Schema({ name: String, age: Number })
schema.pre('save', function () {
  // 在 save 中间件中，this 是正在保存的文档。
  console.log('Save', this.name)
})
const Person = mongoose.model('Person', schema)

const doc = new Person({ name: 'O.O', age: 18 })

// Save O.O
await doc.save()
```

类似地，`post('save')` 告诉 Mongoose 在调用 `save()` 后执行一个方法。例如，您可以将 `pre('save')` 和 `post('save')` 组合起来打印 `save()` 所用的时间。

```js
const schema = new mongoose.Schema({ name: String, age: Number })
schema.pre('save', function () {
  this.$locals.start = Date.now()
})
schema.post('save', function () {
  console.log('保存时间为 ', Date.now() - this.$locals.start, ' ms')
})
const Person = mongoose.model('Person', schema)

const doc = new Person({ name: 'O.O', age: 18 })

// 保存时间为 12 ms
await doc.save()
```

`save()` 中间件是递归的，因此对父文档调用 `save()` 也会触发子文档的 `save()` 中间件。

```js
const friendSchema = new mongoose.Schema({ name: String, age: Number, hobby: String })
friendSchema.pre('save', function () {
  console.log('Save', this.hobby)
})
const schema = new mongoose.Schema({
  name: String,
  age: Number,
  friend: friendSchema
})
const Person = mongoose.model('Person', schema)

const doc = new Person({
  name: 'O.O',
  age: 18,
  friend: {
    name: 'D.O',
    age: 20,
    hobby: 'sing'
  }
})

// Save sing
await doc.save()

doc.friend.hobby = 'dance'
// Save dance
await doc.save()
```
