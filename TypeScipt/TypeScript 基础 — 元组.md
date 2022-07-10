# TypeScript 基础 — 元组

## 类型化数组

**元组（Tuple）**是一个类型化数组，每个索引都有预定义的长度和类型。

元组很好，因为它们允许数组中的每个元素都是已知类型的值。

要定义元组，请指定数组中每个元素的类型：

```ts
// 定义元组
let ourTuple: [number, boolean, string]

// 正确初始化
ourTuple = [6, false, 'I.O']
```

正如你所看到的，我们有一个数字，布尔值和一个字符串。但如果我们试图将它们设置为错误的顺序会发生什么：

```ts
let ourTuple: [number, boolean, string]

// 初始化错误会引发错误
ourTuple = [false, 'I.O', 6]
```

即使我们有一个 `boolean`、`string` 和 `number，但顺序在元组中很重要，并且会抛出一个错误。

## 只读元组

一个好的做法是将元组设置为 `readonly`。

元组仅具有用于初始值的强定义类型：

```ts
let ourTuple: [number, boolean, string] = [6, false, 'I.O']

// 索引 3 的元组中没有类型安全性
ourTuple.push('O.O')
console.log(ourTuple) // [6, false, 'I.O', 'O.O']
```

您可以看到，新的 valueTuple 仅具有针对初始值的强定义类型：

```ts
// 定义只读元组
const ourReadonlyTuple: readonly [number, boolean, string] = [6, true, 'I.O']

// 抛出错误，因为它是只读的。
ourReadonlyTuple.push('O.O') // type error
```

## 命名元组

**命名元组**允许我们为每个索引处的值提供上下文。

```ts
const graph: [x: number, y: number] = [55.2, 41.3]
```

**命名元组**为索引值表示的内容提供了更多上下文。

## 解构元组

由于元组是数组，我们也可以对它们进行分解。

```ts
const graph: [number, number] = [55.2, 41.3]
const [x, y] = graph
```
