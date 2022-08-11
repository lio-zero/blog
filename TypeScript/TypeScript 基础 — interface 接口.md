# TypeScript 基础 — interface 接口

> TypeScript 的核心原则之一是对值所具有的结构进行类型检查。

在 TypeScript 里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

接口（`interface`）是最常用的类型标注方式。

```ts
// interface 关键字
interface user {
  name: string
  age: number
}

function printUser(userObj: user) {
  console.log(userObj.name)
}

const myObj = {
  name: 'lio',
  age: 18
}

printUser(myObj)
```

`user` 接口中给将来可能需要用到的对象属性定义类型。

> **注意**：类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以

## 可选属性（`?`）

接口里的属性不全都是必需的

```ts
interface user {
  name?: string
  age?: number
}

function printUser(userObj: user): { name: string; height: number } {
  let newUser = { name: 'O.O', height: 180 }
  if (userObj.name) {
    newUser.name = userObj.name
  }
  if (userObj.age) {
    newUser.height = userObj.age * userObj.age
  }
  return newUser
}

const myObj = { name: 'D.O' }
printUser(myObj) // { name: 'D.O', height: 180 }
```

带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个 `?` 符号。

好处：

- 可以对可能存在的属性进行预定义
- 可以捕获引用了不存在的属性时的错误

## 只读属性（`readonly`）

一些对象属性只能在对象刚刚创建的时候修改其值。你可以在属性名前用 `readonly` 来指定只读属性：

```ts
interface Point {
  readonly x: number
  readonly y: number
}

const p: Point = { x: 10, y: 20 }
p.x = 5 // error
```

TypeScript 具有 `ReadonlyArray` 类型，它与 `Array` 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。

```ts
let arr: number[] = [1, 2, 3, 4]
const ro: ReadonlyArray<number> = arr

ro[0] = 12 // error!
ro.push(5) // error!
ro.length = 100 // error!
arr = ro // error
```

就算把整个 `ReadonlyArray` 赋值到一个普通数组也是不可以的。但是你可以用类型断言重写：

```ts
arr = ro as number[]
```

## 函数类型

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。

它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```ts
interface SearchFunc {
  (source: string, subString: string): boolean
}

const mySearch: SearchFunc
mySearch = function (source: string, subString: string) {
  const result = source.search(subString)
  return result > -1
}
```

## 类型合并

```ts
interface User {
  name: string
  age: number
}

interface User {
  address: object
}

const user: User = {
  name: 'D.O',
  age: 18,
  address: {}
}

console.log(user) // { name: 'D.O', age: 18, address: {} }
```

VS Code 编辑器虽然会提示您 `'User'` 已被定义。但是 `interface` 接口还是会对相同的接口名进行合并，并正常编译输出。

## 任意数量标识

有时候类型数量是动态的，但类型是指定需要约束的，可以这样写

```ts
interface IVariate {
  first: string
  readonly second: number
  [key: string]: number
}
```

## 接口继承（`extends`）

继承使用 `extends` 关键字

**单接口继承**：

```ts
interface two extends one {
  name: string
}
```

**多接口继承**：使用逗号 `,` 分隔各个接口

```ts
interface three extends one, two {
  name: string
}
```

> 推荐：[TypeScript 基础 — interface 和 type 的区别](https://github.com/lio-zero/blog/blob/main/TypeScript/TypeScript%20%E5%9F%BA%E7%A1%80%20%E2%80%94%20interface%20%E5%92%8C%20type%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)
