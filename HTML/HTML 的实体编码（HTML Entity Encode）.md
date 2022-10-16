# HTML 的实体编码（HTML Entity Encode）

> MDN：[HTML 实体](https://developer.mozilla.org/zh-CN/docs/Glossary/Entity)是一段以连字号（`&`）开头、以分号（`;`）结尾的文本（字符串）。实体常常用于显示保留字符（这些字符会被解析为 HTML 代码）和不可见的字符（如“不换行空格”）。你也可以用实体来代替其他难以用标准键盘键入的字符。

- 不可分的空格：`&nbsp;`
- `<`（小于符号）：`&lt;`
- `>`（大于符号）：`&gt;`
- `&`（与符号）：`&amp;`
- `"`（双引号）：`&quot;`
- `'`（单引号）：'`&apos;`
- `©`（版权符号）`&copy;`

以上列出的一些实体比较容易记忆，但有一些不容易记住的您可以查看 [whatwg](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references) 或使用[解码工具](https://mothereff.in/html-entities)。

HTML 实体是一段以连字符号（`&`）开头、以分号（`;`）结尾的字符串。用以显示不可见字符及保留字符（如 HTML 标签）

在前端，一般为了避免 XSS 攻击，会将 `<>` 编码为 `&lt;` 与 `&gt;`，这些就是 HTML 实体编码。

在 HTML 转义时，仅仅只需要对六个字符进行编码：`&`、`<`、`>`、`"`、`'` 和 `。我们可以使用 [he](https://www.npmjs.com/package/he) 库进行编码及转义。

```js
// 实体编码
he.encode('<img src=""></img>') // "&#x3C;img src=&#x22;&#x22;&#x3E;&#x3C;/img&#x3E;"

// 转义
he.escape('<img src=""></img>') // "&lt;img src=&quot;&quot;&gt;&lt;/img&gt;"
```
