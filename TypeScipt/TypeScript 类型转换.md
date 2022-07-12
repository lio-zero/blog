# TypeScript 类型转换

在使用类型时，有时需要重写变量的类型，例如库提供了错误的类型。

强制转换是重写类型的过程。

## 使用 `as` 类型转换

转换变量的一种直接方法是使用 `as` 关键字，它将直接更改给定变量的类型。

```ts
// 当不确定变量的数据类型时，我们会使用 unknown 类型。
let x: unknown = 'hello'(x as string).length // 5
```

强制转换实际上并不会更改变量中的数据类型，例如，以下代码的工作方式与预期不符，因为变量 `x` 是一个数字。

```ts
let x: unknown = 4(x as string).length // 打印 undefined，因为数字没有长度
```

TypeScript 仍会尝试检查类型转换，以防止看起来不正确的类型转换。例如，以下内容将引发类型错误，因为 TypeScript 知道，如果不转换数据，将字符串转换为数字是没有意义的:

```ts
;(4 as string).length // Error
```

如果你使用 VS Code 编辑器，它也会提醒我们：

> 类型 "number" 到类型 "string" 的转换可能是错误的，因为两种类型不能充分重叠。如果这是有意的，请先将表达式转换为 "unknown"。ts(2352)

## 使用 `<>` 类型转换

使用 `<>` 与使用 `as` 进行转换的效果相同。

```ts
let x: unknown = 'hello'(<string>x).length
```

这种类型的转换不适用于 TSX，例如在处理 React 文件时。

## 强制转换

要覆盖 TypeScript 在强制转换时可能引发的类型错误，请先强制转换为 `unknown`，然后再转换为目标类型。

```ts
let n: unknown = 4(n as unknown as string).length // n 实际上不是一个数字，所以它将返回 undefined
```
