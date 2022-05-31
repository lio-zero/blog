# JavaScript 和 TypeScript 中的布尔值

`boolean` 是 JavaScript 中一种原始数据类型。在 TypeScript 中，它总共允许四个值（**?**）。

## JavaScript 中的布尔值

`boolean` 可以取 `true` 和 `false` 的值。其他类型的值可以是 truthy 或 falsy，例如 `undefined` 或 `null`。

```js
let b = true
if (b) console.log('logged')

b = false
if (b) console.log('not logged')

b = undefined
if (b) console.log('not logged')

b = null
if (b) console.log('not logged')
```

除了 `undefined`、`null` 或 `false` 被认为是 falsy，`""`（空字符串，包括单引号和反引号）、`-0` 和 `0`，以及`NaN` 也为 falsy。

要获取任意值的布尔值，可以使用 `Boolean` 函数：

```js
Boolean(false) // false
Boolean(true) // true
Boolean('false') // true
Boolean('Hello World') // true
Boolean({}) // true
Boolean([]) // true
Boolean(123.4) // true
Boolean(Symbol()) // true
Boolean(function () {}) // true
Boolean(undefined) // false
Boolean(null) // false
Boolean(NaN) // false
Boolean(0) // false
Boolean('') // false
```

> 有一个便捷的技巧：使用 `!!`，它与 `Boolean` 具有一样的效果。您可以尝试一下。

所有空值的计算结果为 `false`。空对象 `{}` 和空数组 `[]`（它本身就是一个对象）确实有值，因为它们是其他值的容器。

`Boolean` 函数非常适合从集合中过滤空值：

```js
const collection = [
  { name: 'D.O', age: 20 },
  undefined,
  { name: 'D.', age: 30 },
  false,
  { name: 'C.', age: 2 },
  false
]

collection.filter(Boolean)
```

如果加上 `Number`，它将所有的值转换成对应的 `number` 或 `NaN`，这是一个非常酷的方式来快速获得实际值:

```js
const x = ['1.23', 2137123, 'D.O', false, 'O.O', undefined, null]

x.map(Number).filter(Boolean) // [1.23, 2137123]
```

`Boolean` 作为构造函数的形式存在，其转换规则与 `Boolean` 函数相同。但是，使用 `new Boolean(...)` 创建一个包装对象，它的值比较为 `true`，但引用比较 `false`：

```js
const value = Boolean('Stefan') // true
const reference = new Boolean('Stefan') // [Boolean: true]

value == reference // true
value === reference // false
```

您可以通过 `valueOf()` 获得值，在进行比较：

```js
value === reference.valueOf() // true
```

## TypeScript 中的布尔值

在 TypeScript 中，`boolean` 是一种原始类型。请务必使用小写版本，不要引用 `Boolean` 中的对象实例

```ts
// bad
const boolObject: Boolean = false

// good
const boolLiteral: boolean = false
```

这是可行的，但这是一种糟糕的做法，因为我们几乎不需要 `new Boolean` 对象。

您可以在 TypeScript 中将 `true`、`false`、`undefined` 和 `null` 分配给 `boolean`，而无需进行严格的 `null` 检查。

```ts
const boolTrue: boolean = true // ✅
const boolFalse: boolean = false // ✅
const boolUndefined: boolean = undefined // ✅
const boolNull: boolean = null // ✅
```

因此，`boolean` 是我们唯一可以通过联合类型完全表达的：

```ts
type MyBoolean = true | false | null | undefined // 与布尔值相同

const mybool: MyBoolean = true
const yourbool: boolean = false
```

当启用 `strictNullChecks` 编译器标志时，值将减少为 `true` 和 `false`。

```ts
const boolTrue: boolean = true // ✅
const boolFalse: boolean = false // ✅
const boolUndefined: boolean = undefined // ❌
const boolNull: boolean = null // ❌
```

所以我们的集合总共减少到两个值。

```ts
type MyStrictBoolean = true | false
```

我们还可以使用 `NonNullable` 帮助器类型去除空值：

```ts
type NonNullable<T> = T extends null | undefined ? never : T

type MyStrictBoolean = NonNullable<MyBoolean> // true | false
```

`boolean` 由一组仅用于条件的有限值组成，这一事实允许有趣的条件类型。

想象一下通过函数在数据存储中发生的变化。您在更新 `userId` 的函数中设置了一个标志。你必须提供 `userId`，然后:

```ts
type CheckUserId<Properties, AddUserId> = AddUserId extends true
  ? Properties & { userId: string }
  : Properties & { userId?: string }
```

根据通用 `AddUserId` 的值，我们希望属性 `userId` 被设置或者是可选的。

通过从我们期望的类型扩展泛型，我们可以使这个类型更加明确

```ts
- type CheckUserId<Properties, AddUserId> =
+ type CheckuserId<
+  Properties extends {},
+  AddUserId extends boolean
+ >
     AddUserId extends true
     ? Properties & { userId: string }
     : Properties & { userId?: string }
```

在使用中，它可能会声明如下函数：

```ts
declare function mutate<P, A extends boolean = false>(
  props: CheckUserId<P, A>,
  addUserId?: A
): void
```

请注意，我甚至为 `A` 设置了一个默认值，以确保 `CheckUserId` 根据要设置或不设置的 `addUserId` 提供正确的信息。

实际作用：

```js
mutate({}) // ✅
mutate({ data: 'Hello World' }) // ✅
mutate({ name: 'O.O' }, false) // ✅
mutate({ name: 'O.O' }, true) // ❌ userId 丢失
mutate({ name: 'O.O', userId: 'ABCD' }, true) // ✅ userId is here
```

如果您的代码很大程度上依赖于真值或假值，这将非常方便。
