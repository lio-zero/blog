# TypeScript 装饰器

随着 TypeScript 和 ES6 里引入了类，在一些场景下我们需要额外的特性来支持标注或修改类及其成员。

TypeScript 官方描述对装饰器如下：

> [装饰器（Decorators）](https://www.typescriptlang.org/docs/handbook/decorators.html#accessor-decorators)提供了一种为类声明及成员添加注释和元编程语法的方法。

简单的说：**您可以通过使用装饰器对代码的某些部分进行注释来更改它们的行为。**

其实，早在几年前 TC39 就已提出装饰器提案，并持续迭代，但目前该提案还处于第二阶段。详细内容可查阅 [proposal-decorators](https://github.com/tc39/proposal-decorators#decorators)。

## 启动装饰器

装饰器是一项实验性功能，因此默认情况下是禁用的。若要启用实验性的装饰器特性，你必须在命令行或 `tsconfig.json` 里启用 `experimentalDecorators` 编译器选项。

命令行：

```bash
tsc --target ES5 --experimentalDecorators
```

`tsconfig.json` 文件：

```json
{
  "compilerOptions": {
    "target": "ES5",
    "experimentalDecorators": true
  }
}
```

## 装饰器的类型

装饰器是一种特殊类型的声明，有 5 种不同类型的装饰器：

- [类装饰器](https://www.typescriptlang.org/docs/handbook/decorators.html#class-decorators)
- [属性装饰器](https://www.typescriptlang.org/docs/handbook/decorators.html#property-decorators)
- [方法装饰器](https://www.typescriptlang.org/docs/handbook/decorators.html#method-decorators)
- [访问器装饰器](https://www.typescriptlang.org/docs/handbook/decorators.html#accessor-decorators) - 应用于 `getter` 和 `setter`
- [参数装饰器](https://www.typescriptlang.org/docs/handbook/decorators.html#parameter-decorators)

以下示例显示了它们可以应用的位置：

```js
// 这不是可运行的代码，因为没有定义 decorators
@classDecorator
class Polygon {
  @propertyDecorator
  edges: number
  private _x: number

  constructor(@parameterDecorator edges: number, x: number) {
    this.edges = edges
    this._x = x
  }

  @accessorDecorator
  get x() {
    return this._x
  }

  @methodDecorator
  calculateArea(): number {
    // ...
  }
}
```

> **注意**：类构造函数不能应用装饰器。

装饰器使用 `@expression` 形式，`expression` 求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。

## 签名概述

每个装饰器函数接收不同的参数。但访问器装饰器是一个例外，因为它本质上只是一个方法装饰器，应用于访问器（`getter` 或 `setter`）。

不同的签名定义在 [`typescript/lib/lib.es5.d.ts`](https://github.com/microsoft/TypeScript/blob/main/lib/lib.es5.d.ts#L1417)：

```ts
interface TypedPropertyDescriptor<T> {
  enumerable?: boolean
  configurable?: boolean
  writable?: boolean
  value?: T
  get?: () => T
  set?: (value: T) => void
}

declare type ClassDecorator = <TFunction extends Function>(
  target: TFunction
) => TFunction | void
// 也适用于访问者装饰器
declare type PropertyDecorator = (
  target: Object,
  propertyKey: string | symbol
) => void
declare type MethodDecorator = <T>(
  target: Object,
  propertyKey: string | symbol,
  descriptor: TypedPropertyDescriptor<T>
) => TypedPropertyDescriptor<T> | void
declare type ParameterDecorator = (
  target: Object,
  propertyKey: string | symbol,
  parameterIndex: number
) => void
```

## 装饰器评估顺序

类中不同类型的装饰器按以下顺序评估：

- 实例成员 — 首先是属性装饰器，然后是访问器、参数或方法装饰器
- 静态成员 — 首先是属性装饰器，然后是访问器、参数或方法装饰器
- 参数装饰器应用于构造函数
- 类装饰器应用于类

将不同的类型，它们的签名和评估顺序放在一起：

```ts
function propertyDecorator(target: Object, propertyKey: string | symbol) {
  console.log('属性装饰器', propertyKey)
}

function parameterDecorator(
  target: Object,
  propertyKey: string | symbol,
  parameterIndex: number
) {
  console.log('参数装饰器', propertyKey, parameterIndex)
}

function methodDecorator<T>(
  target: Object,
  propertyKey: string | symbol,
  descriptor: TypedPropertyDescriptor<T>
) {
  console.log('方法装饰器', propertyKey)
}

function accessorDecorator<T>(
  target: Object,
  propertyKey: string | symbol,
  descriptor: TypedPropertyDescriptor<T>
) {
  console.log('访问者装饰器', propertyKey)
}

function classDecorator(target: Function) {
  console.log('类装饰器')
}

@classDecorator
class Polygon {
  @propertyDecorator
  private static _PI: number = 3.14
  @propertyDecorator
  edges: number
  private _x: number

  constructor(@parameterDecorator edges: number, x: number) {
    this.edges = edges
    this._x = x
  }

  @methodDecorator
  static print(@parameterDecorator foo: string): void {
    // ...
  }

  @accessorDecorator
  static get PI(): number {
    return Polygon._PI
  }

  @accessorDecorator
  get x() {
    return this._x
  }

  @methodDecorator
  calculateArea(@parameterDecorator bar: string): number {
    return this.x * 2
  }
}

console.log('实例化...')
new Polygon(3, 2)

// 属性装饰器 edges
// 访问者装饰器 x
// 参数装饰器 calculateArea 0
// 方法装饰器 calculateArea
// 属性装饰器 _PI
// 参数装饰器 print 0
// 方法装饰器 print
// 访问者装饰器 PI
// 参数装饰器 undefined 0
// 类装饰器
// 实例化...
```

## 装饰器工厂

装饰器工厂就是一个简单的函数，它返回一个表达式，以供装饰器在运行时调用。

```ts
function color(value: string) {
  // 这是一个装饰器工厂
  return function (target) {
    // 这是装饰器
    // 做一些带有 target 和 value 的事情
  }
}
```

示例：

```ts
function log(textToLog: string) {
  // 这是一个装饰器工厂
  return function (target: Object, propertyKey: string | symbol) {
    // 这是装饰器
    console.log(textToLog)
  }
}

class C {
  @log('这将被记录下来')
  x: number
}

// "这将被记录下来"
```

## 装饰器组合

多个装饰器可以同时应用到一个声明上：

```ts
// 书写在同一行上
@f @g x

// 书写在多行上
@f
@g
x
```

示例：

```ts
function log(textToLog: string) {
  console.log(`outer: ${textToLog}`)
  return function (target: Object, propertyKey: string | symbol) {
    console.log(`inner: ${textToLog}`)
  }
}

class C {
  // 书写在同一行上
  @log('first') @log('second') x: number

  // 书写在多行上
  @log('first')
  @log('second')
  x: number
}

// "outer: first"
// "outer: second"
// "inner: second"
// "inner: first"
```

在 TypeScript 里，当多个装饰器应用在一个声明上时会进行如下步骤的操作：

- 由上至下依次对装饰器表达式求值。
- 求值的结果会被当作函数，由下至上依次调用。

## 更多资料

[A Complete Guide to TypeScript Decorators](https://saul-mirone.github.io/a-complete-guide-to-typescript-decorator/)
