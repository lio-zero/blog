# 使用 URLSearchParams 在 JavaScript 中获取查询字符串值

HTTP 协议允许使用查询字符串向网页发出请求。

像这样：

```txt
https://example.com/?name=O.O
https://example.com/post?name=O.O
```

在这种情况下，我们有一个名为 `name` 的查询参数， 其值为 `O.O`。

您可以有多个参数，如下所示：

```txt
https://example.com/post?name=O.O&age=20
```

为了使用 JavaScript 访问浏览器内的查询值，我们有一个名为 URLSearchParam 的特殊 API，所有现代浏览器都支持该 API

![URLSearchParam API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-8358cdcb5757b115.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用法：

```js
const params = new URLSearchParams(window.location.search)
```

**注意**：不要将完整的 URL 作为参数传递给 `URLSearchParams()`，而只传递 URL 的查询字符串部分，您可以使用 `window.location.search`。

例如：

```txt
https://example.com/post?name=O.O
```

`window.location.search` 等于字符串 `?name=O.O`。

现在您有了 `params` 对象，您可以查询它了。

您可以检查是否传递了参数：

```js
params.has('name')
```

您可以获取参数的值：

```js
params.get('name')
```

您可以迭代所有参数，使用 `for...of`：

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

我们可以使用 `append()`、`.delete()`、`set()` 来编辑查询字符串，并使用 `toString()` 生成一个新的查询字符串。

## 示例：生成一个包含当前 URL 参数的对象

方法一：

```js
const getURLParameters = (url) =>
  (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
    (a, v) => (
      (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a
    ),
    {}
  )

getURLParameters('google.com') // {}
getURLParameters('https://example.com?name=O.O&age=18') // {name: 'O.O', age: '18'}
```

方法二：

```js
const queryStringToObject = (url) =>
  [...new URLSearchParams(url.split('?')[1])].reduce(
    (a, [k, v]) => ((a[k] = v), a),
    {}
  )

queryStringToObject('https://google.com?page=1&count=10') // {page: '1', count: '10'}
```
