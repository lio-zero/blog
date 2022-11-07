# TypeScript 泛型

泛型允许创建**类型变量**，可以用来创建不需要显式定义所使用类型的类、函数、类型别名或接口。

它可以使编写可重用代码变得更容易。

## 函数

带有函数的泛型有助于生成更通用的方法，从而更准确地表示所使用和返回的类型。

```ts
function foo<S, T>(v1: S, v2: T): [S, T] {
  return [v1, v2]
}

foo<string, number>('hello', 20) // ['hello', 20]
```

可以看到，`S` 和 `T` 相当于一个占位，在调用时，我们会显示的为泛型传递一个类型，类似于类型系统的传参，以便它能正确的解析类型。

这种方式也被开发者称为泛型的**正向推导**。

既然有正向推导，那么必然也会有**反向推导**。例如：

```ts
function foo<T>(v: T): T {
  return v
}

foo('hello')
```

尝试运行它，你会发现，在没有显示的传递一个类型时，**TypeScript 依然可以从函数参数正确推断出泛型参数的类型**。

**为什么呢？**

因为 `foo` 函数所有被使用到的泛型 `T` 之间是互相关联的，当我们调用函数并传入参数时，它们将被统一推断出来。

如果您使用的是 VS Code 编辑器，这一点尤其明显：

![推断泛型参数的类型](https://upload-images.jianshu.io/upload_images/18281896-dc4c2dbc2cb7237a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

## 默认类型

泛型可以指定默认值，如果没有指定或推断出其他值，则使用默认值。

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

## 约束

当我们使用泛型时，我们可以使用约束来应用于泛型类型参数。

要实现它，我们可以使用 `extends` 关键字，它可以向泛型中添加约束以限制允许的内容。这些约束使得在使用泛型类型时可以依赖更具体的类型。

```ts
function foo<T extends string | number>(arg: T): T {
  return arg
}

foo('a') // a
foo(1) // 1
foo(true) // TypeError
```

可以看到，我们限制了泛型 `T` 只能继承 `string` 和 `number` 类型，其他任何类型都将抛出错误。

它可以与默认类型结合使用。

另外，`extends` 关键字仅适用于接口和类，否则会抛出错误。

值得注意的是，如果**一开始**没有明确指定泛型参数为哪种类型，我们无法操作它的属性或方法。例如：

```ts
function foo<T>(arg: T): T {
  console.log(arg.length) // TypeError
}

foo<string>('hello')
```

可以看到，即使你在调用时，指定了泛型类型，它依然会报出类型错误。需要配合 `extends` 关键字或类型推导。

## 工具类型

泛型（Generics）通常被用来设置一些高级类型。

我们之前写过的一篇 [TypeScript 工具类型 — Utility Types](https://github.com/lio-zero/blog/blob/main/TypeScript/TypeScript%20%E5%B7%A5%E5%85%B7%E7%B1%BB%E5%9E%8B%20%E2%80%94%20Utility%20Types.md)，它们都是使用**泛型**实现。

## 何时使用泛型？

要确定何时使用泛型类型，可以考虑两件事：

- 函数或类是否需要处理各种数据类型？
- 函数或类是否会在多个地方使用？

## 更多资料

- [TypeScript Generics for People Who Gave Up on Understanding Generics](https://ts.chibicode.com/generics/)
- [Understanding TypeScript Generics](https://www.smashingmagazine.com/2020/10/understanding-typescript-generics/)
- [轻松拿下 TS 泛型](https://juejin.cn/post/7064351631072526350)
