# ArrayBuffer

正如 Blob 是磁盘上可用数据的不透明表示一样，[`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 是内存中可用字节的不透明表示。

`ArrayBuffer` 是 JavaScript 中的一种数据类型，它表示一段连续的内存空间，可以用来存储二进制数据。

其构造函数接受一个参数，以字节为单位的长度：

```js
const buffer = new ArrayBuffer(64)
```

`ArrayBuffer` 值具有一个（只读）属性：`byteLength`，顾名思义，它以字节为单位表示其长度。

```js
buffer.byteLength // 64
```

它还提供了一个 `slice()` 实例方法，可以从现有的 `ArrayBuffer` 创建一个新的 `ArrayBuffer`，取一个起始位置和一个可选的长度：

```js
const buffer = new ArrayBuffer(64)
const newBuffer = buffer.slice(32, 8)
```

`ArrayBuffer` 本身并不能直接读写数据，必须使用类型化数组或 `DataView` 对象来对 `ArrayBuffer` 进行操作。

例如，下面的代码创建了一个 8 字节的 `ArrayBuffer`，并使用 `Uint8Array` 类型化数组来读写数据：

```js
const buffer = new ArrayBuffer(8)
const view = new Uint8Array(buffer)

view[0] = 10
view[1] = 20
console.log(view[0], view[1]) // 10 20
```

可以使用类型化数组或 `DataView` 对象来读写 `ArrayBuffer` 中的各种数据类型，例如整数、浮点数和字符串。`ArrayBuffer` 还可以用于在不同的 JavaScript 环境之间传输二进制数据，例如在 Web Worker 和主线程之间传输数据。

`ArrayBuffer` 和类型化数组是 JavaScript 中用于处理二进制数据的重要工具，可以在处理文件、网络请求和其他二进制数据时使用。

## 从互联网上以 `ArrayBuffer` 的形式下载数据

我们可以从互联网上下载一个 blob，并使用 XHR 将其存储到 `ArrayBuffer` 中：

```js
const downloadBlob = (url, callback) => {
  const xhr = new XMLHttpRequest()
  xhr.open('GET', url)
  xhr.responseType = 'arraybuffer'

  xhr.onload = () => {
    callback(xhr.response)
  }

  xhr.send(null)
}
```

## 更多资料

- [ECMAScript 6 入门：ArrayBuffer](https://es6.ruanyifeng.com/#docs/arraybuffer)
- [ArrayBuffer，二进制数组](https://zh.javascript.info/arraybuffer-binary-arrays)
