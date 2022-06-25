# 在 Mongoose 中使用 MongoDB Explain

在 MongoDB 中，[`explain` 命令](https://docs.mongodb.com/manual/reference/method/cursor.explain/)告诉 MongoDB 服务器返回有关其如何执行查询的统计信息，而不是查询结果。[Mongoose 查询](https://mongoosejs.com/docs/queries.html)有一个 `explain()` 方法，用于将查询转换为 `explain()`。

```js
const User = mongoose.model(
  'User',
  new mongoose.Schema({
    name: String,
    age: Number
  })
)

await User.create([
  { name: 'O.O', age: 18 },
  { name: 'D.O', age: 19 },
  { name: 'K.O', age: 28 },
  { name: 'O.K', age: 29 },
  { name: 'LAY', age: 24 }
])

const explain = await User.find({ name: /Y/ })
  .explain()
  .then((res) => res[0])

// 描述 MongoDB 计划如何执行查询
console.log(explain.queryPlanner) // { ... }
// 包含有关 MongoDB 如何执行查询的统计信息
console.log(explain.executionStats) // { ... }
```

## 读取 `queryPlanner` 输出

`queryPlanner` 对象包含有关 MongoDB 决定如何执行查询的更详细信息。例如，下面是来自上述 `explain()` 调用的 `queryPlanner` 对象。

```js
{
  plannerVersion: 1,
  namespace: 'test.users',
  indexFilterSet: false,
  parsedQuery: { name: { '$regex': 'Y' } },
  winningPlan: {
    stage: 'COLLSCAN',
    filter: { name: { '$regex': 'Y' } },
    direction: 'forward'
  },
  rejectedPlans: []
}
```

最重要的信息是 `winningPlan` 属性，它包含有关决定执行查询的 plan MongoDB 的信息。实际上，`winningPlan` 用于检查 MongoDB 是否使用索引进行查询。

查询计划是用于标识与查询匹配的文档的阶段列表。上述计划只有一个阶段 `COLLSCAN`，这意味着 MongoDB 执行了完整的集合扫描来回答查询。集合扫描意味着 MongoDB 搜索 `users` 集合中的每个文档，查看 `name` 是否与给定查询匹配。

当您引入索引时，查询计划会变得更加复杂。例如，假设您在 `name` 上添加一个索引，如下所示。

```js
await User.collection.createIndex({ name: 1 })

const explain = await User.find({ name: 'O.O' })
  .explain()
  .then((res) => res[0])

explain.queryPlanner
```

`queryPlanner` 输出如下所示：

```js
{
  plannerVersion: 1,
  namespace: 'test.users',
  indexFilterSet: false,
  parsedQuery: { name: { '$eq': 'O.O' } },
  winningPlan: {
    stage: 'FETCH',
    inputStage: {
      stage: 'IXSCAN',
      keyPattern: { name: 1 },
      indexName: 'name_1',
      isMultiKey: false,
      multiKeyPaths: { name: [] },
      isUnique: false,
      isSparse: false,
      isPartial: false,
      indexVersion: 2,
      direction: 'forward',
      indexBounds: { name: [ '["O.O", "O.O"]' ] }
    }
  },
  rejectedPlans: []
}
```

`winningPlan` 属性是一个递归结构：`winningPlan` 指向获胜查询计划中的最后一个阶段，每个阶段都有一个描述前一阶段的 `inputStage` 属性。

在上述计划中，有两个阶段：`IXSCAN` 和 `FETCH`。这意味着第一个 MongoDB 使用 `{ name: 1 }` 索引来确定哪些文档与查询匹配，然后获取各个文档。

## 读取 `executionStats` 输出

`executionStats` 输出比 `queryPlanner` 更复杂：它包括关于每个阶段花费的时间以及每个阶段扫描的文档数量的统计信息。

例如，下面是简单集合扫描的 `executionStats` 输出：

```js
{
  executionSuccess: true,
  nReturned: 1,
  executionTimeMillis: 0,
  totalKeysExamined: 0,
  totalDocsExamined: 5,
  executionStages: {
    stage: 'COLLSCAN',
    filter: { name: { '$regex': 'Y' } },
    nReturned: 1,
    executionTimeMillisEstimate: 0,
    works: 7,
    advanced: 1,
    needTime: 5,
    needYield: 0,
    saveState: 0,
    restoreState: 0,
    isEOF: 1,
    direction: 'forward',
    docsExamined: 5
  },
  allPlansExecution: []
}
```

这里需要注意的重要细节是顶层 `executionTimeMillis` 和 `TotalDocsChecked` 属性。`executionTimeMillis` 是 MongoDB 执行查询所花费的时间，`TotalDocsDemined` 是 MongoDB 回答查询时必须查看的文档数。

请记住，`executionTimeMillis` 不包括网络延迟或[被阻塞的时间](https://thecodebarbarian.com/slow-trains-in-mongodb-and-nodejs)。仅仅因为 `executionTimeMillis` 很小，并不意味着最终用户可以立即看到结果。

当您有一个索引和多个阶段时，`executionStats` 会分解每个阶段的大致执行时间和扫描的文档数。以下是带有索引的查询的 `executionStats`，为了简洁起见，排除了一些不太重要的细节：

```js
{
  executionSuccess: true,
  nReturned: 1,
  executionTimeMillis: 2,
  totalKeysExamined: 1,
  totalDocsExamined: 1,
  executionStages: {
    stage: 'FETCH',
    nReturned: 1,
    executionTimeMillisEstimate: 0,
    // ...
    docsExamined: 1,
    // ...
    inputStage: {
      stage: 'IXSCAN',
      nReturned: 1,
      executionTimeMillisEstimate: 0,
      // ...
    }
  },
  allPlansExecution: []
}
```

上面的 `executionStats` 输出表明有两个阶段：`IXSCAN` 和 `FETCH`。`IXSCAN` 阶段在 0 毫秒内执行并导致一个文档被发送到 `FETCH` 阶段。`FETCH` 阶段检查并返回 1 个文档，这是查询的最终结果。
