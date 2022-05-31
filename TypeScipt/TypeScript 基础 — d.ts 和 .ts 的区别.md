# TypeScript 基础 — *.d.ts 和 *.ts 的区别

## 区别

`.ts` 是标准的 TypeScript 文件。其内容将被编译为 JavaScript。

`*.d.ts` 是允许在 TypeScript 中使用现有 JavaScript 代码的类型定义文件。

例如，我们有一个简单的 JavaScript 函数，用于计算两个数字的总和：

```ts
// math.js
const sum = (a, b) => a + b

export { sum }
```

TypeScript 没有关于函数的任何信息，包括名称、参数类型。为了在 TypeScript 文件中使用该函数，我们在 `d.ts` 文件中提供其定义：

```ts
// math.d.ts
declare function sum(a: number, b: number): number
```

现在，我们可以在 TypeScript 中使用该函数，而不会出现任何编译错误。

`d.ts` 文件不包含任何实现，并且根本不编译为 JavaScript。

## 很高兴知道

TypeScript 提供了一个名为 `allowJs` 的选项，允许在 TypeScript 中使用普通 JavaScript 代码。

```ts
// tsconfig.json
{
  "compilerOptions": {
    "allowJs": true,
    ...
  }
}
```

当我们想要将现有的代码库从普通 JavaScript 迁移到 TypeScript 时，它非常有用。因此，我们可以将每个 JavaScript 文件逐个转换为 TypeScript，而不必完全转换整个代码。

> 有关更多信息，请参阅 [Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html)。
