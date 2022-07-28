# TypeScript 基础 — string 和 String 的区别

`string` 和 `String` 是有效的 TypeScript 类型。以下声明有效：

```ts
let foo: String = 'foo'
let bar: string = 'bar'
```

## 区别

`string` 是指 JavaScript 的基本类型，可以使用文本（单引号或双引号）或 `String` 函数（不使用 `new` 关键字）创建。

以下三个声明创建相同的字符串：

```ts
const message = 'hello'
const message = 'hello'
const message = String('hello')
```

我们经常使用 `typeof variable==='string'` 来检查给定变量是否为原始字符串。

另一方面，`String` 是一个基本包装字符串的对象，用于操纵字符串。我们可以从构造函数中创建 `String` 的实例，例如 `new String(...)`：

```ts
const message = new String('hello')
```

为了检查变量是否为 `String` 对象的实例，我们必须使用 `instanceof` 运算符：

```ts
if (variable instanceof String) {
  // ...
}
```

## 很高兴知道

将 `String` 对象分配给基本 `string` 变量：

```ts
let foo: String = 'foo'
let bar: string = 'bar'

foo = bar // OK
```

在编写时，`String` 被声明为 [interface](https://github.com/microsoft/TypeScript/blob/master/src/lib/es5.d.ts#L374)，以便将该 `string` 被视为 `String` 的子类型。因此，分配 `foo = bar` 不会导致任何问题。

但执行相反的赋值将抛出错误：

```ts
// ERROR: Type 'String' is not assignable to type 'string'.
// string 是一个基本类型，但 String 是一个包装器对象。
bar = foo
```

## 建议

根据官方 TypeScript 的[注意事项](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)，建议不要使用 `Number`、`String`、`Boolean`、`Symbol` 或 `Object`。

```ts
// Do NOT
const reverse = (s: String): String => {
  // ...
}

// Do
const reverse = (s: string): string => {
  // ...
}
```
