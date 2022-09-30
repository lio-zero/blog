# ArrayBuffer

正如 Blob 是磁盘上可用数据的不透明表示一样，[`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 是内存中可用字节的不透明表示。

构造函数接受一个参数，以字节为单位的长度：

```js
const buffer = new ArrayBuffer(64)
```

ArrayBuffer 值具有一个（只读）属性：`byteLength`，顾名思义，它以字节为单位表示其长度。

它还提供了一个 `slice()` 实例方法，可以从现有的 `ArrayBuffer` 创建一个新的 ArrayBuffer，取一个起始位置和一个可选的长度：

```js
const buffer = new ArrayBuffer(64)
const newBuffer = buffer.slice(32, 8)
```

## 从互联网上以 ArrayBuffer 的形式下载数据

我们可以从互联网上下载一个 blob，并使用 XHR 将其存储到 ArrayBuffer 中：

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

[ArrayBuffer，二进制数组](https://zh.javascript.info/arraybuffer-binary-arrays)
