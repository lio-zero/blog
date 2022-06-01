# TypeScript 基础 — Null 和 Undefined

`null` 和 `undefined` 都有各自的类型名称。这些类型本身没有用处，因为我们只能将 `null` 和 `undefined` 赋值给定义为 `null` 或 `undefined` 类型的变量。

```ts
let u: undefined = undefined
u = 'string' // compile error

let n: null = null
n = 43 //compile error
```

默认情况下，`null` 和 `undefined` 是所有类型的子类型。 就是说可以把 `null` 和 `undefined` 赋值给 `number` 类型的变量。

```ts
let value: string | undefined | null = null
value = 'hello'
value = undefined
```

> **注意**：默认情况下，将禁用 `null` 和 `undefined` 处理，我们可以通过在 `tsconfig.json` 文件将 `strictNullChecks` 设置为 `true` 来启用。当启用 `strictNullChecks` 时，本文的示例才能正常运行。

## 可选链

可选链接是一种 JavaScript 功能，它与 TypeScript 的空处理配合得很好。它允许使用紧凑的语法访问对象上的属性，这些属性可能存在，也可能不存在。它可以和 **`?.`（可选链）**运算符访在问属性时一起使用。

```ts
interface User {
  name: string
  address?: {
    street: string
  }
}

function printStreet(user: User) {
  const streetInfo = user.address?.street
  if (streetInfo === undefined) {
    console.log('No street info')
  } else {
    console.log(streetInfo)
  }
}

let user: User = {
  name: 'O.O'
}

printStreet(user) // 'No street info'
```

## 空值合并

空值合并是另一个 JavaScript 特性，它也可以很好地与 TypeScript 的空处理配合使用。

它允许编写在处理 `null` 或 `undefined` 时有回退的表达式。当表达式中可能出现其他 falsy 值，但这些值仍然有效时，这非常有用。

它可以与表达式中的 **`??`（空值合并运算符）**一起使用，类似于使用 **`&&`（与运算符）**。

```ts
function printName(name: string | null | undefined) {
  console.log(`${name ?? '无名氏'}`)
}

printName(null) // '无名氏'
printName('O.O') // 'O.O'
```

## 空断言

TypeScript 的推理系统并不完美，有时候忽略一个值为 `null` 或 `undefined` 的可能性是有意义的。一个简单的方法就是使用类型转换，但是 TypeScript 也提供了 `!` 运算符作为方便的快捷方式。

```ts
function getValue(): string | undefined {
  return 'hello'
}

let value = getValue()

value.length // value 可能为 undefined
// 使用 ! 运算符
value!.length // 5
```

就像类型转化一样，这可能是不安全的，应该小心使用。

## 数组边界处理

即使启用了 `strictNullChecks`，默认情况下 TypeScript 也会假定数组访问永远不会返回 `undefined`（除非 `undefined` 是数组类型的一部分）。

```ts
let array: number[] = [1, 2, 3]
let value = array[0] // value 的类型为 number
```

我们可以配置 `noUncheckedIndexedAccess` 来更改此行为。

```ts
let array: number[] = [1, 2, 3]
let value = array[0] // value 的类型为 number | undefined
```
