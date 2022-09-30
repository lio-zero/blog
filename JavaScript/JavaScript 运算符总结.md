# JavaScript 运算符总结

本文整理了 JavaScript 所有的[运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)，并提供了一些示例理解它们。

## 箭头运算符（=>）

从技术上讲，它不是运算符，但是在**箭头功能**中使用了这种字符组合。

箭头函数是编写函数定义的另一种方法。

箭头函数在某种程度上受到限制：它没有自己的上下文（因此 `this` 无法使用），没有 `arguments` 和 `super`，因此不能使用 `new` 进行调用。

- 如果它只用到一个参数，箭头函数可以省略括号
- 如果省略括号，箭头函数有一个隐式的返回
- 箭头功能常用作数组方法的回调

```js
let sayHi = () => 'Hello!'
sayHi() // "Hello!"

let arr = [1, 2, 3]
arr.forEach(console.log) // 1 2 3
```

> 推荐：[JavaScript 中定义函数的方法](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0%E7%9A%84%E6%96%B9%E6%B3%95.md)。

## 加法运算符（+）

此运算符将两个数字加在一起，或将两个字符串连接在一起。

如果将数字和字符串一起使用，它将执行串联。

较不常见的是，它可以用作[一元运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Unary_plus)，尝试将包含数字的字符串转换为数字。这是一个晦涩的技巧，通常最好使用[Number()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) 方法。

```js
console.log(1 + 1) // 2
console.log('hello' + 'world') // 'helloworld'

console.log(1 + '1') // '11'

// 作为一元运算符
console.log(+'5') // 5
console.log(+'hello') // NaN
```

## 加法赋值运算符（+=）

此运算符将提供的值添加到变量，然后用结果覆盖该变量的值。

```js
let x = 10
let y = 10

x += 1
console.log(x) // 11 (10 + 1)

y += 5
console.log(y) // 15 (10 + 5)
```

## 减法运算符（-）

此运算符从另一个减去一个数字。这是一个数学运算符。

作为一元运算符，它也可以用来表示数字为负数（例如 `-5`，不减去任何东西），表示该数字的值为负 5。

```js
console.log(4 - 1) // 3

// 作为一元运算符
const negativeNum = -5
console.log(5 - negativeNum) // 10
```

## 减法赋值运算符（-=）

该运算符从变量中减去提供的值，然后用结果覆盖该变量的值。

```js
let x = 10
let y = 10

x -= 1
console.log(x) // 9 (10 - 1)

y -= 5
console.log(y) // 5 (10 - 5)
```

## 赋值运算符（=）

该运算符为变量赋值一个值。不要与相等运算符（`==`）或严格相等运算符（`===`）运算符混淆！

```js
let key = '123456'
```

## 相等运算符（==）

该运算符检查两个值是否相等。返回一个布尔值。

与严格相等运算符（`===`）不同，此运算符将忽略类型并仅关注值。例如，数字 `2` 被认为等效于字符串 `"2"`。

```js
// 数字不同，因此它们不相等
console.log(10 == 11) // false

// 数字相同，所以相等！
console.log(10 == 10) // true

// 值匹配，与类型无关
console.log(10 == '10') // true
```

## 严格相等运算符（===）

该运算符检查两个值是否相等。返回一个布尔值。

与相等运算符（`==`）不同，此运算符检查类型和值。例如，数字 `2` 不视为等效于字符串 `"2"`。

```js
// 数字不同，因此它们不相等
console.log(10 === 11) // false

// 数字相同，这样就相等了！
console.log(10 === 10) // true

// 值相同，但类型不同
console.log(10 === '10') // false
```

## 不等运算符（!=）

`true` 如果每侧代表不同的值，则此运算符返回。

与严格不等式运算符（`!==`）不同，不考虑值的类型。例如，该数字 `2` 被认为等效于字符串 `"2"`，因此表达式 `2 != '2'` 返回 `false`。通常这是不好的，建议无论什么时候都使用**严格的不等运算符**。

```js
const a = 10

// 数字不同，因此它们是不相等的
console.log(a != 11) // true
// 数字相同，它们不是不相等的
console.log(a != 10) // false
// 即使类型不同，值相同，所以它们是被视为相等。
console.log(a != '10') // false
```

## 严格不等运算符（!==）

该运算符检查两个值是否不相等。返回一个布尔值。

与不等式运算符（`!=`）不同，此运算符同时考虑类型和值。例如，数字 `2` 不视为等效于字符串 "2"。

```js
const a = 10

// 数字不同，因此它们是不相等的
console.log(a !== 11) // true

// 数字相同，因此它们不是不相等的
console.log(a !== 10) // false

// 值相同，但类型不同，因此它们仍然被认为是不平等的
console.log(a !== '10') // true
```

## 逗号运算符（,）

逗号运算符按值序列评估每个值，然后返回最终值。

注意，逗号字符通常不用作运算符。例如，当将多个参数传递给函数时，逗号字符不能用作运算符。那是完全无关的事情。

```js
const x = (2, 3)

console.log(x) // 3
```

## 自减运算符（--）

此运算符用于包含数字的变量。使用时，它将变量的值减 1。

这通常与 `while` 循环一起使用，或者在任何需要计数器的地方使用。

它在功能上等同于 `x = x - 1;`

```js
let x = 5
x--
console.log(x) // 4
```

## 自增运算符（++）

该运算符旨在用于保存数字的变量。使用时，该变量的值增加 1。

这通常与循环一起使用，或在需要计数器的任何地方使用。 `while`

它在功能上等同于 `x = x + 1;`

```js
let x = 5

x++
console.log(x) // 6
```

## 除法运算符（/）

该运算符执行数学除法运算。可以认为它是易于键入的除法符号（`÷`）

```js
console.log(10 / 2) // 5
console.log(5 / 4) // 1.25
```

## 除法赋值运算符（/=）

该运算符对变量执行数学除法运算，然后将结果赋值给该变量。

```js
let x = 6
x /= 3

console.log(x) // 2
```

## 幂运算符(**)

幂运算符也叫做指数运算符，该运算符将值本身乘以指定次数，它相当于 `Math.pow()` 方法。

```js
let x = 2

console.log(2 ** 2) // 4
console.log(2 ** 3) // 8
console.log(2 ** 4) // 16

console.log(2 ** 0) // 1
```

## 幂赋值运算符（**=）

该运算符应用乘幂运算（将值乘以 X 的幂），将结果赋值给受影响的变量。它类似于其他赋值运算符，例如：加法赋值（`+=`）运算符。

```js
let x = 2
x **= 4
console.log(x) // 16 (2 ** 4)
```

## 逻辑与运算符（&&）

该运算符通常用于检查所有提供的值是否真实。

它也可以用作控制流运算符：它将返回第一个伪造的值。如果所有值都不伪造，它将返回最终值。

```js
// 作为逻辑运算符
if (someCondition && someOtherCondition) {
  // Code here is only run if both
  // variables are true, or truthy.
}

// 作为控制流运算符
console.log(0 && 4) // 0, 因为它是第一个虚假值
console.log(2 && 4) // 4, 因为两个值都不是虚假的
console.log(1 && 2 && 3) // 3, 因为所有值都是真实的
console.log('a' && '' && 0) // '', 第一个虚假值
```

## 逻辑与赋值运算符（&&=）

它将为变量赋值一个值，但前提是该变量已具有真实值。

```js
let x = 0
let y = 1

x &&= 2 // 没有效果，因为 0 是虚假的
y &&= 2 // 赋值 2，因为1为真

console.log(x, y) // 0, 2
```

## 逻辑非运算符（!）

逻辑非运算符，它会翻转布尔值，将真值转换为假值，反之亦然。

它也适用于非布尔值，任何虚假的值都会评估为 `false`，同样任何真实的值都会评估为 `true`

- Falsy：`false`、`null`、`undefined`、`NaN`、`0`、`+0`、`-0` 和空字符串（""、''、``）
- truthy：其他都为 `true`

```js
console.log(!true) // false
console.log(!false) // true

console.log(!0) // true
console.log(!10) // false
```

`!!` 运算符可以将右侧的值强制转换为布尔值

- 第一个 `!` 将值强制为布尔值并将其取反。在这种情况下，`!0` 将返回 `true`。
- 因此，要将其恢复为 `false`，我们可以再加上一个 `!`。

```js
console.log(!!0) // false
console.log(!!10) // true
```

## 逻辑或运算符（||）

该运算符通常用于检查任何一个提供的值是否真实。

它也可以用作控制流运算符：它将返回第一个真实值。如果所有值都不是真实的，它将返回最终值。

```js
// 作为逻辑运算符
if (someCondition || someOtherCondition) {
  // 仅在以下至少之一的情况下才运行此处的代码
  // 这两个变量包含真实值。
}

// 作为控制流运算符
console.log(0 || 4) // 4，因为它是第一个真实值
console.log(2 || 4) // 2，因为它是第一个真实值
console.log('' || 0) // 0，因为没有值是真实的
console.log(1 || 2 || 3) // 1，因为它立即发现了真实的价值！
```

## 逻辑或赋值运算符（||=）

它将为变量赋值一个值，但前提是该变量已具有伪造的值。

```js
let x = 0
let y = 1

x ||= 2 // 赋值为2，因为0为假
y ||= 2 // 无效，因为'y'保持真实值
console.log(x, y) // 2, 1
```

## 逻辑空赋值运算符（??=）

该运算符将使用新值更新变量，但前提是该变量当前值为 `null` 或 `undefined`。

这是 JavaScript 相对较新的功能（Chrome 85，Firefox 79，Safari 14）。IE 还不支持。

```js
let x = 0
let y = null
let z

x ??= 2 // 无效，因为 'x' 已经拥有一个值
y ??= 2 // 更新 'y' 来保存这个新值
z ??= 2 // 更新'z'来保存这个新值（通过最初不赋值 'z' 值，被认为是未定义的）。

console.log(x, y, z) // 0, 2, 2
```

## 乘法运算符（*）

该运算符执行数学乘法运算，计算操作数的乘积。

```js
console.log(10 * 2) // 20
console.log(1.25 * 4) // 5
```

## 乘法赋值运算符（*=）

该运算符对变量执行数学乘法运算，将变量与右操作数的值相乘，然后将结果赋值给该变量。

```js
let x = 6
x *= 3

console.log(x) // 18
```

## 空值合并运算符（??）

这种相对较新的语言添加方式类似于逻辑或运算符（`||`），除了不依赖 `truthy/falsy` 值，它依赖于空值（只有 2 个空值，`null` 和 `undefined`）

这意味着当您将虚假值（如 0）视为有效值时，使用它更安全。

与逻辑或类似，它用作控制流运算符。它的计算结果为第一个非空值。

它是在 Chrome 80 / Firefox 72 / Safari 13.1 中引入的。它还不支持 IE。

```js
console.log(4 ?? 5) // 4，因为两个值都不为空

console.log(null ?? 10) // 10，因为 'null' 为空

console.log(undefined ?? 0) // 0，因为 'undefined' 为空

// 这是一个不同于逻辑或（||）
console.log(0 ?? 5) // 0
console.log(0 || 5) // 5
```

## 可选链运算符（?.）

该运算符类似于**属性访问器**运算符（`.`）；它访问对象的属性。

不同之处在于链是安全的；如果在任何一点上出现空值（`null` 或 `undefined`），链就会短路并返回 `undefined`。

可选链 `?.` 语法有三种形式：

- `obj?.prop` 对象属性

```js
let user = {}

// 当属性不存在时
console.log(user.address.street) // Uncaught TypeError: Cannot read property 'street' of undefined
console.log(user?.address?.street) // undefined
```

- `obj?.[expr]` 对象属性

```js
let user = {
  name: 'lisi',
  hi: {
    name: 'zhaosi'
  }
}

// 当属性名称存储在变量中时
let outerKey = 'hi'
let innerKey = 'name'

console.log(user?.[outerKey]) // { name: "zhaosi" }
console.log(user?.[outerKey]?.[innerKey]) // zhaosi
```

- `func?.(...args)` 函数或对象方法的调用

```js
// 当所讨论的属性是一个函数时
let user = {
  sayHi() {
    return 'hi'
  }
}

console.log(user.sayHi?.()) // "hi"
console.log(user.running?.()) // undefined
```

## 管道运算符（|>）

该运算符目前是第 2 阶段建议的功能；它在任何浏览器中都不存在。

它起一种 "反向" 函数调用的作用；不是使用参数调用函数，而是将参数 "传递" 到函数中。

这在某些函数式编程范例中很有用。例如，它为[柯里化](https://github.com/lio-zero/blog/blob/main/JavaScript/%E6%9F%AF%E9%87%8C%E5%8C%96.md)（Currying）提供了更好的接口。

```js
// 典型的函数调用：
alert('hello')

// 用管道替代：
'hello' |> alert
```

> 推荐：[如何在 JavaScript 中使用管道（管道运算符）？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E4%BD%BF%E7%94%A8%E7%AE%A1%E9%81%93%EF%BC%88%E7%AE%A1%E9%81%93%E8%BF%90%E7%AE%97%E7%AC%A6%EF%BC%89%EF%BC%9F.md)

## 属性访问器运算符（.）

访问对象的属性时，可以使用点表示法将其拔出。仅当您知道该属性的密钥时，此方法才有效。如果该属性名称保存在变量中，则需要使用方括号表示法。

```js
const user = {
  name: 'lisi',
  address: {
    street: '123 Street Ave.',
    city: 'Guangzhou',
    province: 'Guangdong',
    country: 'China'
  }
}

console.log(user.name) // 'lisi'
console.log(user.address.city) // 'Guangzhou'

// 如果将某个想取到的值保存在变量中，我需要使用方括号。
const value = 'city'
console.log(user.address.value) // undefined
console.log(user.address[value]) // 'Guangzhou'
```

## 余数（模）运算符（%）

该运算符与加法（`+`）或乘法（`*`）属于同一数学族。

它的计算结果是将左侧值除以右侧的余数。

```js
console.log(4 % 2) // 0

console.log(10 % 7) // 3
```

## 余数（模）赋值运算符（%=）

该运算符是余数运算符（`%`）的“赋值”版本。它对变量执行运算，并将结果赋值给该变量。

```js
let num = 5

num %= 4
console.log(num) // 1
```

## 剩余/扩展运算符（...）

根据上下文，此运算符实际上是两个单独的运算符。

**剩余运算符**：函数定义中使用此运算符来收集其他函数参数。当您不知道函数需要多少个参数时很有用。它将它们收集到一个数组中。它也用于解构数组和对象。

```js
// 剩余操作符
function rest(...nums) {
  console.log(nums)
}

rest(1, 2, 3, 4) // [1, 2, 3, 4]

// 解构数组
const [s1, s2, ...rest] = ['red', 'yellow', 'green', 'orange', 'purple']
console.log(rest) // ["green", "orange", "purple"]

// 解构对象
const { age, name, ...others } = {
  name: 'lisi',
  age: 18,
  children: {
    name: 'zhaosi',
    age: 20
  }
} // others: { name: 'zhaosi', age: 20 }
```

**扩展运算符**：执行与 "rest" 相反的操作，可用于从数组中填充函数。它也可以用于克隆或合并数组和对象。

```js
// 扩展运算符
function addNums(a, b) {
  console.log(a + b)
}

const nums = [1, 1]
addNums(...nums) // 2

// 克隆数组
const arr = [1, 2, 3]
const arrCopy = [0, ...arr] // [0, 1, 2, 3]
console.log(arrCopy)

// 克隆对象
const user = { name: 'lisi', age: 17 }
const modifyUser = { ...user, age: 18 }
console.log(modifyUser) // { name: 5, age: 18 };
```

## 大于运算符（>）

该运算符检查左侧的值是否大于右侧的值。

使用 `>、<、>=、<=` 四个运算符时要注意：对于数字，这可以按您预期的那样工作。对于字符串，每个字符都将转换为相应的字符代码。

```js
console.log(10 > 5) // true
console.log(10 > 15) // false
console.log(-20 > 15) // false
console.log(0.5 > 1) // false
console.log(1 > 1) // false

console.log('b' > 'a') // true
console.log('B' > 'a') // false
```

## 小于运算符（<）

该运算符检查左侧的值是否小于右侧的值。

```js
console.log(10 < 5) // false
console.log(10 < 15) // true
console.log(-20 < 15) // true
console.log(0.5 < 1) // true
console.log(1 < 1) // false

console.log('b' < 'a') // false
console.log('B' < 'a') // true
```

## 大于或等于运算符（>=）

该运算符检查左侧的值是否大于或等于右侧的值。

```js
console.log(10 >= 5) // true
console.log(10 >= 15) // false
console.log(-20 >= 15) // false
console.log(0.5 >= 1) // false
console.log(1 >= 1) // true

console.log('b' >= 'a') // true
console.log('B' >= 'a') // false
```

## 小于或等于运算符（<=）

该运算符检查左侧的值是否小于或等于右侧的值。

```js
console.log(10 <= 5) // false
console.log(10 <= 15) // true
console.log(-20 <= 15) // true
console.log(0.5 <= 1) // true
console.log(1 <= 1) // true

console.log('b' <= 'a') // false
console.log('B' <= 'a') // true
```

## 按位异或运算符（^）

该运算符对变量执行按位异或运算。在每个位位置返回 1，其中一个操作数（而不是两个操作数）的对应位为 1。

```js
const a = 5 // 00000000000000000000000000000101
const b = 3 // 00000000000000000000000000000011

console.log(a ^ b) // 00000000000000000000000000000110
// 输出: 6
```

> 操作数转换为 32 位整数，并用一系列位（0 和 1）表示。超过 32 位的数字将丢弃其最高有效位。例如，以下大于 32 位的整数将转换为 32 位整数：
>
> Before: 11100110111110100000000000000110000000000001
> After: 10100000000000000110000000000001

## 按位 NOT 运算符（~）

该运算符反转其操作数的位。

```js
const a = 5 // 0000000000000101
const b = -3 // 1111111111111101

console.log(~a) // 1111111111111010
// 输出: -6

console.log(~b) // 0000000000000010
// 输出: 2
```

## 其他运算符

本文篇幅过长，以下运算法您可以在 MDN 的 [Expressions and operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators) 章节查看它们的具体用法。

### 按位与运算符（&）

该运算符在每个位的位置都返回 1，两个操作数的对应位都为 1。不要与逻辑 AND 运算符（`&&`）混淆

### 按位与赋值运算符（&=）

该运算符对变量执行按位与运算，并将结果覆盖到相同的变量中。

### 按位或运算符（|）

该运算符执行按位或运算。不要与逻辑或运算符（`||`）混淆

### 按位或赋值运算符（|=）

该运算符对变量执行按位或运算，并将结果赋值给同一变量。

### 按位 XOR 赋值运算符（^=）

该运算符对变量执行按位 XOR 运算，并将结果赋值给同一变量。

### 左移运算符（<<）

该运算符执行按位移位操作。

### 右移运算符（>>）

该运算符执行按位移位操作。

### 左移赋值运算符（<<=）

该运算符对变量执行按位移位操作，并使用此新值覆盖变量。

### 右移赋值运算符（>>=）

该运算符对变量执行按位移位操作，并使用此新值覆盖变量。

### 无符号右移运算符（>>>）

该运算符执行按位移位操作。

### 无符号右移赋值运算符（>>>=）

该运算符对变量执行按位移位操作，并使用此新值覆盖变量。

## 更多资料

[Operator Lookup](https://www.joshwcomeau.com/operator-lookup/)
