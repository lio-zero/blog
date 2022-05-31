# TypeScript 基础 — Object 类型

`object` 表示非原始类型，也就是除 `number`、`string`、`boolean`、`symbol`、`bigInt`、`null` 或 `undefined` 之外的类型。

```ts
const user: { name: string; age: number } = {
  name: 'O.O',
  age: 18
}
```

## 类型推断

TypeScript 可以根据属性的值推断属性的类型。

```ts
const user = {
  type: 'gold'
}

user.type = 'wood'
user.type = 10 // type error
```

## 可选属性

可选属性是不必在对象定义中定义的属性。

不带可选属性：

```ts
const user: { name: string; age: number } = {
  name: 'O.O'
} // type error
```

带可选属性：

```ts
const user: { name: string; age?: number } = {
  name: 'O.O'
}

user.age = 18
```

## 索引签名

索引签名可用于没有定义属性列表的对象。

```ts
const user: { [index: string]: string } = {}
user.name = 'O.O'
user.age = 18 // type error
```
