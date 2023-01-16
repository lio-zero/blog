# HTML 的实体编码（HTML Entity Encode）

[HTML 实体](https://developer.mozilla.org/zh-CN/docs/Glossary/Entity)编码是将特殊字符编码为一个符号，这样就可以在 HTML 中使用这些字符而不会影响文档的结构。

例如：

- 不可分的空格 — `&nbsp;`
- `<`（小于符号） — `&lt;`
- `>`（大于符号） — `&gt;`
- `&`（与符号） — `&amp;`
- `"`（双引号） — `&quot;`
- `'`（单引号） — '`&apos;`
- `©`（版权符号）— `&copy;`

以上列出的一些实体比较容易记忆，但有一些不容易记住的你可以查看 WHATWG 提供的[字符实体的官方列表](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references)或使用[解码工具](https://mothereff.in/html-entities)。

HTML 实体是一段以连字符号（`&`）开头，以分号（`;`）结尾的字符串。用以显示不可见字符及保留字符（如 HTML 标签）

在前端，一般为了避免 [XSS 攻击](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20XSS%20%E6%94%BB%E5%87%BB%EF%BC%9F.md)，会将 `<>` 编码为 `&lt;` 与 `&gt;`，这些就是 HTML 实体编码。

在 HTML 转义时，仅仅只需要对六个字符进行编码：`&`、`<`、`>`、`"`、`'` 以及 **`**。我们可以使用 [he](https://www.npmjs.com/package/he) 或 [DOMPurify](https://github.com/cure53/DOMPurify) 库进行编码及转义。

```js
// 实体编码
he.encode('<img src=""></img>') // '&#x3C;img src=&#x22;&#x22;&#x3E;&#x3C;/img&#x3E;'

// 转义
he.escape('<img src=""></img>') // '&lt;img src=&quot;&quot;&gt;&lt;/img&gt;'
```
