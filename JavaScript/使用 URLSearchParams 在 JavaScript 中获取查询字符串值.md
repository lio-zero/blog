# 使用 URLSearchParams 在 JavaScript 中获取查询字符串值

[wiki](https://en.wikipedia.org/wiki/URL#Syntax) 有一个不错的 URL 语法图，如下：

![URL 语法图](https://upload-images.jianshu.io/upload_images/18281896-8a7f161f6ae05c0c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它包含 URL 具体的构成，你可以先点击链接，了解它们各部分的含义，在继续往下阅读（也可以跳过）。

HTTP 协议允许使用查询字符串向网页发出请求。

像这样：

```txt
https://example.com/?name=O.O
https://example.com/post?name=O.O
```

在这种情况下，我们有一个名为 `name` 的查询参数， 其值为 `O.O`。

你可以有多个参数，如下所示：

```txt
https://example.com/post?name=O.O&age=20
```

为了使用 JavaScript 访问浏览器内的查询值，我们有一个名为 [`URLSearchParam`](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams) 的特殊 API，除了 IE，所有现代浏览器都支持该 API：

![URLSearchParam API 支持情况](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a31eaba3661b4b0ca02e0728a9f8f80b~tplv-k3u1fbpfcp-watermark.image?)

用法：

```js
const params = new URLSearchParams(window.location.search)
```

**注意**：不要将完整的 URL 作为参数传递给 `URLSearchParams()`，而只传递 URL 的查询字符串部分，你可以使用 `window.location.search`。

例如：

```txt
https://example.com/post?name=O.O
```

`window.location.search` 等于字符串 `?name=O.O`。

现在你有了 `params` 对象，你可以查询它了。

你可以检查是否传递了参数：

```js
params.has('name')
```

你可以获取参数的值：

```js
params.get('name')
```

你可以迭代所有参数，使用 `for...of`：

```js
const params = new URLSearchParams(window.location.search)

for (const param of params) {
  console.log(param)
}
```

一个参数可以有多个值。

在这种情况下，我们多次传递相同的参数名，如下所示：

```txt
https://example.com/post?name=D.O&name=O.O
```

我们无法检测一个参数是否被多次传递。如果我们使用 `params.get('name')`，我们只会得到第一个值。

我们可以使用 `params.getAll('name')` 返回一个包含所有传递值的数组。

除了 `has()`、`get()` 和 `getAll()`，URLSearchParams API 还提供了几种其他方法，我们可以使用这些方法循环参数：

- `forEach()` 迭代参数
- `entries()` 返回包含参数键/值的迭代器
- `keys()` 返回包含参数键的迭代器
- `values()` 返回包含参数值的迭代器

更改参数的其他方法，用于页面中运行的其他 JavaScript（它们不会更改 URL）：

- `append()` 将新参数附加到对象
- `delete()` 删除现有参数
- `set()` 设置参数的值

我们还使用 `sort()` 按键值对参数进行排序，使用 `toString()` 方法从值生成查询字符串。

我们可以使用 `append()`、`.delete()` 和 `set()` 来编辑查询字符串，并使用 `toString()` 生成一个新的查询字符串。

## 示例：生成一个包含当前 URL 参数的对象

使用正则匹配获取查询字符串值：

```js
const getURLParameters = (url) =>
  (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
    (acc, cur) => (
      (acc[cur.slice(0, cur.indexOf('='))] = cur.slice(cur.indexOf('=') + 1)),
      acc
    ),
    {}
  )

getURLParameters('google.com') // {}
getURLParameters('https://example.com?name=O.O&age=18') // {name: 'O.O', age: '18'}
```

使用 `URLSearchParams` 查询字符串值：

```js
const queryStringToObject = (url) =>
  [...new URLSearchParams(url.split('?')[1])].reduce(
    (a, [k, v]) => ((a[k] = v), a),
    {}
  )

queryStringToObject('https://google.com?page=1&count=10') // {page: '1', count: '10'}
```

你可以编写两个方法来在查询字符串和查询对象之间相互转换：

```js
function objectToParams(object) {
  const params = new URLSearchParams()
  Object.entries(object).map((entry) => {
    let [key, value] = entry
    params.set(key, value)
  })
  return params.toString()
}

function paramsToObject(paramString) {
  const params = new URLSearchParams(paramString)
  const object = {}
  for (const [key, value] of params.entries()) {
    object[key] = value
  }
  return object
}

// URL 示例：https://example.com/post?name=O.O
// window.location.search = ?name=O O
const params = paramsToObject('?name=O.O') // {name: 'O.O'}
objectToParams(params) // 'name=O O'
```

记住，它不支持 IE 的任何版本，且 `URLSearchParams` 没有在最后一个字符串前加上 `?` 字符，因此需要手动将其添加到返回的字符串和主 URL 之间。

在这之后，你可以配合 `encodeURI()` 和 `encodeURIComponent()` 对 URL 的特殊字符进行编码。

> 推荐：[encodeURI 与 encodeURIComponent 的区别](https://github.com/lio-zero/blog/blob/main/JavaScript/encodeURI%20%E4%B8%8E%20encodeURIComponent%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)
>
> 更新：[URL API](https://github.com/lio-zero/blog/blob/main/Web%20API/URL%20API.md)
