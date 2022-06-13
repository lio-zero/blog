# Mongoose 数组

[Mongoose 的 `Array` 类](https://mongoosejs.com/docs/schematypes.html#arrays)使用额外的 Mongoose 功能扩展了普通的 JavaScript 数组。

例如，假设您有一个带有 `tags` 数组的 `BlogPost` 模型骨架：

```js
const blogPostSchema = new mongoose.Schema({
  title: String,
  tags: [String]
})
```

当您创建一个新 `BlogPost` 文档时，`tags` 属性是普通 JavaScript 数组类的一个实例。但它也有一些特殊的性质。

```js
const blogPostSchema = Schema(
  {
    title: String,
    tags: [String]
  },
  { versionKey: false }
)
const BlogPost = mongoose.model('BlogPost', blogPostSchema)

const doc = new BlogPost({
  title: 'Intro to JavaScript',
  tags: ['programming']
})

Array.isArray(doc.tags) // true
doc.tags.isMongooseArray // true
```

例如，Mongoose 截取 `push()` 对 `tags` 数组的调用，当你 `save()` 文档时，它足够聪明地使用 `$push` 更新文档。

```js
mongoose.set('debug', true)

doc.tags.push('web development')
// 由于 debug 模式，将打印:
// Mongoose: blogposts.updateOne({ _id: ObjectId(...) }, { '$push': { tags: { '$each': [ 'web development' ] } } }, { session: null })
await doc.save()
```

## 文档数组

`tags` 示例是一个基元数组。Mongoose 还支持子文档数组。下面是如何定义 `members` 数组的方法，每个数组都有 `firstName` 和 `lastName` 属性。

```js
const groupSchema = new mongoose.Schema({
  name: String,
  members: [{ firstName: String, lastName: String }]
})
```

`doc.members` 是一个普通 JavaScript 数组的实例，因此它具有所有常用函数，例如 `slice()` 和 `filter()`。但它也有一些特定于 Mongoose 的功能。

```js
const groupSchema = new mongoose.Schema({
  name: String,
  members: [{ firstName: String, lastName: String }]
})
const Group = mongoose.model('Group', groupSchema)

const doc = new Group({
  title: 'Jedi Order',
  members: [{ firstName: 'Luke', lastName: 'Skywalker' }]
})

Array.isArray(doc.members) // true
doc.members.isMongooseArray // true
doc.members.isMongooseDocumentArray // true
```

例如，如果您设置第 0 个成员的 `firstName`，Mongoose 将在调用 `save()` 时将其转换为 `member.0.firstName` 上的集合。

```js
const groupSchema = Schema(
  {
    name: String,
    members: [{ firstName: String, lastName: String }]
  },
  { versionKey: false }
)
const Group = mongoose.model('Group', groupSchema)

const doc = new Group({
  title: 'Jedi Order',
  members: [{ firstName: 'Luke', lastName: 'Skywalker' }]
})
await doc.save()

mongoose.set('debug', true)

doc.members[0].firstName = 'Anakin'
// Mongoose: groups.updateOne({ _id: ObjectId("...") }, { '$set': { 'members.0.firstName': 'Anakin' } }, { session: null })
await doc.save()
```

## 设置数组索引时的注意事项

Mongoose 在直接设置数组索引方面存在一个已知问题。例如，如果您设置了 `doc.tags[0]`，Mongoose 更改跟踪将不会获取该更改。

```js
const blogPostSchema = Schema(
  {
    title: String,
    tags: [String]
  },
  { versionKey: false }
)
const BlogPost = mongoose.model('BlogPost', blogPostSchema)

const doc = new BlogPost({
  title: 'Intro to JavaScript',
  tags: ['programming']
})
await doc.save()

// 此更改不会在数据库中结束！
doc.tags[0] = 'JavaScript'
await doc.save()

const fromDb = await BlogPost.findOne({ _id: doc._id })
console.log(fromDb.tags) // ['programming']
```

为了解决这个问题，您需要使用 [`markModified()`方法](https://mongoosejs.com/docs/api/document.html#document_Document-markModified)或在数组元素上显式调用 [`MongooseArray.set()`](https://mongoosejs.com/docs/api/array.html#mongoosearray_MongooseArray-set) 来通知 Mongoose 的更改跟踪，如下所示。

```js
// 这种改变是有效的。set() 是 Mongoose 数组上触发更改跟踪的一种特殊方法。
doc.tags.set(0, 'JavaScript')
await doc.save()

const fromDb = await BlogPost.findOne({ _id: doc._id })
console.log(fromDb.tags) // ['JavaScript']
```
