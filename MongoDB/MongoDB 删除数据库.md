# MongoDB 删除数据库

在 MongoDB 中，您可以通过切换到该数据库并运行 `db.dropDatabase()` 方法或 `dropDatabase` 命令来删除数据库。

## `db.dropDatabase()` 方法

首先，让我们检查一下我们的数据库列表：

```bash
$ show databases
# or
$ show dbs
# local  0.000GB
# user   0.000GB
# test   0.005GB
```

切换到 `user` 数据库，使用 `db.dropDatabase()` 方法删除当前数据库。

```bash
$ use user
$ db.dropDatabase()
# { "dropped" : "user", "ok" : 1 }
```

再次查看数据库列表：

```bash
$ show databases
# local  0.000GB
# test   0.005GB
```

可以看到，`user` 数据库已删除

## `dropDatabase` 命令

您也可以使用 `dropDatabase` 命令来做同样的事情。

```bash
$ use test
$ db.runCommand( { dropDatabase: 1 } )
# { "dropped" : "test", "ok" : 1 }
```

我们的数据库列表再次减少：

```bash
$ show databases
# local  0.000GB
```
