# TypeScript 基础 — 类型谓词

TypeScript 中的[类型谓词（type predicates）](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)可以帮助您根据条件缩小类型范围。它与 type guards 类似，但在函数上工作。如果函数返回 `true`，则将参数的类型更改为更有用的类型。

让我们从一个基本的例子开始。假设您有一个函数，用于检查某个值是否为 `string` 类型：

```ts
function isString(s) {
  return typeof s === 'string'
}
```

在另一个函数中使用 `isString` 函数：

```ts
function toUpperCase(x: unknown) {
  if (isString(x)) {
    x.toUpperCase() // x 的类型 unknown
  }
}
```

TypeScript 会抛出一个错误。我们可以确定此时 `x` 的类型是 `string`。但是，由于验证封装在函数中，所以 `x` 的类型不会改变（与 type guard 相反）。

让我们显式地告诉 TypeScript，如果 `isString` 的值为 `true`，则形参的类型是一个字符串:

```ts
function isString(s): s is string {
  return typeof s === 'string'
}
```

TypeScript 现在知道我们在 `toUpperCase` 函数中处理的是字符串。

```ts
function toUpperCase(x: unknown) {
  if (isString(x)) {
    x.toUpperCase() // ✅ x是字符串
  }
}
```

## 缩小集合范围

这不仅可以帮助您查找未知类型或多个类型，还可以缩小类型内的集合。我们来做一个掷骰子的程序。每次你掷出一个六，你就赢了。

```ts
function pipsAreValid(pips: number) {
  // 我们检查每个离散值，因为数字也可以是介于 1 和 2 之间的值。
  return (
    pips === 1 ||
    pips === 2 ||
    pips === 3 ||
    pips === 4 ||
    pips === 5 ||
    pips === 6
  )
}

function evalThrow(count: number) {
  if (pipsAreValid(count)) {
    switch (count) {
      case 1:
      case 2:
      case 3:
      case 4:
      case 5:
        console.log('Oh!')
        break
      case 6:
        console.log('赢了!')
        break
      case 7:
        // TypeScript 在这里没有报错，尽管 count 不可能是 7
        console.log('这行不通!')
        break
    }
  }
}
```

该程序一开始看起来不错，但从类型的角度来看有一些问题：`count` 是 `number` 类型。这可以作为输入参数。我们马上验证 `count` 是一个介于 1 和 6 之间的数字。一旦我们验证了这一点，`count` 就不再是任何数字了。它被缩小为六个值的离散集合。

所以从 `switch` 语句开始，我的类型已经错了！为了避免任何进一步的复杂情况，让我们使用**联合类型**将数字集缩小到这六个离散值：

```ts
type Dice = 1 | 2 | 3 | 4 | 5 | 6

function pipsAreValid(pips: number): pips is Dice {
  return (
    pips === 1 ||
    pips === 2 ||
    pips === 3 ||
    pips === 4 ||
    pips === 5 ||
    pips === 6
  )
}

function evalThrow(count: number) {
  if (pipsAreValid(count)) {
    switch (count) {
      case 1:
      case 2:
      case 3:
      case 4:
      case 5:
        console.log('Oh!')
        break
      case 6:
        console.log('赢了!')
        break
      case 7:
        // error：7 不是联合类型的
        // 类型 7 不可与类型 Dice 进行比较
        console.log('这行不通!')
        break
    }
  }
}
```

这对我们来说更为安全。当然，这种“类型转换”可以是任何有助于增强应用程序的东西。即使验证了复杂的对象，也可以将参数缩小到特定的类型，并确保它们与代码的其余部分保持一致。非常有用，尤其是当你依赖很多函数时。
