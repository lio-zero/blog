# TypeScript 基础 — 枚举

## 什么是枚举？

在大多数面向对象的编程语言（如 C、C# 和 Java）中，有一种数据类型我们称为枚举 — 简称为 [`enum`](https://www.typescriptlang.org/docs/handbook/enums.html)。Java 枚举是一种特殊的 Java 类，用于定义常量集合。然而，JavaScript 没有 `enum` 数据类型，但幸运的是，它们现在[在 TypeScript 2.4 版本中可用](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-4.html)。

枚举是一组可以采用数字或字符串形式的命名常量。与 TypeScript 中可用的某些类型不同，枚举是经过预处理的，不会在编译时或运行时进行测试。

```ts
enum State {
  on = 'ON',
  off = 'OFF'
}
```

`enum` 类型是对 JavaScript 标准数据类型的一个补充。使用枚举类型可以为一组数值赋予友好的名字。

```ts
enum Color {
  Red,
  Green,
  Blue
}

let c: Color = Color.Green
```

## 为什么要枚举？

枚举只是在 TypeScript 中组织代码的一种有用方式。以下是枚举派上用场的一些原因：

- 使用枚举，您可以创建易于关联的常量，使常量更加清晰
- 开发人员可以自由地在 JavaScript 中创建内存高效的自定义常量。正如我们所知，JavaScript 不支持枚举，但 TypeScript 可以帮助我们访问它们
- TypeScript 枚举使用 JavaScript 中的内联代码节省了运行时间和编译时间
- TypeScript 枚举还提供了我们以前只有在 Java 等语言中才有的一定灵活性。这种灵活性使我们可以轻松地表达和记录我们的意图和用例

## TypeScript 枚举的类型

TypeScript 枚举共有三种类型，即：

1. 数字枚举
2. 字符串枚举
3. 异构枚举

## 数字枚举

```ts
enum Days {
  Sun,
  Mon,
  Tue
}

console.log(Days) // { '0': 'Sun', '1': 'Mon', '2': 'Tue', Sun: 0, Mon: 1, Tue: 2 }
```

默认情况下，第一个枚举成员的值是 0，后面的枚举成员的值依次加 1。也可以指定一个起始值。

### 自定义枚举

```ts
enum Color {
  Red = 1,
  Green,
  Blue
}

console.log(Color) // { '1': 'Red', '2': 'Green', '3': 'Blue', Red: 1, Green: 2, Blue: 3 }
```

可以看到，我们为第一个成员添加为 1，其余成员会依次递增 1。

我们也可以将其设置为一个没有规则顺序：

```ts
enum Color {
  Red = 1,
  Green = 10,
  Blue = 8
}

console.log(Color) // { '1': 'Red', '10': 'Green', '8': 'Blue', Red: 1, Green: 10, Blue: 8 }
```

### 常量枚举

常量枚举其实就是在 `enum`关键字前使用 `const` 修饰符。它会在**编译阶段被移除**。

如果想提高数值枚举的性能，可以将它们声明为常量。我们来看一个简单的示例：

```ts
enum Month {
  Jan,
  Feb,
  Mar
}

console.log(Month.Jan) // 0
```

当编译成 JavaScript 时，运行时会查找 `Month` 和 `Month.Jan`。为了在运行时获得最佳性能，可以将枚举设置为常量，如下所示：

```ts
const enum Month {
  Jan,
  Feb,
  Mar
}

console.log(Month.Jan) // 0
```

编译时使用常量生成的 JavaScript 为：

```js
console.log(0 /* Jan */)
```

如果我们加上 `console.log(Month)`，他在编译阶段将会报错。

常量枚举成员在使用的地方被内联进来，且常量枚举不可能有**计算成员**。

```ts
const enum Directions {
  Up,
  Right = 3,
  UpRight = 3 + 1,
  RightDown = Directions.Right + 2,
  RightUp = 'hello'.length // Error
}
```

其实，当您引用到上面的错误代码时，VSCode 编辑器已经智能的提示您当前代码有误：**常量枚举**成员初始值设定项只能包含文字值和其他计算的枚举值。

> **总结**：当我们不需要整个对象，而需要对象的某个值时，就可以使用常量枚举，这样就可以避免在编译时生成多余的代码和间接引用。

### 计算枚举

数值枚举的值可以是常量，也可以是求值的，就像 TypeScript 中的任何其他数字数据类型一样。可以使用计算值定义或初始化数值枚举：

```ts
enum Weekend {
  Friday = 1,
  Saturday = getDate('TGIF'),
  Sunday = Saturday * 40
}

function getDate(day: string): number {
  if (day === 'TGIF') {
    return 3
  }
}

Weekend.Saturday // 3
Weekend.Sunday // 120
```

当枚举包含**计算成员**和**常量成员**的混合时，未初始化的枚举成员要么排在第一位，要么必须排在其他具有数值常量的初始化成员之后。

忽略上述规则可能会导致初始化错误，请记住相应地重新排列枚举成员。

## 字符串枚举

字符串枚举是不可以做双向映射的。

```ts
enum State {
  success = '成功',
  fail = '失败'
}

console.log(State) // { success: '成功', fail: '失败' }
```

## 异构枚举

TypeScript 还允许混合字符串和数字，称为异构枚举值：

```ts
enum Weekend {
  Friday = 'FRIDAY',
  Saturday = 1,
  Sunday = 2
}

console.log(Weekend)

/*
{
  '1': 'Saturday',
  '2': 'Sunday',
  Friday: 'FRIDAY',
  Saturday: 1,
  Sunday: 2
}
*/
```

## 枚举成员

枚举成员的值不可修改

```ts
enum State {
  success = '成功',
  fail = '失败'
}

State.success = '等待成功' // 无法分配到 "success" ，因为它是只读属性。
```

枚举成员可以分为**常量枚举**和**计算枚举**成员。在上面数字枚举内我们已经提到。

**const enum（常量枚举）**：

1. 没有设置初始值
2. 对已有枚举成员的引用
3. 常量的表达式

常量枚举成员会在编译时计算出结果，然后以常量的形式，出现在运行时环境

**computed enum（计算枚举）**：

顾名思义，需要计算的枚举成员，这些枚举成员的值不会在编译阶段计算，而是保留到程序的执行阶段。

## const enum 与 computed enum 的区别

可以使用或不使用 `const` 关键字声明枚举。以下是常规枚举的示例：

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
```

`const` 枚举：

```ts
const enum Light {
  Red,
  Green,
  Blue
}
```

## 区别

TypeScript 将常规枚举编译为 JavaScript 对象。给定上面的 `enum` 方向，它将被编译为以下 JavaScript 代码：

```ts
var Direction
;(function (Direction) {
  Direction[(Direction['Up'] = 0)] = 'Up'
  Direction[(Direction['Down'] = 1)] = 'Down'
  Direction[(Direction['Left'] = 2)] = 'Left'
  Direction[(Direction['Right'] = 3)] = 'Right'
})(Direction || (Direction = {}))
```

另一方面，Light 枚举根本不被编译。如果不使用枚举，您将看不到任何内容。

在其他情况下，所有枚举引用都被内联代码替换。例如，`console.log(Light.Red)` 编译为 `console.log(0 /* Red */)`。

由于没有在运行时生成与 `const` 枚举关联的 JavaScript 对象，因此不可能在 `const` 枚举值上循环。

当我们尝试迭代 `Light` 枚举时，TypeScript 将抛出一个错误：

```js
// ERROR
for (let i in Light) {
  console.log(i)
}
```

## 很高兴知道

如果不希望 TypeScript 去除为 `const` 枚举生成的代码，可以使用 `preserveConstEnums` 编译器标志。

```json
{
  "compilerOptions": {
    "preserveConstEnums": true
  }
}
```
