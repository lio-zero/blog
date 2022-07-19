# Node.js Buffer 模块 — 缓冲区

## 什么是二进制代码？

为了理解缓冲区是什么以及计算机和二进制文件之间的关系，我们先来看看二进制代码的概念。

为了让计算机理解、处理和存储数据，必须将数据转换为二进制代码。这主要是因为计算机处理器是由晶体管组成的，它们是由开（1）和关（0）信号激活的电子机器。

发送到计算机的每一条数据在处理和输出结果之前，首先由微处理器转换成二进制。因此，能够区分不同的数据类型至关重要。通过一种称为编码的过程，计算机对不同的数据类型进行不同的编码，以区分不同类型的数据。

## 什么是 Node.js 缓冲区？

二进制流是大量二进制数据的集合。由于其庞大的规模，二进制流不会一起发送；在发送之前，它们被分解成更小的部分。

当数据处理单元不能接受更多数据流时，多余的数据将存储在缓冲区中，直到数据处理单元准备好接收更多数据。

## Node.js 中的缓冲区类

Node.js 服务器通常需要读写文件系统，当然，文件存储在二进制文件中。此外，Node.js 处理 TCP 流，它在以小块形式发送二进制数据之前保护与接收器的连接。

发送到接收器的数据流需要存储在某个地方（缓冲区），直到接收器准备好接收更多数据块进行处理。这就是 Node.js `Buffer` 类发挥作用的地方，它在 [V8 引擎](<https://en.wikipedia.org/wiki/V8_(JavaScript_engine)>) 之外处理和存储二进制数据。

以上概念讲完了，下面是 Node.js 的 [`Buffer` 模块](https://nodejs.org/api/buffer.html)的一些简单介绍。

## Buffer 属性和方法

Buffer 对象是 Node.js 中的全局对象，不需要使用 `require` 关键字导入。

例如：创建长度为 15 的空 Buffer：

```js
const buf = Buffer.alloc(15)
buf // <Buffer 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>
buf.length // 15
```

以下是一些属性和方法：

- `Buffer.alloc()` 创建指定长度的缓冲区对象。它以字节为单位分配缓冲区的大小。
- `Buffer.byteLength()` 返回指定对象中的字节数
- `Buffer.compare()` 比较两个缓冲区对象
- `Buffer.concat()` 将缓冲区对象数组连接到一个缓冲区对象中
- `Buffer.fill()` 用指定的值填充缓冲区对象
- `Buffer.from()` 从对象（字符串/数组/缓冲区）创建缓冲区对象
- `Buffer.isEncoding()` 检查缓冲区对象是否支持指定的编码
- `buf.entries()` 返回缓冲区对象的 index、byte 对的迭代器
- `buff.includes()` 检查缓冲区对象是否包含指定的值。如果存在匹配项，则返回`true`，否则返回 `false`
- `buf.slice()` 将一个缓冲区对象分割成一个新的缓冲区对象，从指定的位置开始和结束。
- `buf.readInt8()` 从缓冲区对象读取 8 位整数
- `buf.writeFloatBE()` 使用 big-endian 将指定的字节写入缓冲区对象。字节应为 32 位浮点。
- `buf.length` 返回缓冲区对象的长度，以字节为单位
- etc

详细示例请查阅[文档](https://nodejs.org/api/buffer.html#buffer)。

> **扩展**：Node 全局对象有 `process`、`console`、`Buffer`、`global`、EventLoop 相关 API（`setImmediate`、`setInterval` 和 `setTimeout` 等）及为模块包装所使用的全局对象（`exports`、`module`、`require` 等），它们都不需要您使用 `require()` 即可在 Node 环境中使用。

## 更多资料

- [Node.js 缓冲区介绍](https://livecodestream.dev/post/2020-06-06-a-complete-introduction-to-node-buffers/)
- [在 Nodejs 中将 Buffer 转换为 JSON 和 Utf8 字符串](https://github.com/lio-zero/blog/blob/master/Node/%E5%9C%A8Node.js%20%E4%B8%AD%E5%B0%86%20Buffer%20%E8%BD%AC%E6%8D%A2%E4%B8%BA%20JSON%20%E5%92%8C%20Utf8%20%E5%AD%97%E7%AC%A6%E4%B8%B2.md)
- [Node.js Buffer(缓冲区)](https://www.runoob.com/nodejs/nodejs-buffer.html)
- [Buffer](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#buffer)
