# Mongoose Populate

在 Mongoose 中，[Populate](https://mongoosejs.com/docs/populate.html) 允许您引用其他集合中的文档。其类似于 [SQL 中的左外部连接](https://www.dofactory.com/sql/left-outer-join)，但区别在于 Populate 发生在 Node.js 应用程序中，而不是在数据库服务器上。Mongoose 在引擎下执行单独的查询以加载引用的文档。

## Populate 基础

假设你有两个 [Mongoose 模型](https://mongoosejs.com/docs/models.html)：`Movie` 和 `Person`。`Movie` 文档有一个 `director` 和一系列 `actors`。

```js
const Person = mongoose.model(
  'Person',
  new mongoose.Schema({
    name: String
  })
)

// ref 告诉 Mongoose Populate 要查询的模型
const Movie = mongoose.model(
  'Movie',
  new mongoose.Schema({
    title: String,
    director: {
      type: mongoose.ObjectId,
      ref: 'Person'
    },
    actors: [
      {
        type: mongoose.ObjectId,
        ref: 'Person'
      }
    ]
  })
)
```

Mongoose 查询有一个 [`populate()` 方法](https://mongoosejs.com/docs/api/query.html#query_Query-populate)，允许您在一行中加载电影及其相应的 `director` 和 `actors`：

```js
const people = await Person.create([
  { name: 'James Cameron' },
  { name: 'Arnold Schwarzenegger' },
  { name: 'Linda Hamilton' }
])

await Movie.create({
  title: 'Terminator 2',
  director: people[0]._id,
  actors: [people[1]._id, people[2]._id]
})

// 只加载电影导演
let movie = await Movie.findOne().populate('director')
console.log(movie.director.name) // 'James Cameron'
console.log(movie.actors[0].name) // undefined

// 加载导演和演员
movie = await Movie.findOne().populate(['director', 'actors'])

console.log(movie.director.name) // 'James Cameron'
console.log(movie.actors[0].name) // 'Arnold Schwarzenegger'
console.log(movie.actors[1].name) // 'Linda Hamilton'
```

> **注意**：Mongoose 文档上也有一个 [`populate()` 方法](https://mongoosejs.com/docs/api/document.html#document_Document-populate)，详细可以查看文档。这里需要注意的是，6.x 已移除文档上 [`document.execPopulate()`](https://mongoosejs.com/docs/migrating_to_6.html#removed-execpopulate)。`populate` 现在返回一个 Promise，其不再是可链接。

所以，以下方法将不在适用：

```js
movie = await Movie.findOne().populate('director').populate('actors')
```

您应将旧的链接写法进行替换：

```js
await doc.populate('path1').populate('path2').execPopulate()
// 替换为
await doc.populate(['path1', 'path2'])

await doc
  .populate('path1', 'select1')
  .populate('path2', 'select2')
  .execPopulate()
// 替换为
await doc.populate([
  { path: 'path1', select: 'select1' },
  { path: 'path2', select: 'select2' }
])
```

## 其他案例

如果您正在 `populate` 单个文档，而引用的文档不存在，Mongoose 会将 populate 的属性设置为 `null`。

```js
await Person.deleteOne({ name: 'James Cameron' })

const movie = await Movie.findOne().populate('director')
console.log(movie.director) // null
```

如果您正在 `populate` 一个数组，而其中一个引用的文档不存在，Mongoose 将在默认情况下从数组中过滤该值，并返回一个较短的数组。您可以使用 `retainNullValues` 选项覆盖此选项。

```js
await Person.deleteOne({ name: 'Arnold Schwarzenegger' })

let movie = await Movie.findOne().populate('actors')
console.log(movie.actors.length) // 1
console.log(movie.actors[0].name) // 'Linda Hamilton'

// 设置 retainNullValues 选项，为数组中缺少的文档插入 null
movie = await Movie.findOne().populate({
  path: 'actors',
  options: { retainNullValues: true }
})

console.log(movie.actors.length) // 2
console.log(movie.actors[0]) // null
console.log(movie.actors[1].name) // 'Linda Hamilton'
```
