# TypeScript 基础 — 联合类型

当一个值可以是多个类型时，使用**联合类型。**

例如，当一个属性是 `string` 或 `number` 时。

## 联合（`|`，OR）

使用 `|` 表示参数是 `string` 或 `number` 类型：

```ts
function isNumber(arg: string | number): boolean {
  return typeof arg === 'number'
}

isNumber(1) // true
isNumber('cat') // false
isNumber(undefined) // type error
```

我们可以在任何可以使用其他类型的情况下使用联合类型。例如，使用类型别名：

```ts
type Code = string | number

let x: Code = 1000
let y: Code = 2000
let nowhere: Code = null // type error
```

`interface` 类似，例如：

```ts
interface User {
  message: string | number
}
```

## 联合类型错误

在使用联合类型以避免类型错误时，您需要知道自己的类型：

```ts
function printStatusCode(code: string | number) {
  console.log(code.toUpperCase()) // type error
}
```

在我们的示例中，调用 `toUpperCase()` 作为 `string` 方法时遇到了一个问题，`number` 无权访问它。

这里，你可以利用 `typeof` 收窄类型：

```ts
function printStatusCode(code: string | number) {
  if (typeof code === 'string') {
    console.log(code.toUpperCase())
  }
}
```

TypeScript 不允许我们混合使用联合类型。例如，`string | number` 不能分配给普通 `number`。

```ts
function getCode(): string | number {
  return 44106
}

let foo: string | number = getCode()
foo // 44106

let baz: number = getCode() // type error
```

我们可以看到函数返回 44106，这是一个数字。但我们明确地告诉编译器，该函数返回一个 `string | number`。编译器不会试图找到比我们提供的类型更好的类型。它只检查提供的类型 `string | number`。

我们要做的是将它包含在指定的联合类型中。

```ts
function getCode(): number {
  return 44106
}
const foo: string | number = getCode()
baz // 44106
```

## 文字联合类型

我们已经看到了 `number` 和 `string` 之类的基本类型。每个具体的数字和每个具体的字符串也是一种类型。例如，类型变量 1 只能保存数字 1。它无法容纳 2；这是一个编译时类型错误。

```ts
let one: 1 = 1
one // 1

let one: 1 = 2 // type error

let one: 1 = 'one' // type error
```

这些被称为**文字类型**。例如，上面的类型 1 是文字数字类型。我们可以将文字类型与联合结合起来，只允许某些值。

```ts
let oneOrTwo: 1 | 2 = 1
oneOrTwo // 1

let oneOrTwo: 1 | 2 = 2
oneOrTwo // 2

let oneOrTwo: 1 | 2 = 3 // type error
```

起初，这似乎是一个奇怪的特性，大多数静态类型的编程语言都不支持它。然而，它在实践中非常有用，并且经常用于实用程序的 TypeScript 代码中。

例如，我们可能有一个排序函数。它可以按升序（`'asc'`）或降序（`'desc'`）排序。

```ts
function sort(numbers: number[], direction: 'asc' | 'desc'): number[] {
  const sortable = numbers.slice() // 克隆数组
  sortable.sort()
  if (direction === 'desc') {
    sortable.reverse()
  }
  return sortable
}

sort([2, 1, 3], 'asc') //  [1, 2, 3]
sort([2, 1, 3], 'desc') // [3, 2, 1]
sort([2, 1, 3], 'something else') // type error
```

理论上，我们可以在上面使用布尔值。但是看到 `'asc'` 或 `'desc'` 比 `true` 或 `false` 更具描述性。

当我们有两个以上的可能值时，文字类型真的很出色。例如，假设我们正在编写一个数据库库。

```ts
type DatabaseType = 'mysql' | 'postgres' | 'oracle'
let dbType: DatabaseType = 'mysql'
dbType // 'mysql'

let dbType: DatabaseType = 'msaccess' // type error
```

有了这个类型约束，如果用户试图在不受支持的数据库中使用我们的库，就会立即被告知。它还会告诉他们数据库名称是否有输入错误。他们甚至不必运行应用程序就能看到故障。它根本无法运行，因为它包含类型错误。

`true` 和 `false` 也是文字类型。`true` 类型的变量永远不能为 `false`。

```ts
let t: true = true
t // true

let t: true = false // type error
```

另一种看待布尔类型的方式是 `true | false`，这是一种比 `true` 更广泛的类型。这意味着我们不能将布尔值赋给 `true` 类型的变量，因为布尔值有时可能是 `false`。

通过类比，我们不能将 `number | string` 分配给 `number` 类型的变量。

```ts
function myBoolean(): boolean {
  return true
}
let t: true = myBoolean() // type error
```
