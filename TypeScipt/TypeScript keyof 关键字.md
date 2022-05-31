# TypeScript keyof 关键字

`keyof` 是 TypeScript 中的关键字，用于从对象类型中提取键类型。

## 带有显式键的 `keyof`

在具有显式键的对象类型上使用时，`keyof` 会使用这些键创建联合类型。

```ts
interface Person {
  name: string
  age: number
}

// keyof Person 在这里创建一个 name 和 age 的联合类型，不允许使用其他字符串
function printName(person: Person, property: keyof Person) {
  console.log(`打印 person 属性 ${property}: '${person[property]}'`)
}

let person = {
  name: 'O.O',
  age: 18
}

printName(person, 'name') // 'O.O'
```

## 带索引签名的 `keyof`

`keyof` 还可以与索引签名一起使用，以提取索引类型。

```ts
type StringMap = {
  [key: string]: unknown
}

// 这里的 keyof StringMap 解析为 string
function foo(property: keyof StringMap, value: string): StringMap {
  return {
    [property]: value
  }
}

foo('name', 'O.O') // { name: 'O.O' }
```
