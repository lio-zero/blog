# Number() 和 parseInt() 的区别

`Number()` 和 `parseInt()` 通常用于将字符串转换为数字。

## 区别

`Number()` 转换类型，而 `parseInt` 解析输入值。

```js
// 解析
parseInt('32px') // 32
parseInt('5e1') // 5

// 转换类型
Number('32px') // NaN
Number('5e1') // 50
```

如您所见，`parseInt` 将最多解析第一个非数字字符。另一方面，`Number` 将尝试转换整个字符串。

`parseInt` 将第一个字符串转换为字符串，查看下面的浮点数解析：

```js
parseInt(0.1) // 0
parseInt(0.01) // 0
parseInt(0.001) // 0
```

`parseInt` 将参数转换为字符串后，无法解析 `.` 符号，所以只取到 `0`。

`parseInt` 接受两个参数。第二个参数用于指示基数。

```js
parseInt('0101') // 101
parseInt('0101', 10) // 101
parseInt('0101', 2) // 5

Number('0101') // 101
```

当我们传递特殊值（如 `undefined` 或 `null`）时，它们返回不同的结果：

```js
parseInt() // NaN
parseInt(null) // NaN
parseInt(true) // NaN
parseInt('') // NaN

Number() // 0
Number(null) // 0
Number(true) // 1
Number('') // 0
```

## 建议

始终将基数传递给 `parseInt`。

`parseInt` 方法采用两个参数：

```js
parseInt(value, radix)
```

第二个参数指定当前的数字系统。如果未指定，则将根据该值自动设置。

如果该值以 `0x` 或 `0X` 开头，则基数为 16（十六进制）

在其他情况下，基数为 10（十进制）。

在旧版本的 JavaScript 中，如果字符串以 0 开头，则基数设置为 8（八进制）。

```js
parseInt('0xF') // 15
parseInt('0XF') // 15
parseInt('0xF', 16) // 15

parseInt('0xF', 10) // 0
```

由于该方法在不同版本的 JavaScript 和浏览器中可以实现不同，因此建议传递基数。

在解析数字之前修剪空格。

`Number()` 和 `parseInt` 都接受输入中的空格。但请注意，在传递带有空格的值时，可能会得到不同的结果，如下所示：

```js
parseInt('   5   ') // 5
parseInt('12 345') // 12，不是12345
```

为避免类似情况，应在分析之前删除所有空格：

```js
parseInt(value.replace(/\s+/g, ''), 10)
```

不要使用 `new Number()` 来比较数字。

```js
Number('2') == 2 // true
Number('2') === 2 // true

new Number('2') == 2 // true
new Number('2') === 2 // false

const a = new Number('2')
const b = new Number('2')

a == b // false
a === b // false
```

## 提示

您可以使用 `+` 运算符，而不是使用 `Number()` 构造函数将字符串转换为数字：

```js
+'010' // 10
+'2e1' // 20
+'0xF' // 15
```
