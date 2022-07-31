# TypeScript 中的类型收窄

在本文中，我们将学习各种收窄类型的方法。**类型（narrowing）收窄**是将类型从不太精确的类型推导为更精确的类型的过程。

让我们从一个简单的函数开始：

```ts
function friends(input: string | number) {
  // code here
}
```

上面的函数可以接受一个数字或一个字符串。假设我们要根据 `input` 是数字还是字符串来执行不同的操作。在这种情况下，我们将使用 JavaScript 类型保护来检查它是字符串还是数字，如下所示：

```ts
function someFunc(input: string | number) {
  if (typeof input === 'string') {
    // do something with the string
    console.log('input is a string')
  }

  if (typeof input === 'number') {
    // do something with number
    console.log('input is a number')
  }
}
```

## 类型保护 — Type Guard

在上面的示例中，我们使用 JavaScript 类型保护将输入类型收窄为数字或字符串。类型保护用于检查变量是否属于特定类型，即数字、字符串、对象等。使用类型保护时，Typescript 希望该变量属于该类型。它将根据该信息自动键入并检查其使用情况。

示例中，我们使用 `typeof` 运算符收窄类型，它检查值是否具有原始类型的类型。

以下是可用的 JavaScript 类型保护的列表：

**`string`**

```ts
if (typeof param === 'string') {
  // do something with string value
}
```

**`number`**

```ts
if (typeof param === 'number') {
  // do something with number value
}
```

**`bigint`**

```ts
if (typeof param === 'bigint') {
  // do something with bigint value
}
```

**`boolean`**

```ts
if (typeof param === 'boolean') {
  // do something with boolean value
}
```

**`symbol`**

```ts
if (typeof param === 'symbol') {
  // do something with symbol value
}
```

**`undefined`**

```ts
if (typeof param === 'undefined') {
  // do something with undefined value
}
```

**`object`**

```ts
if (typeof param === 'object') {
  // do something with object value
}
```

**`function`**

```ts
if (typeof param === 'function') {
  // do something with the function
}
```

下面我们来看一下其他的收窄类型方法。

## 真值收窄

在这种类型收窄中，我们在使用变量之前检查它是否为真。当变量为真时，TypeScript 将自动消除该变量在条件检查中出错的可能性，即 `undefined` 或 `null` 等。

举个例子，下面的函数 `foo` 接受一个参数 `x`，其类型为 `string` 或 `undefined`（即可选）。

```ts
function foo(x?: string) {
  if (x) {
    console.log(typeof x) // "string"
  }
}

foo('TS')
```

通过检查 `x` 是否为 `true`，`x` 的类型将变为 `string`，否则为 `undefined`。

很多情况下都会进行真值收窄：表达式（`||`、`&&`、`!`）、`!!` 和 `Boolean` 等。

## 等值收窄

如果两个变量相等，那么两个变量的类型必须相同。如果一个变量的类型不精确（即 `unknown`、`any` 等），并且等于另一个精确类型的变量，那么 TypeScript 将使用该信息收窄第一个变量的类型。

以下面的函数为例，它有两个参数：`x` 和 `y`，`x` 是 `string` 或 `number`，`y` 是 `number`。当 `x` 的值等于 `y` 的值时，`x` 的类型被推断为 `number`，否则为 `string`。

```ts
function someFunction(x: string | number, y: number) {
  if (x === y) {
    // 收窄到 number
    console.log(typeof x) // number
  } else {
    // 这并没有收窄范围
    console.log(typeof x) // number or string
  }
}
```

## 可识别联合

在这种方法中，您创建了一个对象，其中包含一个文本成员，可以用来区分两个不同的联合。

以下例子中，函数计算不同形状的正方形、矩形和圆形。我们将从定义 `Rectangle` 和 `Circle` 的类型开始。

```ts
type Rectangle = {
  shape: 'reactangle'
  width: number
  height: number
}

type Circle = {
  shape: 'circle'
  radius: number
}
```

从上述类型中，每个对象都将具有 `shape` 的文字字段，可以是 `circle` 或 `rectangle`。我们可以使用函数中的 `shape` 字段来计算面积，这将接受 `Rectangle` 和 `Circle` 的并集，如下所示：

```ts
function calculateArea(shape: Rectangle | Circle) {
  if (shape.shape === 'reactangle') {
    // 只能访问 reactangle 的属性，不能访问 circle 的属性
    console.log(shape.height * shape.width)
  }

  if (shape.shape === 'circle') {
    // 只能访问 circle 的属性，而不能访问 reactangle 的属性
    console.log(3.14 * shape.radius * shape.radius)
  }
}
```

当 `shape` 字段是矩形时，您只能访问 `Rectangle` 类型中可用的属性，即 `width`、`height` 和 `shape`。这同样适用于当 `shape` 字段是 `circle` 时，TypesSript 只允许您访问 `radius` 和 `circle`，否则将抛出错误。

## 使用 `in` 运算符进行收窄

`in` 运算符用于确定对象是否具有包含的属性名。它以 `"property" in object` 的格式使用，其中 `property` 是要检查其是否存在于 `object` 中的属性名。

在上面的例子中，我们使用有区别的并集来区分圆和矩形。我们也可以使用 `in` 运算符来实现同样的效果，但这次我们将检查一个形状是否包含某些属性，即 `Circle` 的 `radius`、`Rectangle` 的 `width` 和 `height`，结果将是相同的。

```ts
type Circle = {
  radius: number
}

type Reactangle = {
  width: number
  height: number
}

function calculateArea(shape: Circle | Reactangle) {
  if ('radius' in shape) {
    // 现在，您可以从 shape 访问 radius
    console.log(3.14 * shape.radius * shape.radius)

    //  任何访问 height 或 width 的尝试都将导致错误
    shape.width // TypeError
    shape.height // TypeError
  }
  if ('width' in shape && 'height' in shape) {
    // 现在可以从 shape 对象访问 height 和 width
    console.log(shape.height * shape.width)

    // 任何访问 raidus 的尝试都会导致错误
    shape.radius // TypeError
  }
}
```

## 使用赋值收窄

在这种类型的收窄中，一旦给变量赋值，TypeScript 就会收窄变量的类型。取一个 `number` 或 `string` 的联合类型的变量 `x`，如果我们给它赋值 `number`，类型就变成了一个数，如果我们给它赋值 `string`，类型就变成了字符串。

```ts
let x: number | string = 1
console.log(typeof x) // "number"

x = 'something'
console.log(typeof x) // "string"
```

## 使用 `instanceof` 进行收窄

JavaScript 的 `instanceof` 运算符用于检查某个值是否是某个类的实例。它以 `value instanceof value2` 的格式使用，并返回一个布尔值。当检查某个值是否为类的实例 `instanceof` 时，TypeScript 会将该类型分配给变量，从而收窄类型。

以下面的示例为例，其中函数接受一个参数，可以是字符串或日期。如果是日期，我们将其转换为字符串，如果是字符串，我们将按原样返回。我们可以使用 `instanceof` 检查它是否是 `Date` 的实例，并将其转换为字符串，如下所示：

```ts
function dateToString(value: string | Date) {
  if (value instanceof Date) {
    // 现在的类型是 Date，您可以访问 Date 方法
    return value.toISOString()
  }
  return value
}
```
