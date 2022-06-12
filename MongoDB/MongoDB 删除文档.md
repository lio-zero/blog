# MongoDB 删除文档

MongoDB 提供了三种删除文档的方法：

- `db.collection.deleteOne()`
- `db.collection.deleteMany()`
- `db.collection.remove()`

## `db.collection.deleteOne()` 方法

`db.collection.deleteOne()` 只删除一个文档，即使有多个文档符合条件。

下面是用于删除单个文档的 `db.collection.deleteOne()` 方法的示例。

首先，让我们运行一个返回多个结果的查询：

```bash
$ db.articles.find({ artistname: { $in: [ "The Kooks", "Gang of Four", "Bastille" ] } })

# { "_id" : ObjectId("5781d7f248ef8c6b3ffb014d"), "artistname" : "The Kooks" }
# { "_id" : ObjectId("5781d7f248ef8c6b3ffb014e"), "artistname" : "Bastille" }
# { "_id" : ObjectId("5781d7f248ef8c6b3ffb014f"), "artistname" : "Gang of Four" }
```

可以看到，我们有三个文档符合条件。

现在，让我们对 `db.collection.deleteOne()` 方法使用完全相同的筛选条件：

```bash
$ db.articles.deleteOne({ artistname: { $in: [ "The Kooks", "Gang of Four", "Bastille" ] } })

# { "acknowledged" : true, "deletedCount" : 1 }
```

因此，即使三个文档符合条件，也只删除了一个文档。

让我们再次运行 `find()` 查询以查看剩余哪些文档：

```bash
$ db.articles.find({ artistname: { $in: [ "The Kooks", "Gang of Four", "Bastille" ] } })

# { "_id" : ObjectId("5781d7f248ef8c6b3ffb014e"), "artistname" : "Bastille" }
# { "_id" : ObjectId("5781d7f248ef8c6b3ffb014f"), "artistname" : "Gang of Four" }
```

## `db.collection.deleteMany()` 方法

`db.collection.deleteMany()` 方法删除所有符合条件的文档。

因此，让我们使用与前面示例完全相同的条件运行 `db.collection.deleteMany()` 方法。记住，这时只有两条记录。让我们看看 `db.collection.deleteMany()` 是否同时删除了以下两项：

```bash
$ db.articles.deleteMany({ artistname: { $in: [ "The Kooks", "Gang of Four", "Bastille" ] } })

# { "acknowledged" : true, "deletedCount" : 2 }
```

现在，剩下的两条记录都被删除了。

## `db.collection.remove()` 方法

`db.collection.remove()` 方法删除单个文档或符合指定条件的所有文档。

在这里，我们删除 `article` 名为 `AC/DC` 的所有文档。

```bash
$ db.articles.remove({ artistname: "AC/DC" })

# WriteResult({ "nRemoved" : 1 })
```

在这种情况下，只有一个 `AC/DC`

### `justOne` 选项

您可以使用 `justOne` 参数将删除操作限制为仅一个文档（就像使用 `db.collection.deleteOne()`）。

首先，让我们运行一个返回多个文档的查询：

```bash
$ db.users.find({ born: { $lt: 1950 } })

# { "_id" : 2, "name" : "Ian Paice", "instrument": "Drums", "born": 1948 }
# { "_id" : 3, "name" : "Roger Glover", "instrument": "Bass", "born": 1945 }
# { "_id" : 5, "name" : "Don Airey", "instrument": "Keyboards", "born": 1948 }
```

现在，我们将使用 `justOne` 选项删除其中一条记录。同样，我们将使用完全相同的筛选条件：

```bash
$ db.users.remove({ born: { $lt: 1950 } }, { justOne: 1 })

# WriteResult({ "nRemoved" : 1 })
```

现在，让我们运行相同的查询以查看剩余的文档：

```bash
$ db.users.find({ born: { $lt: 1950 } })

# { "_id" : 3, "name" : "Roger Glover", "instrument": "Bass", "born": 1945 }
# { "_id" : 5, "name" : "Don Airey", "instrument": "Keyboards", "born": 1948 }
```

### 删除集合中的所有文档

只需省略任何筛选条件，即可删除集合中的所有文档。

让我们删除 `article` 集合中的所有文档：

```bash
$ db.articles.remove({})

# WriteResult({ "nRemoved" : 8 })
```

> **注意**：如果您收到一个错误：`remove needs a query`，请检查您是否忘记包含花括号。
