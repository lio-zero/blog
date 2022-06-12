# MongoDB 删除集合

在 MongoDB 中，您可以使用 `db.collection.drop()` 方法从数据库中删除集合。如果集合存在，它将返回 `true`，如果它不存在，它将返回 `false`。

## 删除一个存在的集合

在这里，我们将使用 `db.collection.drop()` 删除存在的集合。

首先，让我们快速检查一下我们的 `yb` 数据库有哪些集合：

```bash
$ show collections

# users
# articles
# subscribes
```

我们可以看到，该数据库有三个集合，我们选择删除 `articles` 集合。

```bash
$ db.article.drop()
# true
```

成功后，将返回 `true`。我们可以再快速查看一下我们现在拥有哪些集合：

```bash
$ show collections

# users
# subscribes
```

请注意，`db.collection.drop()` 方法不接受任何参数。只需按照上面的说明运行它。

## 尝试删除不存在的集合

根据上面操作，我们没有了 `articles` 集合，让我们再次尝试删除它，看看我们得到了什么消息：

```bash
$ db.articles.drop()
# false
```

它返回了 `false`，因为集合不存在。
