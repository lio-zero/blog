# 创建 GUID/UUID

根据 [RFC 4122](https://www.ietf.org/rfc/rfc4122.txt)，UUID（通用唯一标识符）也称为 GUID（全局唯一标识符），是设计用于提供特定唯一性保证的标识符。

虽然可以在几行 JavaScript 代码中实现符合 RFC 兼容的 UUID，但有几个常见的陷阱：

- 无效的 ID 格式（UUID 的格式必须为 `xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx`，其中 x 是 [0-9] 或 [a-f] 之一，M 是 [1-5] 之一，N 是 [8、9、a 或 b]。
- 使用低质量的随机源（例如 `Math.random`）

因此，鼓励为生产环境编写代码的开发人员使用严格的、维护良好的实现，例如 [`uuid` 模块](https://github.com/uuidjs/uuid)。它还有另外一个竞品 [NanoID](https://www.npmjs.com/package/nanoid)，很多用过的都说没有明显的缺点或限制。

另外，Node.js 中的 `crypto` 模块有一个 [`randomUUID()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) 方法可以生成 UUID。它是一个新的标准，且受到越来越多的[浏览器支持](https://caniuse.com/mdn-api_crypto_randomuuid)。

```js
const crypto = require('crypto')

crypto.randomUUID() // bb2c6a75-b373-4c24-a3c3-1fb0911a8a25
```

因为它是一个加密安全的标识符，所以需要 SSL 证书才能运行。但是，对于测试而言，`localhost://` 和 `file://` 除外。

如果这两种方法都不适用于您，还有以下最原始的方法：

```js
function uuidv4() {
  return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  )
}

uuidv4() // 'ab399796-c3b4-4329-903a-4db62af83a17'
```

但强烈建议不要使用任何依赖 `Math.random()` 的 UUID 生成器。基于 `Math.random()` 的解决方案不能提供良好的唯一性保证。[点击此处查看详情](https://bocoup.com/blog/random-numbers)

## 更多资料

- [How to create a GUID / UUID](https://stackoverflow.com/questions/105034/how-to-create-a-guid-uuid)
- [Collisions when generating UUIDs in JavaScript](https://stackoverflow.com/questions/6906916/collisions-when-generating-uuids-in-javascript)
- [Why is NanoID Replacing UUID?](https://blog.bitsrc.io/why-is-nanoid-replacing-uuid-1b5100e62ed2)
- [UUID vs Crypto.randomUUID vs NanoID](https://medium.com/@matynelawani/uuid-vs-crypto-randomuuid-vs-nanoid-313e18144d8c)
