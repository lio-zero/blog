# encodeURI 与 encodeURIComponent 的区别

由于 URL 只能由标准 ASCII 字符组成，因此必须对其他特殊字符进行编码。它们将被代表其 UTF-8 编码的不同字符序列替换。

[`encodeURI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) 和 [`encodeURIComponent`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) 用于此目的。

## 区别

`encodeURI` 用于对完整 URL 进行编码。

```js
encodeURI('https://example.com/path to a document.pdf')

// 空格 -> %20
// 'https://example.com/path%20to%20a%20document.pdf'
```

而 `encodeURIComponent` 用于编码 URI 部分，例如查询字符串。

```js
;`http://example.com/?search=${encodeURIComponent('encode & decode param')}`

// & -> %26
// 'http://example.com/?search=encode%20%26%20decode%20param'
```

有 11 个字符不是由 `encodeURI` 编码的，而是由 `encodeURIComponent` 编码的。

以下方法打印这些字符：

```js
const arr = Array(256)
  .fill(0)
  .map((_, i) => String.fromCharCode(i))
  .filter((c) => encodeURI(c) != encodeURIComponent(c))

arr.forEach((c) => console.log(c, encodeURI(c), encodeURIComponent(c)))
```

以下是这些字符的列表：

| 字符 | `encodeURI` | `encodeURIComponent` |
| :--- | :---------- | :------------------- |
| `#`  | `#`         | `%23`                |
| `$`  | `$`         | `%24`                |
| `&`  | `&`         | `%26`                |
| `+`  | `+`         | `%2B`                |
| `,`  | `,`         | `%2C`                |
| `/`  | `/`         | `%2F`                |
| `:`  | `:`         | `%3A`                |
| `;`  | `;`         | `%3B`                |
| `=`  | `=`         | `%3D`                |
| `?`  | `?`         | `%3F`                |
| `@`  | `@`         | `%40`                |

另外，`decodeURI` 和 `decodeURIComponent` 是分别对 `encodeURI` 和 `encodeURIComponent` 编码的字符串进行解码的方法。

`encodeURIComponent` 不编码 `-_.!~*'()`。如果要对这些字符进行编码，必须将其替换为相应的 UTF-8 字符序列：

```js
const encode = (str) =>
  encodeURIComponent(str)
    .replace(/\-/g, '%2D')
    .replace(/\_/g, '%5F')
    .replace(/\./g, '%2E')
    .replace(/\!/g, '%21')
    .replace(/\~/g, '%7E')
    .replace(/\*/g, '%2A')
    .replace(/\'/g, '%27')
    .replace(/\(/g, '%28')
    .replace(/\)/g, '%29')

encode("What's result of (4 + 2)?") // "What%27s%20result%20of%20%284%20%2B%202%29%3F"
```

解码方法可能如下所示：

```js
const decode = (str) =>
  decodeURIComponent(
    str
      .replace(/\%2D/g, '-')
      .replace(/\%5F/g, '_')
      .replace(/\%2E/g, '.')
      .replace(/\%21/g, '!')
      .replace(/\%7E/g, '~')
      .replace(/\%2A/g, '*')
      .replace(/\%27/g, "'")
      .replace(/\%28/g, '(')
      .replace(/\%29/g, ')')
  )

decode('What%27s%20result%20of%20%284%20%2B%202%29%3F') // "What's result of (4 + 2)?"
```
