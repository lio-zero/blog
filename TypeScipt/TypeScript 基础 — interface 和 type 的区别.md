# TypeScript 基础 — interface 和 type 的区别

使用 `interface` 和 `type` 声明是表示给定数据结构的常用方法。

```ts
// 使用接口
interface Point {
  x: number
  y: number
}

// 使用类型别名
type Point = {
  x: number
  y: number
}
```

## 区别

可以多次声明同一接口。它们将合并在一起形成一个接口定义。

```ts
interface Point {
  x: number
}

interface Point {
  y: number
}

const p: Point = {
  x: 1,
  y: 2
}
```

其中 `type` 别名必须是唯一的，并且不允许合并声明，否则编译器将报错。

```js
type Person = {
  firstName: string
}

// Throw error: Duplicate identifier
type Person = {
  lastName: string
}
```

能够合并接口声明是非常有用的，例如，当我们为一个外部库提供类型定义时，它不是完全用 TypeScript 创建的。

在这种情况下，如果缺少一些定义，我们可以通过声明合并来提供它们。

## 建议

- 如果您希望定义一个变量类型，就用 `type`，如果希望能够继承并约束，就用 `interface`。
- 如果你不知道该用哪个，建议使用 `type`
- 如果您是库的作者或为外部库创建类型定义，请使用 `interface`。方便其他人也可以扩展它们。

## 更多资料

[Interfaces vs Types in TypeScript](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript)
