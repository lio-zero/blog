# TypeScript 基础 — 泛型

泛型允许创建 “类型变量”，可以用来创建不需要显式定义所使用类型的类、函数和类型别名。

泛型使编写可重用代码变得更容易。

## 函数

带有函数的泛型有助于生成更通用的方法，从而更准确地表示所使用和返回的类型。

```ts
function foo<S, T>(v1: S, v2: T): [S, T] {
  return [v1, v2]
}

console.log(foo<string, number>('hello', 20)) // ['hello', 20]
```

TypeScript 还可以从函数参数推断泛型参数的类型。

如果您使用的是 VS Code 编辑器，这一点尤其明显：

![推断泛型参数的类型](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f2737c07a9c643129e2ae8279aac444f~tplv-k3u1fbpfcp-watermark.image?)

## 类

泛型可以用来创建泛型类，比如 Map。

```ts
class NamedValue<T> {
  private _value: T | undefined

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value
  }

  public getValue(): T | undefined {
    return this._value
  }

  public toString(): string {
    return `${this.name}: ${this._value}`
  }
}

let value = new NamedValue<number>('myNumber')
value.setValue(10)
console.log(value.toString()) // myNumber: 10
```

如果在构造函数参数中使用泛型参数，则 TypeScript 还可以推断泛型参数的类型。

## 类型别名

类型别名中的泛型允许创建更可重用的类型。

```ts
type Wrapped<T> = { value: T }

const wrappedValue: Wrapped<number> = { value: 10 }
```

这也适用于具有 `interface Wrapped<T> { ... }` 语法的接口。

## 默认值

泛型可以指定默认值，如果没有指定或推断其他值，则默认值适用。

```ts
class NamedValue<T = string> {
  private _value: T | undefined

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value
  }

  public getValue(): T | undefined {
    return this._value
  }

  public toString(): string {
    return `${this.name}: ${this._value}`
  }
}

let value = new NamedValue('myNumber')
value.setValue('myValue')
console.log(value.toString()) // myNumber: myValue
```

## 继承

可以向泛型中添加约束以限制允许的内容。这些约束使得在使用泛型类型时可以依赖更具体的类型。

```ts
function foo<S extends string | number, T extends string | number>(
  v1: S,
  v2: T
): [S, T] {
  console.log(v1, v2)
  return [v1, v2]
}

foo('a', 'b') // a b
foo('a', 1) // a 1
foo('a', true) // TypeError
```

这可以与默认值结合使用。
