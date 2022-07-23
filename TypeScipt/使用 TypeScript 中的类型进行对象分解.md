# 使用 TypeScript 中的类型进行对象分解

在编写项目时，我们经常会用到解构。但如果是在 TS 项目，它有所不同。

例如：

```ts
const { name, age } = body.value
```

我尝试添加如下 `string` 和 `number` 类型：

```ts
const { name: string, age: number } = body.value
```

但这不起作用。它显然有效，但实际上这是将 `name` 属性分配给 `string` 变量，将 `age` 属性值分配给 `number` 变量。

正确的语法如下：

```ts
const { name, age }: { name: string; age: number } = body.value
```

最好的方法是为该数据创建一个 `type` 或 `interface`：

```ts
type User = {
  name: string
  age: number
}
// or
interface User {
  name: string
  age: number
}
```

这种方式更加简洁：

```ts
const user: User = body.value
```
