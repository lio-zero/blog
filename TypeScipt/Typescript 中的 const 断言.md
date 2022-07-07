# TypeScript 中的 const 断言

在 TypeScript 4.3 中，TypeScript 引入了 `const` 断言。`const` 断言用于告诉 TypeScript 编译器以下内容之一：

## 对象属性是只读的

将对象强制转换为 `const` 时，属性标记为只读，无法修改。

以下面的 `person` 变量为例，其中包含 `name` 和 `age`。

```ts
const person = {
  name: 'O.O',
  age: 20
}
```

其类型按预期推断为 `string` 和 `number`。

但如果我们将其断言为 `const`，`person` 对象的推断类型将被标记为只读，并且无法修改。

```ts
const person = {
  name: 'O.O',
  age: 20
} as const

person.name = 'D.O' // TypeError
```

如果尝试更新 `age` 字段，则会抛出错误。

## 数组变成只读元组

数组上的 `const` 断言允许我们将数组标记为只读元组，即数组中每个位置的内容都成为无法修改的文本类型。

例如：

```ts
const arr = ['O.O', 20]
```

TypeScript 会将其推断为字符串或数字数组，即 `(string | number)[]`。

![数组变成只读元组](https://upload-images.jianshu.io/upload_images/18281896-3525987296e5e7e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是，如果我们使用 `as const` 断言，它将被限制为只读元组，即第一个位置为 `'O.O'`，第二个位置为 `25`，并且它的值不能被修改：

```ts
const arr = ['O.O', 20] as const

arr[0] = 'D.O' // TypeError
```

## 变量值应被视为文字类型

文字类型允许我们定义更具体的类型，而不是像字符串或数字这样泛化的类型。例如：

```ts
type Switch = 'On' | 'Off'
```

`const` 断言允许我们将变量值标记为文字类型。例如，如果我们有一个变量 `onSwitch`，并为其赋值为 `On`，那么 TypeScript 通常会将变量的类型推断为字符串：

```ts
let onSwitch = 'On'
```

![字符串](https://upload-images.jianshu.io/upload_images/18281896-8cac895de33cc56c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是，如果我们使用 `const` 断言，它将被推断为 `On` 的文字类型：

```ts
let onSwitch = 'On' as const
```

![文字类型](https://upload-images.jianshu.io/upload_images/18281896-8f171acc96a349df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

并且不能接受除 `On` 之外的任何其他变量：

```ts
onSwitch = 'Off' // TypeError
```

需要记住的一点是，`const` 断言只能应用于简单表达式。所以你不能这样做：

```ts
function switchValue(input: boolean) {
  let onSwitch = (input ? 'On' : 'Off') as const // 行不通
  return onSwitch
}
```

为了解决上述问题，我们需要对三元运算符的每个输出值应用 `const` 断言：

```ts
function switchValue(input: boolean) {
  let onSwitch = input ? ('On' as const) : ('Off' as const)
  return onSwitch
}
```

`onSwitch` 变量的类型被推断为一个文本类型联合 `On | Off`。

---

通过使用 `as const` 语法告诉编译器将元组字面量的类型推断为元组字面量，而不是 `string[]`。

```ts
const list = ['a', 'b', 'c'] as const
```

![元组字面量](https://upload-images.jianshu.io/upload_images/18281896-57ad9dfb14041776.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种类型的断言导致编译器推断出一个值可能的最窄类型，包括将所有内容设为只读的。它应该如下所示：

```ts
type NeededUnionType = typeof list[number] // 'a'|'b'|'c'
```

![只读联合类型](https://upload-images.jianshu.io/upload_images/18281896-b558bd7d93da267e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

来自 [TypeScript derive union type from tuple/array values](https://stackoverflow.com/questions/45251664/typescript-derive-union-type-from-tuple-array-values)
