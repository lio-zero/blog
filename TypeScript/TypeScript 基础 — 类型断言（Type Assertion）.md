# TypeScript 基础 — 类型断言（Type Assertion）

类型断言可以用来手动指定一个值具体的类型，即允许变量从一种类型更改为另一种类型。

当您比 TS 更了解某个值的类型，并且需要指定更具体的类型时，我们可以使用**类型断言**。

类型断言有两种形式。

## "尖括号" 语法：<类型>值

```ts
let someValue: any = 'this is a string'
let strLength: number = (<string>someValue).length
```

## as 语法：值 as 类型

```ts
let someValue: any = 'this is a string'
let strLength: number = (someValue as string).length
```

> **注意**：这两种形式是等价的。但当你在 TypeScript 里使用 JSX 时，只有 `as` 语法断言是被允许的。所以，这里推荐使用 `as` 来进行类型断言。

## 示例

通常情况下，我们使用 `document.querySelector()` 方法返回值类型为 `Element`。

假设，我们明确 `.image` 是一个图片元素，我们可以将其类型明确指定为 `HTMLImageElement`：

```ts
let img = document.querySelector('.image') as HTMLImageElement
```

> **小技巧**：通过 `console.dir()` 打印 DOM 对象， 我们可以查看该元素的类型。
