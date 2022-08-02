# JavaScript 和 TypeScript 中的 void

如果您使用过强类型语言，你可能熟悉 `void` 的概念：一种类型告诉你函数和方法在调用时不返回任何东西。

`void` 作为操作符存在于 JavaScript 中，作为基本类型存在于 TypeScript 中。在这两个世界里，`void` 的工作方式和大多数人习惯的略有不同。

## JavaScript 中的 `void`

JavaScript 中的 `void` 是一个运算符，用于计算它旁边的表达式。无论对哪个表达式求值，`void` 总是返回 `undefined`。

```js
let i = void 2
i === undefined // true
```

为什么我们需要这样的东西？

首先，在早期，人们能够覆盖 `undefined` 并赋予它一个实际值。而 `void` 总是返回**真正的意义上 `undefined`**。

其次，这是调用立即执行函数的好方法：

```js
void (function () {
  console.log('Why')
})()
```

所有这些都不会污染全局命名空间：

```js
void (function fn(i) {
  if (i > 0) {
    console.log(i--)
    fn(i)
  }
})(3)

console.log(typeof fn) // undefined
```

由于 `void` 总是返回 `undefined`，并且 `void` 总是对它旁边的表达式求值，因此有一种非常简洁的方法可以从函数返回，而不返回值，但仍然调用回调函数，例如：

```js
// 返回未定义的内容会导致应用程序崩溃
function middleware(nextCallback) {
  if (condition()) {
    return void nextCallback()
  }
}
```

下面是 `void` 的一个很好的用例：当您的函数总是返回 `undefined` 时，您可以确保总是这样，它对于应用程序来说很安全。

```js
button.onclick = () => void doSomething()
```

## TypeScript 中的 `void`

TypeScript 中的 `void` 是 `undefined` 的子类型。JavaScript 中的函数总是返回一些东西。要么是一个值，要么是 `undefined`：

```ts
function foo(i) {
  console.log(i)
}

foo() // undefined
```

由于没有返回值的函数总是返回 `undefined`，并且 `void` 在 JavaScript 中总是返回 `undefined`，因此 TypeScript 中的 `void` 是告诉开发人员此函数是返回 `undefined` 的合适类型：

```ts
declare function foo(i: number): void
```

作为类型的 `void` 也可以用于形参和所有其他声明。唯一可以传递的值是 `undefined`:

```ts
declare function foo(x: void): void {}

foo() // ✅
foo(undefined) // ✅
foo(void 2) // ✅
```

所以 `void` 和 `undefined` 基本上是一样的。但有一个区别很重要：**`void` 作为返回类型可以被不同的类型替换，以允许高级回调模式**：

```ts
function doSomething(callback: () => void) {
  // 在当前环境中，callback 总是返回 undefined
  let c = callback()
  // c 也是 undefined 类型
}

// 这个函数返回一个数字
function aNumberCallback(): number {
  return 2
}

// 正常工作，doSomething 确保了类型安全
doSomething(aNumberCallback)
```

这是理想的行为，并且经常在 JavaScript 应用程序中使用。

如果你想确保传递的函数只返回 `undefined`，请确保调整你的回调方法签名:

```ts
function doSomething(callback: () => undefined) {
  /* ... */
}
function aNumberCallback(): number {
  return 2
}

// 类型不匹配
doSomething(aNumberCallback)
```
