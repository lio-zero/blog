# TypeScript 基础 — 类

TypeScript 向 JavaScript 类添加类型和可访问性修饰符。

## 类成员的类型

类的成员（属性和方法）使用类型注释进行类型化，类似于变量。

```ts
class Person {
  name: string
}

const person = new Person()
person.name = 'O.O'
```

## 类成员的可访问性

类成员也会被赋予影响可访问性的特殊修饰符。

TypeScript 中有三个主要的可访问性修饰符：

- `public` —（默认）允许从任何地方访问类成员
- `private` — 只允许从类内访问类成员
- `protected` — 允许从类成员本身以及继承它的任何类访问该类成员

```ts
class Person {
  private name: string

  public constructor(name: string) {
    this.name = name
  }

  public getName(): string {
    return this.name
  }
}

const person = new Person('O.O')

console.log(person.getName()) // O.O
console.log(person.name) // person.name 是私有的，因此无法从类外部访问
```

类中的 `this` 关键字通常指的是该类的实例。

## 参数属性

TypeScript 通过向参数添加可访问性修饰符，提供了一种在构造函数中定义类成员的方便方法。

```ts
class Person {
  // name 是私有成员变量
  public constructor(private name: string) {}
  public getName(): string {
    return this.name
  }
}

const person = new Person('O.O')
console.log(person.name)
console.log(person.getName()) // O.O
```

## Readonly

与数组类似，`readonly` 关键字可以防止更改类成员。

```ts
class Person {
  private readonly name: string

  public constructor(name: string) {
    // name 不能在初始定义之后更改，初始定义必须在其声明或构造函数中。
    this.name = name
  }

  public getName(): string {
    // this.name = 'D.O' // error
    return this.name
  }
}

const person = new Person('O.O')
console.log(person.getName()) // O.O
```

## 继承：Implements 关键字

接口可以用来定义类必须遵循 `implements` 关键字的类型。

```ts
interface Shape {
  getArea: () => number
}

class Rectangle implements Shape {
  public constructor(
    protected readonly width: number,
    protected readonly height: number
  ) {}

  public getArea(): number {
    return this.width * this.height
  }
}
```

一个类可以通过在 `implements` 后面列出每个接口来继承多个接口，每个接口之间用逗号分隔，例如：`class Rectangle implements Shape, Colored {`

## 继承：Extends 关键字

类可以通过 `extends` 关键字相互继承。一个类只能继承另一个类。

```ts
interface Shape {
  getArea: () => number
}

class Rectangle implements Shape {
  public constructor(
    protected readonly width: number,
    protected readonly height: number
  ) {}

  public getArea(): number {
    return this.width * this.height
  }
}

class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width)
  }

  // getArea 从 Rectangle 中继承
}
```

## Override 关键字

当一个类继承另一个类时，它可以用相同的名称替换父类的成员。

较新版本的 TypeScript 允许使用 `override` 关键字显式标记此项。

```ts
interface Shape {
  getArea: () => number
}

class Rectangle implements Shape {
  // 为这些成员使用 protected 允许从该类扩展而来的类访问，比如 Square
  public constructor(
    protected readonly width: number,
    protected readonly height: number
  ) {}

  public getArea(): number {
    return this.width * this.height
  }

  public toString(): string {
    return `Rectangle[width=${this.width}, height=${this.height}]`
  }
}

class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width)
  }

  // toString 替换 Rectangle 中的 toString
  public override toString(): string {
    return `Square[width=${this.width}]`
  }
}
```

默认情况下，重写方法时，`override`关键字是可选的，仅有助于防止意外重写不存在的方法。使用设置 `noImplicitOverride` 强制在重写时使用它。

## 抽象类

类的编写方式可以允许它们用作其他类的基类，而不必实现所有成员。这是通过使用 `abstract` 关键字来实现的。未实现的成员也使用 `abstract` 关键字。

```ts
abstract class Polygon {
  public abstract getArea(): number

  public toString(): string {
    return `Polygon[area=${this.getArea()}]`
  }
}

class Rectangle extends Polygon {
  public constructor(
    protected readonly width: number,
    protected readonly height: number
  ) {
    super()
  }

  public getArea(): number {
    return this.width * this.height
  }
}
```

抽象类不能直接实例化，因为它们没有实现所有成员。

## Implements 和 Extend 关键字的区别
