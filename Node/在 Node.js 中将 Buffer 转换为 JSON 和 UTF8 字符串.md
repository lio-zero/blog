# 在 Node.js 中将 Buffer 转换为 JSON 和 UTF8 字符串

Nodejs 和基于浏览器的 JavaScript 有所不同，因为 Node 甚至在 ES6 草案提出 `ArrayBuffer` 之前就有处理二进制数据的方法。在 Node 中，`Buffer` 类是大多数 I/O 操作使用的主要数据结构。它是在 V8 堆外部分配的原始二进制数据，一旦分配，就无法调整大小。

在 Node v6.0 之前，要创建新的 Buffer，我们只需使用 `new` 关键字调用构造函数：

```js
let newBuff = new Buffer('New String')
```

v6.0 之后，要创建新的 Buffer 实例：

```js
let newBuff = Buffer.from('New String')
```

`new Buffer()` 构造函数已被弃用，并被单独的 `Buffer.from()`、`Buffer.alloc()` 和 `Buffer.allocUnsafe()` 方法替换。

> 更多详细信息可以阅读[**官方文档**](https://nodejs.org/api/buffer.html)。

## 将 Buffer 转换为 JSON

Buffer 可以转换为 JSON。

```js
let bufferOne = Buffer.from('All work and no play makes Jack a dull boy')
console.log(bufferOne)

// <Buffer 41 6c 6c 20 77 6f 72 6b 20 61 6e 64 20 6e 6f 20 70 6c 61 79 20 6d 61 6b 65 73 20 4a 61 63 6b 20 61 20 64 75 6c 6c 20 62 6f 79>

let json = JSON.stringify(bufferOne, null, 2)
console.log(json)
/*
{
  "type": "Buffer",
  "data": [
    65,
    108,
    108,
    32,
    119,
    111,
    114,
    107,
    32,
    97,
    110,
    100,
    32,
    110,
    111,
    32,
    112,
    108,
    97,
    121,
    32,
    109,
    97,
    107,
    101,
    115,
    32,
    74,
    97,
    99,
    107,
    32,
    97,
    32,
    100,
    117,
    108,
    108,
    32,
    98,
    111,
    121
  ]
}
*/
```

JSON 指定要转换的对象的类型是 `Buffer` 及其数据。

### 将 JSON 转换为 Buffer

```js
let bufferOriginal = Buffer.from(JSON.parse(json).data)
console.log(bufferOriginal)

// <Buffer 41 6c 6c 20 77 6f 72 6b 20 61 6e 64 20 6e 6f 20 70 6c 61 79 20 6d 61 6b 65 73 20 4a 61 63 6b 20 61 20 64 75 6c 6c 20 62 6f 79>
```

## 将 Buffer 转换为 UTF-8 字符串

```js
console.log(bufferOriginal.toString('utf8')) // All work and no play makes Jack a dull boy
```

`.toString()` 不是将 Buffer 转换为字符串的唯一方法。此外，默认情况下，它会转换为 utf-8 格式字符串。

另一种将 Buffer 转换为字符串的方法是使用 Node.js API 中的 `StringDecoder` 核心模块。

<!-- https://masteringjs.io/tutorials/node/buffer
https://masteringjs.io/tutorials/node/buffer-to-string
https://masteringjs.io/tutorials/node/buffer-length
https://masteringjs.io/tutorials/node/buffer-compare -->
