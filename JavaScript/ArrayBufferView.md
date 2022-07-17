# ArrayBufferView

`ArrayBufferView` 是 `ArrayBuffer` 的一部分。

它有一个偏移量和一个长度。

创建后，它提供 3 个只读属性：

- `buffer` 指向原始的 ArrayBuffer
- `byteOffset` 是该缓冲区的偏移量
- `byteLength` 是其内容的长度（以字节为单位）

类型化数组（`Typed Arrays`） 和 `DataView` 是 `ArrayBufferView` 的实例。
