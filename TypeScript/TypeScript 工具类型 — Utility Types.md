# TypeScript 工具类型 — Utility Types

TypeScript 附带了大量类型，可以帮助进行一些常见的类型操作，通常称为 [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)。

本章介绍了最流行的工具类型。

## Partial

`Partial` 将对象中的所有属性更改为可选。

```ts
interface Point {
  x: number
  y: number
}

let pointPart: Partial<Point> = {} // Partial 允许 x 和 y 是可选的
pointPart.x = 10
```

## Required

`Required` 更改对象中需要的所有属性。与 `Partial` 相反。

```ts
interface User {
  name?: string
  age?: number
}

const user: User = { name: 'O.O' }

const user2: Required<User> = { name: 'O.O' }
// 类型 "{ name: string; }" 中缺少属性 "age"，但类型 "Required<User>" 中需要该属性。ts(2741)
```

## Record

`Record` 是定义具有特定键类型和值类型的对象类型的快捷方式。

```ts
const nameAgeMap: Record<string, number> = {
  KAI: 27,
  LAY: 30
}
```

`Record<string, number>` 相当于 `{ [key: string]: number }`

## Omit

`Omit` 从对象类型中删除键。

```ts
interface Person {
  name: string
  age: number
  location?: string
}

const user: Omit<Person, 'age' | 'location'> = {
  name: 'D.O' // Omit 从类型中删除了 age 和 location，不能在此处定义
}
```

## Pick

`Pick` 从对象类型中删除除指定键以外的所有键。与 `Omit` 相反。

```ts
interface Person {
  name: string
  age: number
  location?: string
}

const user: Pick<Person, 'name'> = {
  name: 'O.O' // Pick 只保留了 name，因此 age 和 location 已从类型中删除，无法在此处定义
}
```

## Exclude

`Exclude` 从联合中删除类型。

```ts
type Primitive = string | number | boolean

const value: Exclude<Primitive, string> = true // 此处不能使用字符串，因为 Exclude 将其从类型中删除。
```

## ReturnType

`ReturnType` 提取函数类型的返回类型。

```ts
type foo = () => {
  x: number
  y: number
}

const point: ReturnType<foo> = {
  x: 10,
  y: 20
}
```

## Parameters

`Parameters` 将函数类型的参数类型提取为数组。

```ts
type PointPrinter = (p: { x: number; y: number }) => void
const point: Parameters<PointPrinter>[0] = {
  x: 10,
  y: 20
}
```

## 更多资料

更多工具类型的详细内容可查阅 TS 官网的 [Utility Types 章节](https://www.typescriptlang.org/docs/handbook/utility-types.html#requiredtype)。

- https://juejin.cn/post/6994102811218673700#heading-3
