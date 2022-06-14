# Mongoose 更新文档

Mongoose 有 4 种不同的方式来更新文档。

- `Document.save()`
- `Model.updateOne()`和 `updateMany()`
- `Document.updateOne()`
- `Model.findOneAndUpdate()`

## 使用 save()

下面是使用 `save()` 更新标题的示例。

```js
const schema = new mongoose.Schema({ name: String, title: String })
const ArticleModel = mongoose.model('Article', schema)

const doc = await ArticleModel.create({
  name: 'lio-zero',
  title: 'Mongoose 更新文档'
})

// 通过设置属性并调用 save() 更新文档
doc.title = '使用 save() 方法更新文档'
await doc.save()
```

这个简单的例子有几个细微差别。首先，`save()` 是文档上的一个方法，这意味着您必须有一个要保存的文档。您需要使用 `create()` 或 `find()` 来获取文件。

其次，Mongoose 文档具有更改跟踪功能。当您调用 `doc.save()` 时，Mongoose 知道您设置 `title` 并将您的 `save()` 调用转换为 `updateOne({ $set: { title } })`。尝试在调试模式下运行 Mongoose 以查看 Mongoose 执行的查询。

## 使用 Model.updateOne() 和 Model.updateMany()

使用 `Model.updateOne()` 和 `Model.updateMany()`，您可以更新文档，而无需先从数据库加载它。在下面的示例中，当 `updateOne()` 被调用时，`name = 'lio-zero'` 的文档不在 Node.js 进程的内存中。

```js
// 使用 updateOne() 更新文档
await ArticleModel.updateOne(
  { name: 'lio-zero' },
  {
    title: '使用 updateOne() 方法更新文档'
  }
)

// 加载文档以查看更新的值
const doc = await ArticleModel.findOne()
doc.title // '使用 updateOne() 方法更新文档'
```

如果你想在 Mongoose 中用一个命令更新多个文档，你应该使用 `updateMany()` 方法。

```js
await ArticleModel.create({ name: 'lio' })
await ArticleModel.create({ name: 'lion' })
await ArticleModel.create({ name: 'lio-zero' })

// 在所有文档上设置 title 属性
await ArticleModel.updateMany({}, { title: '使用 updateMany() 方法更新文档' })

// 设置 name 以 o 结尾的文档的 title 属性
await ArticleModel.updateMany(
  { name: /o$/ },
  { $set: { title: '使用 updateMany() 方法更新文档' } }
)
```

`await Model.updateMany()` 返回具有 5 个属性的对象，详细可查阅[文档](https://mongoosejs.com/docs/api.html#model_Model.updateMany)。

`updateMany()` 很相似。这两个方法之间的区别在于，`updateOne()` 将最多更新一个文档，而 `updateMany()` 将更新与过滤器匹配的每个文档。

您应该尽可能使用 `save()` 而不是 `updateOne()` 和 `updateMany()`。但是，`Model.updateOne()` 和 `Model.updateMany()` 有一些优点：

- `updateOne()` 是原子的。如果使用 `find()` 加载文档，它可能会在 `save()` 之前发生更改。
- `updateOne()` 不需要您将文档加载到内存中，如果文档很大，这可能会给您带来更好的性能。

## 使用 Document.updateOne()

`Document.updateOne()` 方法是 `Model.updateOne()` 的语法糖。如果您已经将文档保存在内存中，那么 `doc.updateOne()` 将为您构建一个 `Model.updateOne()` 调用。

```js
// 加载文档
const doc = await ArticleModel.findOne({ name: 'lio-zero' })

// 使用 Document.updateOne() 加载文档等同于 ArticleModel.updateOne({ _id: doc._id }, update)
const update = { title: '使用 findOne() 方法更新文档' }
await doc.updateOne(update)

const updatedDoc = await ArticleModel.findOne({ name: 'lio-zero' })
updatedDoc.title // '使用 findOne() 方法更新文档'
```

通常，`Document.updateOne()` 很少有用。最好使用 `save()` 和 `Model.updateOne()` 用于 `save()` 不够灵活的情况。

## 使用 Model.findOneAndUpdate()

[`Model.findOneAndUpdate()`](https://mongoosejs.com/docs/tutorials/findoneandupdate.html) 方法或其变体 [`Model.findByIdAndUpdate()`](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate) 的行为类似于 `updateOne()`：它们以原子方式更新与第一个参数 `filter` 匹配的第一个文档。与 `updateOne()` 不同的是，它会返回更新的文档。

```js
const doc = await ArticleModel.findOneAndUpdate(
  { name: 'lio-zero' },
  { title: '使用 findOneAndUpdate() 方法更新文档' },
  { returnDocument: 'before' }
)

doc.title // '使用 findOneAndUpdate() 方法更新文档'
```

`returnDocument` 返回更新后的文档。它有两个可能的值：`'before'` 和 `'after'`。默认行为是 `'before'`，这意味着返回文档在应用更新之前的状态。

在 `returnDocument` 出现之前，有两个类似的选项：`returoriginal` 或 `new`。它们都是布尔值，和 `returnDocument` 有着相同的效果，但 mongoose 建议我们使用最新的 `returnDocument`。

## 总结

通常，除非需要原子更新，否则应该使用 `save()` 更新 Mongoose 中的文档。以下是更新文档的所有 4 种方法的主要功能摘要：

|                            | Atomic | 内存中的文档 | 返回更新的文档 | 更改跟踪 |
| -------------------------- | :----: | :----------: | :------------: | :------: |
| `Document.save()`          |   ❌   |      ✅      |       ✅       |    ✅    |
| `Model.updateOne()`        |   ✅   |      ❌      |       ❌       |    ❌    |
| `Model.updateMany()`       |   ❌   |      ❌      |       ❌       |    ❌    |
| `Document.updateOne()`     |   ❌   |      ✅      |       ❌       |    ❌    |
| `Model.findOneAndUpdate()` |   ✅   |      ❌      |       ✅       |    ❌    |
