# DataView 对象

[`DataView`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DataView) 是 `ArrayBuffer` 的视图，与[类型化数组（Typed Arrays）](https://github.com/lio-zero/blog/blob/main/JavaScript/%E7%B1%BB%E5%9E%8B%E5%8C%96%E6%95%B0%E7%BB%84.md)类似，但在这种情况下，数组中的项可以具有不同的大小和类型。

这是一个例子：

```js
const buffer = new ArrayBuffer(64)
const view = new DataView(buffer)
```

由于这是缓冲区上的视图，我们可以指定要从哪个字节开始，以及长度：

```js
const view = new DataView(buffer, 10) // 从字节 10 开始
const view = new DataView(buffer, 10, 30) // 从字节 10 开始，添加 30 项
```

如果我们不添加这些附加参数，视图将从位置 0 开始，并加载缓冲区中存在的所有字节。

我们可以使用一组方法将数据添加到缓冲区中：

- `setInt8()`
- `setInt16()`
- `setInt32()`
- `setUint8()`
- `setUint16()`
- `setUint32()`
- `setFloat32()`
- `setFloat64()`

以下是如何调用其中一种方法：

```js
const buffer = new ArrayBuffer(64)
const view = new DataView(buffer)
view.setInt16(0, 2019)
```

默认情况下，使用**大字节序（Big-Endian）**存储数据。您可以通过添加第三个具有 `true` 值的参数来覆盖此设置并使用小字节序（Little-Endian）：

```js
const buffer = new ArrayBuffer(64)
const view = new DataView(buffer)
view.setInt16(0, 2019, true)
```

以下是我们如何从视图中获取数据：

- `getInt8()`
- `getInt16()`
- `getInt32()`
- `getUint8()`
- `getUint16()`
- `getUint32()`
- `getFloat32()`
- `getFloat64()`

例子：

```js
const buffer = new ArrayBuffer(64)
const view = new DataView(buffer)
view.setInt16(0, 2019)
view.getInt16(0) // 2019
```

由于 `DataView` 是 `ArrayBufferView`，因此我们有以下 3 个只读属性：

- `buffer` 指向原始的 ArrayBuffer
- `byteOffset` 是该缓冲区的偏移量
- `byteLength` 是其内容的长度（以字节为单位）

需要记住的一点是，类型化数组不允许我们控制字节序：**它使用系统的字节序**。一般来说，这很好，因为我们所说的主要用例是使用一个多媒体 API 在本地使用数组。

如果您在另一个系统上传输类型化数组的数据，如果它使用大字节序而您使用小字节序，则数据可能不会被错误编码。

如果您需要这种控制，**DataView** 是一个完美的选择。
