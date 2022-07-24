# 在 TypeScript 中使用 unknown 而不是 any

有时，我们会遇到事先不知道类型的情况，即可能是任何类型。在 TS v3 之前，我们会使用 `any` 类型来表示这些类型。但这需要进行一些权衡，比如失去 TypeScript 提供的任何类型安全性。

例如：

```ts
const x: any = {
  a: 'a-value',
  b: 'b-value'
}
```

您可以访问上面对象的属性，即 `x.a` 和 `x.b`，一切都将按预期工作。问题是，如果试图访问 `x.c` 值，TypeScript 不会抛出错误，因为对象 `x` 可以是任何东西。

```ts
const c = x.c

console.log(c) // undefined
```

正如您所看到的，这可能是许多 bug 的来源，因为 TypeScript 在构建期间捕获的常见错误将被允许通过。这是因为当您使用 `any` 类型时，您都会选择不进行类型检查。

## 为什么使用 `unknown`？

`unknown` 类型是在 TypeScript 的第 3 版中引入的，作为 `any` 类型的附带类型，当分配给变量时，`unknown` 意味着变量类型未知。

TypeScript 不允许使用 `unknown` 的变量，除非将该变量强制转换为已知类型或收窄其类型。

举个例子。

```ts
const x: unknown = 1

x * x // Error
```

如果我们试图在不收窄类型的情况下将 `x` 平方，TypeScript 将抛出错误。

为了修复上述错误，我们可以使用类型保护来检查它是否是一个数字，然后再将其平方。

```ts
if (typeof x === 'number') {
  console.log(x * x)
}
```

与最初的示例相同，如果我们将类型更改为 `unknown` 并尝试访问任何属性，TypeScript 将抛出一个错误。

您需要将其强制转换，以便 TypeScript 以允许您使用它。

```ts
const x: unknown = {
  a: 'a-value',
  b: 'b-value'
}

console.log((x as { a: string; b: string }).b)
```

从上面的例子可以看出，`unknown` 类型迫使您通过类型转换或类型收窄来确定 `unknown` 的变量是什么。这会产生一个更好、更安全的程序，因为 TypeScript 随后可以对生成的类型进行类型检查。
