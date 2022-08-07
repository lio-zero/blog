# TypeScript 基础 — 函数

TypeScript 有一个特定的语法用于输入函数参数和返回值。

## 返回类型

可以显式定义函数返回的值的类型。

```ts
// :number 指定函数返回一个数字
function getTime(): number {
  return new Date().getTime()
}
```

如果没有定义返回类型，TypeScript 将尝试通过返回的变量或表达式的类型来推断它。

## void 返回类型

类型 `void` 可用于指示函数不返回任何值。

```ts
function printHello(): void {
  console.log('Hello!')
}
```

## 参数

函数参数的类型化语法与变量声明类似。

```ts
function multiply(a: number, b: number) {
  return a * b
}
```

如果没有定义参数类型，TypeScript 将默认使用 `any`，除非有额外的类型信息，如下面的默认参数和类型别名部分所示。

## 可选参数

默认情况下，TypeScript 将假定所有参数都是必需的，但它们可以显式标记为可选。

```ts
// ? 运算符在这里将参数 c 标记为可选
function add(a: number, b: number, c?: number) {
  return a + b + (c || 0)
}
```

注意，可选参数不能直接使用，它需要配合[类型收窄](https://github.com/lio-zero/blog/blob/main/TypeScipt/TypeScript%20%E4%B8%AD%E7%9A%84%E7%B1%BB%E5%9E%8B%E6%94%B6%E7%AA%84.md)。

## 默认参数

对于具有默认值的参数，默认值位于类型注释之后：

```ts
function pow(value: number, exponent: number = 10) {
  return value ** exponent
}
```

TypeScript 还可以从默认值推断类型。

## 命名参数

键入命名参数遵循与键入普通参数相同的模式。

```ts
function divide({ dividend, divisor }: { dividend: number; divisor: number }) {
  return dividend / divisor
}
```

## Rest 参数

Rest 形参可以像普通形参一样具有类型，但类型必须是数组，因为 Rest 参数始终是数组。

```ts
function add(a: number, b: number, ...rest: number[]) {
  return a + b + rest.reduce((p, c) => p + c, 0)
}
```

## 类型别名

函数类型可以与具有类型别名的函数分开指定。

这些类型的编写方式与箭头函数类似。

```ts
type Negate = (value: number) => number

// 在这个函数中，参数 value 会自动从 Negate 类型中获得 number 类型
const negateFunction: Negate = (value) => value * -1
```

## 更多资料

- [使用泛型定义函数](https://github.com/lio-zero/blog/blob/main/TypeScript/TypeScript%20%E5%9F%BA%E7%A1%80%20%E2%80%94%20%E5%87%BD%E6%95%B0.md)
- [TypeScript 基础 — interface 中的函数和属性](https://github.com/lio-zero/blog/blob/main/TypeScript/TypeScript%20%E5%9F%BA%E7%A1%80%20%E2%80%94%20interface%20%E4%B8%AD%E7%9A%84%E5%87%BD%E6%95%B0%E5%92%8C%E5%B1%9E%E6%80%A7.md)
