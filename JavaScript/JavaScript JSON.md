# JavaScript JSON

[JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)（JavaScript Object Notation）是一种轻量级的用于存储和传输数据的文本格式，2001 年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式。

它具有**自我描述的**且易于理解，同时也易于机器解析和生成，并有效地提升网络传输效率。

> **注意**：JSON 与任何语言无关，可以在任何编程语言中使用。

## 为什么需要 JSON？

在浏览器和服务器之间传输数据时，数据只能是文本。由于 JSON 格式仅为文本，因此可以轻松地与服务器之间进行传输，并且可以通过任何编程语言将其用作数据格式。

## JSON 语法规则

- 数据在键/值对中，并且都必须使用双引号包裹
- 数据用逗号分隔
- 花括号容纳对象
- 方括号包含数组
- 不能有尾随逗号

以下是一个正确的示例：

```json
{
  "name": "O.O",
  "place": "China",
  "friend": {
    "name": "K.O"
  },
  "hobby": ["sing", "jump", "rap", "basketball"],
  "number": [1, 2, 3, 4, 5, 6, 7, 8, 9]
}
```

## JSON 的典型错误

```json
{ "surname": 'Smith' } // ❌ 值使用的是单引号（必须使用双引号）

{ 'isAdmin': false } // ❌ 键使用的是单引号（必须使用双引号）

{ "birthday": new Date(2000, 2, 3) } // ❌ 属性值不能使用函数和日期对象

[32, 64, 128, 0xFFF] // ❌ 不能使用十六进制值

{ "name": "O.O", "age": undefined } // ❌ 不能使用 undefined

// ❌ 不能有尾随逗号
{ "name": "O.O", }
{ "hobby": ["sing", ] }
{
  "friend": {
    "name": "K.O",
  }
}
```

> **注意**：`null`、空数组（`[]`）和空对象（`{}`）都是合法的 JSON 值。

## JavaScript JSON 方法

JavaScript 提供了两个方法用于处理 JSON 数据格式，如下：

- `JSON.stringify` 将对象转换为 JSON。
- `JSON.parse` 将 JSON 转换回对象。

> 推荐：[格式化输出 JSON](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%A0%BC%E5%BC%8F%E5%8C%96%E8%BE%93%E5%87%BA%20JSON.md)

### JSON.stringify

JSON 通常用于与服务端交换数据。在向服务器发送数据时一般是字符串。

[`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 方法将 JavaScript 对象转换为字符串。

```js
let json = JSON.stringify(value[, replacer, space])
```

- `value` — 要编码的值。
- `replacer` — 要编码的属性数组或映射函数 `function(key, value)`。
- `space` — 用于格式化的空格数量

示例：

```js
var user = {
  name: 'O.O',
  age: 18
}

var json = JSON.stringify(user)
console.log(json) // "{"name": "O.O","age": 18}"
```

得到的 JSON 字符串是一个被称为 **JSON 编码（JSON-encoded）**或**序列化（serialized）**或**字符串化（stringified）** 或 **编组化（marshalled）** 的对象。

请注意，JSON 编码的对象与对象字面量有几个重要的区别：

- 字符串使用双引号。JSON 中没有单引号或反引号。所以 `'O.O'` 被转换为 `"O.O"`。
- 对象属性名也是双引号的，这是强制性的。所以 `age: 18` 被转换成 `"age": 18`。

`JSON.stringify` 也可以应用于原始（primitive）数据类型。

JSON 值支持以下数据类型：

- 引用类型的 `Object` 和 `Array`
- 原始类型的 `string`、`number`、`boolean` 和 `null`

```js
// 数字在 JSON 还是数字
console.log(JSON.stringify(1)) // 1

// 字符串在 JSON 中还是字符串，只是被双引号扩起来
console.log(JSON.stringify('test')) // "test"
console.log(JSON.stringify(true)) // true
console.log(JSON.stringify([1, 2, 3])) // [1, 2, 3]
```

JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 `JSON.stringify` 跳过。

例如：

- 函数/方法
- Symbol、bigInt 类型的属性
- 存储 `undefined` 的属性
- 以及一些特殊的对象类型：例如 `set`、`map` 等。

```js
let user = {
  sayHi() {
    // 被忽略
    console.log('Hello World')
  },
  [Symbol('id')]: 123, // 被忽略
  something: undefined // 被忽略
}

console.log(JSON.stringify(user)) // {}（空对象）
```

#### `replacer` 参数

大部分情况，`JSON.stringify` 仅与第一个参数一起使用。但是，如果我们需要微调替换过程，比如过滤掉循环引用，我们可以使用 `JSON.stringify` 的第二个参数。

作为函数：

```js
function replacer(key, value) {
  // 筛选出属性
  if (typeof value === 'string') {
    return undefined
  }
  return value
}

let foo = {
  foundation: 'Mozilla',
  model: 'box',
  week: 45,
  transport: 'car',
  month: 7
}

JSON.stringify(foo, replacer)
// '{"week":45,"month":7}'
```

作为数组：

```js
JSON.stringify(foo, ['week', 'month'])
// '{"week":45,"month":7}', only keep "week" and "month" properties
```

#### 格式化：`space` 参数

`JSON.stringify(value, replacer, spaces)` 的第三个参数是用于优化格式的空格数量。

这里的 `space = 2` 告诉 JavaScript 在多行中显示嵌套的对象，对象内部缩紧 2 个空格：

```json
let user = {
  name: "O.O",
  age: 18,
  address: {
    street: 'xxx-xxx'
  }
}

console.log(JSON.stringify(user, null, 2))
/*
  {
    "name": "O.O",
    "age": 18,
    "address": {
      "street": "xxx-xxx"
    }
  }
*/
```

`spaces` 参数仅用于日志记录和美化输出。

#### 自定义 toJSON

像 `toString` 进行字符串转换，对象也可以提供 `toJSON` 方法来进行 JSON 转换。如果可用，`JSON.stringify` 会自动调用它。

```js
let user = {
  age: 18,
  toJSON() {
    return this.age
  }
}

let editor = {
  name: 'li',
  user
}

console.log(JSON.stringify(user)) // 18
console.log(JSON.stringify(editor)) // {"name":"li","user":18}
```

`toJSON` 既可以用于直接调用 `JSON.stringify(user)` 也可以用于当 `user` 嵌套在另一个编码对象中。

### `JSON.parse` 方法

JSON 通常用于与服务端交换数据。在接收服务器数据时一般是字符串。

[`JSON.parse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) 方法将数据转换为 JavaScript 对象。

```js
let value = JSON.parse(str, [reviver])
```

- `str` — 要解析的 JSON 字符串。
- `reviver` — 可选的函数 `function(key, value)`，该函数将为每个 `(key, value)` 对调用，并可以对值进行转换。

示例：

```js
JSON.parse('{}') // {}
JSON.parse('true') // true
JSON.parse('"foo"') // "foo"
JSON.parse('[1, 5, "false"]') // [1, 5, "false"]
JSON.parse('null') // null
```

对于对象：

```js
var userData = '{ "name": "O.O", "age": 18 }'

var user = JSON.parse(userData)

console.log(user) // {name: "O.O", age: 18}
```

#### `reviver` 参数

```js
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}'

let meetup = JSON.parse(str, function (key, value) {
  if (key == 'date') return new Date(value)
  return value
})

console.log(meetup.date.getDate())
```

上面代码中，将字符串对象**反序列（deserialize）**为 JavaScript 对象，使用 `JSON.parse` 的第二个参数，返回一个 Date 对象，供后面调用。

## 总结

- JSON 是一种数据格式，具有自己的独立标准和大多数编程语言的库。
- JSON 支持 `object`、`array`、`string`、`number`、`boolean` 和 `null` 作为其值。
- JavaScript 提供序列化（serialize）成 JSON 的方法 `JSON.stringify` 和解析 JSON 的方法 `JSON.parse`。
- 这两种方法都支持用于智能读/写的转换函数。
- 如果一个对象具有 `toJSON`，那么它会被 `JSON.stringify` 调用。

## 更多资料

- [What is the correct JSON content type?](https://stackoverflow.com/questions/477816/what-is-the-correct-json-content-type)
- [JSON 方法，toJSON](https://zh.javascript.info/json#pai-chu-he-zhuan-huan-replacer)
- [数据类型和 JSON 格式](http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html)
