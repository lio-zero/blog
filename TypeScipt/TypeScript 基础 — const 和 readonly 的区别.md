# TypeScript 基础 — const 和 readonly 的区别

## 区别

`const` 用于变量。

```ts
const message = 'Hello'

// 不起作用
message = 'World'
```

而 `readonly` 用于属性。属性可以声明为类的成员。

```ts
class Triangle {
  public readonly numberOfVertices = 3
}

const triangle = new Triangle()

// 不起作用
triangle.numberOfVertices = 4
```

或 `type`、`interface`：

```ts
interface Person {
  firstName: string
  lastName: string
  readonly fullName: string
}
```

`const` 声明必须初始化，并且不能重新分配其值。可以在构造函数中重新分配 `readonly` 属性。

```ts
class Square {
  readonly numberOfVertices: number

  constructor() {
    this.numberOfVertices = 4
  }
}
```

如果不直接传递 `readonly` 属性的类或接口，而是传递别名，则可以更改 `readonly` 属性。

让我们看看上面的 `Person` 接口，假设我们有以下的功能来更新人员信息：

```ts
const updatePerson = (person: {
  firstName: string
  lastName: string
  fullName: string
}) => {
  person.fullName = `${firstName}, ${lastName}`
}
```

我们可以更新 `fullName` 属性，因为它是 `person` 参数的属性：

```ts
let person: Person = {
  firstName: 'Foo',
  lastName: 'Bar',
  fullName: 'Foo Bar'
}

updatePerson(person)

person.fullName // `Foo, Bar`
```

当然，如果我们传递原始类型 `Person`，编译器将抛出一个错误：

```ts
const updatePerson = (person: Person) => {
  // Error: Cannot assign to 'fullName' because it is a read only property
  // 提示很明显：无法分配给 fullName，因为它是只读属性
  person.fullName = `${person.firstName}, ${person.lastName}`
}
```

## 很高兴知道

在给定的类中，如果一个属性只有 `getter` 方法而没有 `setter` 方法，它将被视为只读。

```ts
class Square {
  side: number = 0

  get area() {
    return this.side * this.side
  }
}

const s = new Square()
```

设置 `s.area = 100` 将引发错误，因为 `area` 是仅准备为被获取的属性。

在 React 库中，我们不会直接更改组件的 `props` 和 `state`。因为 `props` 是不可变的，`state` 可以通过 `setState()` 方法更新。

React [类型定义](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts)将 `props` 和 `state` 包装为只读类型。

```ts
// P，S 分别代表 props 和 state
class Component<P, S> {
  constructor(props: Readonly<P>)

  readonly props: Readonly<P> & Readonly<{ children?: ReactNode }>
  state: Readonly<S>
}
```
