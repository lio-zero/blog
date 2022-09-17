# IndexedDB

> MDN：[IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) 是一种底层异步 API，浏览器内置的数据库，用于在客户端存储大量的结构化数据（也包括文件/[二进制大对象 blob](https://github.com/lio-zero/blog/blob/main/JavaScript/Blob%20%E5%AF%B9%E8%B1%A1.md)）。

IndexedDB 是多年来引入浏览器的存储功能之一。它是一种键/值存储（noSQL 数据库，非关系型的数据库），被认为是在浏览器中存储数据的最终解决方案。

它是一个异步 API，这意味着执行昂贵的操作不会阻塞 UI 线程为用户提供草率的体验。它可以存储无限量的数据，但一旦超过某个阈值，用户会被提示给网站更高的限制。

所有现代浏览器都支持 [IndexedDB](http://caniuse.com/#feat=indexeddb)。

![IndexedDB 支持情况](https://upload-images.jianshu.io/upload_images/18281896-7e4e1f68409d6e08.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它支持事务、版本控制并提供良好的性能。

在浏览器内部，我们还可以使用：

- [Cookie](https://github.com/lio-zero/blog/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTP%20Cookie.md) — 可以承载非常少量的字符串（容量 ~4kb）
- Web Storage（或 DOM Storage），通常用于标识 `localStorage` 和 `sessionStorage` 这两个键/值存储的术语。`sessionStorage` 不保留数据，会话结束时清除数据，`localStorage` 保留跨会话的数据，不主动清除，永远保留数据。

`localStorage` 和 `sessionStorage` 的缺点是存储大小有限且不一致，浏览器实现为每个网站提供 2MB 到 10MB 的存储空间。

过去我们也有 Web SQL，它是 SQLite 的包装器，但现在一些现代浏览器**已弃用**和不支持它，它从未成为公认的标准，因此不应该使用它。

虽然技术上可以为每个网站创建多个数据库，但通常只**创建一个数据库**，并且在该数据库中可以创建**多个对象库（Object Store）**，可以理解为一张张表。

数据库对域来说是私有的，因此任何其他网站都无法访问另一个网站 IndexedDB 存储。

存储包含许多具有唯一键的项，该键表示可以识别对象的方式。

您可以通过执行添加、编辑和删除操作，并迭代它们所包含的项，使用事务更改这些存储。

## IndexedDB 特点

总结一下上面介绍的 IndexedDB 特点：

- **储存空间大** — 相比于其他本地存储，IndexedDB 存储空间几乎无限量，但一旦超过某个阈值，用户会被提示给网站更高的限制。
- **支持二进制储存** — IndexedDB 不仅可以储存字符串，还可以储存二进制数据（ArrayBuffer 对象和 Blob 对象）。
- **同源限制** — 数据库对域来说是私有的，因此任何其他网站都无法访问另一个网站 IndexedDB 存储。
- **异步** — 使用 IndexedDB 执行的操作是异步执行的，以免阻塞应用。
- **支持事务** — 执行一系列操作，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况。
- **键值对储存** — IndexedDB 内部采用**对象库（object store）**存放数据。所有类型的数据都可以直接存入，包括**字符串、数字、对象、数组、日期**。对象库中，数据以"键值对"的形式保存，每一个数据记录都有对应的主键，主键是唯一的，不能有重复，否则会抛出一个错误。

## IndexedDB 库

IndexedDB API 功能强大，但对于简单的情况可能看起来太复杂如果你更喜欢一个简单的 API，请尝试：

- [localForage](https://localforage.github.io/localForage/) — 一个简单的 Polyfill，提供了简单的客户端数据存储的值语法。它在后台使用 IndexedDB，并且在不支持 IndexedDB 的浏览器中回退到 WebSQL 或 localStorage。
- [Dexie.js](http://www.dexie.org/) — IndexedDB 的包装，通过简单的语法，可以进行代码开发。
- [pouchdb](https://pouchdb.com/) — 使用 IndexedDB 在浏览器中实现 CouchDB 的客户端。
- [idb](https://www.npmjs.com/package/idb) — 一个微小的（〜115k）库，大多数 API 类似，但做了一些细微的改进，让数据库的空间有了很大的提升。
- [idb-keyval](https://www.npmjs.com/package/idb-keyval) — 使用 IndexedDB 实现的超级简单且小巧的键（~600B）基于 Promise 对值存储。
- [JsStore](https://jsstore.net/) — 一个 SQL 语法的 IndexedDB 包装器。
- [lovefield](https://github.com/google/lovefield) — Lovefield Web App 的关系数据库，使用安全的 JavaScript 编写环境，可以用于不同的浏览中运行，类似 SQL 的 API，速度快、方便易用。
- [MiniMongo](https://github.com/mWater/minimongo) — 由 localstorage 支持的客户端内存中的 mongodb，通过 http 进行服务器同步。MeteorJS 使用 MiniMongo。

这些库使 IndexedDB 对开发者来说更加友好。

下面我将使用 [idb](https://github.com/jakearchibald/idb) 库提供使用示例，如果您想了解原生 IndexedDB API 操作可以阅读阮一峰老师的[浏览器数据库 IndexedDB 入门教程](http://www.ruanyifeng.com/blog/2018/07/indexeddb.html)，其中可以了解一些未在本文中涉及的基本概念（很重要）。

## 创建 IndexedDB 数据库

最简单的方法是使用 CDN unpkg：

```html
<script type="module">
  import { openDB, deleteDB } from 'https://unpkg.com/idb?module'
</script>
```

在使用 IndexedDB API 之前，请始终确保在浏览器中检查支持，即使它广泛可用，但您永远不知道用户正在使用的是哪种浏览器：

```js
;(() => {
  'use strict'

  if (!('indexedDB' in window)) {
    console.warn('IndexedDB not supported')
    return
  }

  // ...IndexedDB code
})()
```

接着，使用 `openDB()` 创建数据库：

```js
;(async () => {
  // ...

  const dbName = 'mydbname'
  const storeName = 'store1'
  const version = 1 // 版本从 1 开始（默认）

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
    }
  })
})()
```

前两个参数是数据库名称和版本号。第三个参数（可选）是一个对象，它包含一个仅在版本号高于当前安装的数据库版本时才调用的 `upgrade` 函数。在函数体中，您可以升级数据库的结构（对象库和索引）。

## 将数据添加到对象库中

### 创建对象库时添加数据，并对其进行初始化

可以使用对象库的 `put` 方法，但首先我们需要一个对它的引用，可以从 `db.createObjectStore()` 中获取。

使用 `put` 时，值是第一个参数，键是第二个参数。这是因为如果在创建对象库时指定 `keyPath`，则不需要在每个 `put()` 请求中输入键名，只需写入值即可。

这将在我们创建 `store0` 后立即填充它：

```js
;(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
      store.put('Hello world!', 'Hello')
    }
  })
})()
```

### 使用事务在已创建对象库时添加数据

要在以后添加项目，您需要创建一个读/写事务，以确保数据库的完整性（如果操作失败，事务中的所有操作都将回滚，状态将回到已知状态）。

为此，使用对调用 `openDB` 时获得的 `dbPromise` 对象的引用，然后运行：

```js
;(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(/* ... */)

  const tx = db.transaction(storeName, 'readwrite')
  const store = await tx.objectStore(storeName)

  const val = 'hey!'
  const key = 'Hello again'
  const value = await store.put(val, key)
  await tx.done
})()
```

## 从对象库获取数据

使用 `get()` 方法从对象库中获取一项：

```js
const key = 'Hello again'
const item = await db.transaction(storeName).objectStore(storeName).get(key)
```

使用 `getAllKeys` 方法获取存储的所有键：

```js
const items = await db
  .transaction(storeName)
  .objectStore(storeName)
  .getAllKeys()
```

使用 `getAll` 方法获取所有存储的值：

```js
const items = await db.transaction(storeName).objectStore(storeName).getAll()
```

## 从 IndexedDB 中删除数据

删除数据库、对象库和数据。

### 删除数据库

使用 `deleteDB()` 方法删除整个 IndexedDB 数据库：

```js
const dbName = 'mydbname'
await deleteDB(dbName)
```

### 删除对象库

使用事务删除对象库中的数据：

```js
;(async () => {
  //...

  const dbName = 'mydbname'
  const storeName = 'store1'
  const version = 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
    }
  })

  const tx = await db.transaction(storeName, 'readwrite')
  const store = await tx.objectStore(storeName)

  const key = 'Hello again'
  await store.delete(key)
  await tx.done
})()
```

### 从数据库的早期版本迁移

```js
const name = 'mydbname'
const version = 1

openDB(name, version, {
  upgrade(db, oldVersion, newVersion, transaction) {
    console.log(oldVersion)
  }
})
```

在这个回调中，您可以检查用户正在更新哪个版本，并相应地执行一些操作。

可以使用以下语法从以前的数据库版本执行迁移：

```js
;(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      switch (oldVersion) {
        case 0: // 之前未创建数据库
          // 版本 1 中引入的对象库
          db.createObjectStore('store1')
        case 1:
          // 版本 2 中的新对象库
          db.createObjectStore('store2', { keyPath: 'name' })
      }
      db.createObjectStore(storeName)
    }
  })
})()
```

## 唯一键

如案例 1 所示，`createObjectStore()` 接受指示数据库索引键的第二个参数。当您存储对象时，这非常有用：`put()` 调用不需要第二个参数，但可以只获取值（对象），键将映射到具有该名称的对象属性。

索引为您提供了一种稍后通过该特定键检索值的方法，并且它必须是唯一的（每个项必须具有不同的键）

可以将键设置为自动递增，因此不需要在客户端代码上跟踪它：

```js
db.createObjectStore('notes', { autoIncrement: true })
```

如果值尚未包含唯一键（例如，如果收集的电子邮件地址没有关联名称），请使用自动递增。

## 检查对象库是否存在

您可以通过调用 `objectStoreNames()` 方法来检查对象库是否已存在：

```js
const storeName = 'store1'

if (!db.objectStoreNames.contains(storeName)) {
  db.createObjectStore(storeName)
}
```

## 最后

以上内容仅仅是一些基础知识和 API 的基本用法。IndexedDB 还有很多高级的内容可以讨论。

但本文到此就结束了，我希望这是一个良好的开端。

更多内容往下看。

## 更多资料

- 原生 IndexedDB API 操作可以看看阮一峰老师的[浏览器数据库 IndexedDB 入门教程](http://www.ruanyifeng.com/blog/2018/07/indexeddb.html)
- [How to Store Unlimited\* Data in the Browser with IndexedDB](https://www.sitepoint.com/indexeddb-store-unlimited-data/)
- [IndexedDB](https://zh.javascript.info/indexeddb)
