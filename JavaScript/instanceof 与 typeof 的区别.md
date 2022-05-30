# instanceof 与 typeof 的区别

[`instanceof`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof) 和 [`typeof`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof) 是检查值类型的两个运算符。

## 区别

`typeof` 运算符检查值是否具有原始类型的类型，原始类型可以是 `Boolean`、`Function`、`Object`、`Number`、`String`、`Undefined`、`Symbol`（ES6） 和 `bigint` 之一。

```js
typeof 'helloworld' // 'string'
typeof new String('helloworld') // 'object'
```

`instanceof` 运算符检查值是否为类或构造函数的实例。

```js
'helloworld' instanceof String // false
new String('helloworld') instanceof String // true
```

## 用法

如果要检查值是原始字符串还是 `String` 对象，则需要同时使用两个运算符：

```js
const isString = (value) => typeof value === 'string' || value instanceof String

isString('helloworld') // true
isString(new String('helloworld')) // true
```

另一种方法是依赖 `Object` 的 `toString()`，如下所示：

```js
const isString = (value) =>
  Object.prototype.toString.call(value) === '[object String]'

isString('hello world') // true
isString(new String('hello world')) // true
isString(10) // false
```

> **注意**：除了 `null` 和 `undefined` 之外，其他的类型都有 `toString()` 方法，它返回相应值的字符串表现。

我们可以使用类似的方法根据给定的原始或包装的原始类型检查值：

```js
const isBoolean = (value) =>
  Object.prototype.toString.call(value) === '[object Boolean]'
```

使用构造函数创建原始类型的值时要小心。可以根据使用方式更改值的类型。在下面的代码段中，我们从字符串构造函数创建 `String` 开始：

```js
let message = new String('hello')
message instanceof String // true
typeof message // 'object'
```

我们将用另一个字符串附加 `string` 对象：

```js
message += ' world'
```

现在，让我们看看运算符的结果：

```js
message instanceof String // false
typeof message // 'string'
```

这些类型修改称为装箱和拆箱。装箱是按对象包装原始值的过程。拆箱从对象中提取包装的原始值。

将 `typeof` 与 `null` 一起使用时有一种特殊情况：

```js
typeof null // object, 而不是 null
```

`instanceof` 不适用于原始类型。

如果希望始终使用 `instanceof`，则可以通过使用 [`Symbol.hasInstance`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/hasInstance) 键实现静态方法来重写 `instanceof` 的行为。

在以下代码中，我们创建了一个名为 `PrimitiveNumber` 的类，用于检查值是否为数字：

```js
class PrimitiveNumber {
  static [Symbol.hasInstance](value) {
    return typeof value === 'number'
  }
}

12345 instanceof PrimitiveNumber // true
'helloworld' instanceof PrimitiveNumber // false
```
