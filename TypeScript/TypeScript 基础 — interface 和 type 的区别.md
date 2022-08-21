# TypeScript 基础 — interface 和 type 的区别

使用 `interface` 和 `type` 声明是表示给定数据结构的常用方法。

```ts
// 使用接口
interface Point {
  x: number
  y: number
}

// 使用类型别名
type Point = {
  x: number
  y: number
}
```

## 区别

TypeScript 中类型别名和接口之间的区别在过去更为明显，但随着 TypeScript 的最新版本，它们变得越来越相似。

以下是它们的一些区别。

### 对象/函数

两者都可以用来描述对象的结构或函数签名。但语法不同。

```ts
// 接口
interface Point {
  x: number
  y: number
}

interface SetPoint {
  (x: number, y: number): void
}
```

类型别名

```ts
type Point = {
  x: number
  y: number
}

type SetPoint = (x: number, y: number) => void
```

### 其他类型

与接口不同，类型别名是数据类型的定义，可用于任何其他类型，例如：联合类型、交叉类型、原始类型、元组等。

```ts
// union
type PartialPoint = PartialPointX | PartialPointY

// object
type PartialPointX = { x: number }
type PartialPointY = { y: number }

// primitive
type Name = string

// tuple
type Data = [number, string]

// other
```

### 声明合并

与类型别名不同，一个接口可以定义多次，并将被视为一个接口（所有声明的成员都被合并）。

```ts
interface Point {
  x: number
}

interface Point {
  y: number
}

const p: Point = {
  x: 1,
  y: 2
}
```

而类型别名必须是唯一的，并且不允许合并声明，否则编译器将报错。

```ts
type Person = {
  firstName: string
}

// Throw error: Duplicate identifier
type Person = {
  lastName: string
}
```

能够合并接口声明是非常有用的。例如，当我们为一个外部库提供类型定义时，它不是完全用 TypeScript 创建的。

在这种情况下，如果缺少一些定义，我们可以通过声明合并来提供它们。

### 继承

两者都可以继承，但同样，语法不同。此外，请注意接口和类型别名不是相互排斥的。接口可以继承类型别名，反之亦然。

**接口继承接口**：

```ts
interface PartialPointX {
  x: number
}
interface Point extends PartialPointX {
  y: number
}
```

**类型别名继承类型别名**：

```ts
type PartialPointX = { x: number }
type Point = PartialPointX & { y: number }
```

**接口继承类型别名**：

```ts
type PartialPointX = { x: number }
interface Point extends PartialPointX {
  y: number
}
```

**类型别名继承接口**：

```ts
interface PartialPointX {
  x: number
}

type Point = PartialPointX & { y: number }
```

### `implement` 关键字

类可以以完全相同的方式实现接口或类型别名。但是请注意，类和接口被视为静态蓝图。因此，它们不能**实现/继承**命名联合类型的类型别名。

```ts
interface Point {
  x: number
  y: number
}

class SomePoint implements Point {
  x = 1
  y = 2
}

type Point2 = {
  x: number
  y: number
}

class SomePoint2 implements Point2 {
  x = 1
  y = 2
}

type PartialPoint = { x: number } | { y: number }

// FIXME: 无法实现联合类型
class SomePartialPoint implements PartialPoint {
  x = 1
  y = 2
}
```

## 建议

要决定是使用类型别名还是接口，您应该仔细考虑和分析情况，您正在处理什么，具体的代码等。

结合上述它们之间的区别，以下是一些我认为不错的建议：

- 如果您希望定义一个变量类型，就用 `type`，如果希望能够继承并约束，就用 `interface`
- 如果您不知道该用哪个，建议使用 `type`
- 如果您是库的作者或为外部库创建类型定义，请使用 `interface`，方便其他人也可以扩展它们

## 更多资料

Stack Overflow 有更多关于这方面的讨论，地址 👉 [Interfaces vs Types in TypeScript](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript)。
