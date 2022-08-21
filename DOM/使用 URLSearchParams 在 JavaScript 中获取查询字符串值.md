# 使用 URLSearchParams 在 JavaScript 中获取查询字符串值

在开始之前，这里插入一段来自 [wiki](https://en.wikipedia.org/wiki/URL#Syntax) 提供的一个非常不错的 URL 语法图：

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="1068px" height="100px" viewBox="0 0 1068 100" enable-background="new 0 0 1068 100" xml:space="preserve">
<line fill="none" stroke="#332900" x1="0" y1="17" x2="1068" y2="17"/>
<path fill="none" stroke="#332900" d="M153.65,17c6.5,0,10,3,10,10v12c0,6.5,3,10,10,10H619c6.5,0,10-3,10-10V27c0-6.5,3-10,10-10"/>
<path fill="none" stroke="#332900" d="M704.35,17c6.5,0,10,3,10,10v12c0,6.5,3,10,10,10h120c6.5,0,10-3,10-10V27c0-6.5,3-10,10-10"/>
<path fill="none" stroke="#332900" d="M865.35,17c6.5,0,10,3,10,10v12c0,6.5,3,10,10,10h143.5c6.5,0,10-3,10-10V27c0-6.5,3-10,10-10"/>
<path fill="none" stroke="#332900" d="M228.82,49c6.5,0,10,3,10,10v12c0,6.5,3,10,10,10h136c6.5,0,10-3,10-10V59c0-6.5,3-10,10-10"/>
<path fill="none" stroke="#332900" d="M470.82,49c6.5,0,10,3,10,10v12c0,6.5,3,10,10,10h108c6.5,0,10-3,10-10V59c0-6.5,3-10,10-10"/>
<polygon fill="#332900" stroke="#332900" points="8.5,17 0.5,13 0.5,21"/>
<polygon fill="#332900" stroke="#332900" points="16.5,17 8.5,13 8.5,21"/>
<polygon fill="#332900" stroke="#332900" points="1058,17 1050,13 1050,21"/>
<polygon fill="#332900" stroke="#332900" points="1067.5,21 1067.5,13 1059.5,17"/>
<a id="scheme" xlink:href="#scheme">
	<path fill="#332900" stroke="#332900" d="M41.5,2.5h50c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-50c-5.5,0-10-4.5-10-10v-12C31.5,7,36,2.5,41.5,2.5z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M40,0.5h50c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10H39.5c-5.5,0-10-4.5-10-10v-12C30,5,34,0.5,40,0.5z"/>
	<text transform="matrix(1 0 0 1 64 20)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">scheme</text>
</a>
<a id="scheme-delimiter" xlink:href="#scheme-delimiter">
	<path fill="#332900" stroke="#332900" d="M130,2.5h6c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-6c-5.5,0-10-4.5-10-10v-12C120,7,124,2.5,130,2.5z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M127.5,0.5h6c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-6c-5.5,0-10-4.5-10-10v-12C117.5,5,122,0.5,127.5,0.5z"/>
	<text transform="matrix(1 0 0 1 130 20)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">:</text>
</a>
<a id="slashes" xlink:href="#slashes">
	<path fill="#332900" stroke="#332900" d="M194,35h17c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-17c-5.5,0-10-4.5-10-10V45C184,39,189,35,194,35z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M192.35,32.75h17c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-17c-5.5,0-10-4.5-10-10v-12c-0.35-5.75,4.65-10,9.65-10H192.35z"/>
	<text transform="matrix(1 0 0 1 200 52)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">//</text>
</a>
<a id="userinfo" xlink:href="#userinfo">
	<rect x="260" y="67.5" fill="#332900" stroke="#332900" width="67" height="32"/>
	<rect x="258" y="65.5" fill="#FFEC9E" stroke="#332900" width="67" height="32"/>
	<text transform="matrix(1 0 0 1 291 86)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">userinfo</text>
</a>
<a id="at-sign" xlink:href="#at-sign">
	<path fill="#332900" stroke="#332900" d="M356,67.5h11c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-11c-5.5,0-10-4.5-10-10v-12C346,72,351,67.5,356,67.5z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M354,65.5h11c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-11c-5.5,0-10-4.5-10-10v-12C344,70,349,65.5,354,65.5z"/>
	<text transform="matrix(1 0 0 1 360 85)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">@</text>
</a>
<a id="host" xlink:href="#host">
	<rect x="416" y="35" fill="#332900" stroke="#332900" width="47" height="32"/>
	<rect x="414" y="33" fill="#FFEC9E" stroke="#332900" width="47" height="32"/>
	<text transform="matrix(1 0 0 1 437 53)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">host</text>
</a>
<a id="port-delimiter" xlink:href="#port-delimiter">
	<path fill="#332900" stroke="#332900" d="M512,67.5h5c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-5c-5.5,0-10-4.5-10-10v-12C502,72,507,67.5,512,67.5z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M510,65.5h5c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-5c-5.5,0-10-4.5-10-10v-12C500,70,505,65.5,510,65.5z"/>
	<text transform="matrix(1 0 0 1 512 85)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">:</text>
</a>
<a id="port" xlink:href="#port">
	<rect x="546" y="67.5" fill="#332900" stroke="#332900" width="46" height="32"/>
	<rect x="544" y="65.5" fill="#FFEC9E" stroke="#332900" width="46" height="32"/>
	<text transform="matrix(1 0 0 1 567 86)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">port</text>
</a>
<a id="path" xlink:href="#path">
	<rect x="650" y="2.5" fill="#332900" stroke="#332900" width="47" height="32"/>
	<rect x="648" y="0.5" fill="#FFEC9E" stroke="#332900" width="47" height="32"/>
	<text transform="matrix(1 0 0 1 672 21)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">path</text>
</a>
<a id="question-mark" xlink:href="#question-mark">
	<path fill="#332900" stroke="#332900" d="M746.5,34.75h7c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-7c-5.5,0-10-4.5-10-10v-12C736.5,39,741,34.75,746.5,34.75z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M744.5,32.75h7c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-7c-5.5,0-10-4.5-10-10v-12c0-5.75,4.5-10,10.5-10H744.5z"/>
	<text transform="matrix(1 0 0 1 748 53)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">?</text>
</a>
<a id="query" xlink:href="#query">
	<rect x="783" y="35" fill="#332900" stroke="#332900" width="53" height="32"/>
	<rect x="781" y="33" fill="#FFEC9E" stroke="#332900" width="53" height="32"/>
	<text transform="matrix(1 0 0 1 807 53)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">query</text>
</a>
<a id="hash" xlink:href="#hash">
	<path fill="#332900" stroke="#332900" d="M906.5,35h10c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-10c-5.5,0-10-4.5-10-10V44.75C896.5,39,901,35,906.5,35z"/>
	<path fill="#FFDB4D" stroke="#332900" d="M904.5,33h10c5.5,0,10,4.5,10,10v12c0,5.5-4.5,10-10,10h-10c-5.5,0-10-4.5-10-10V42.75C894.5,37,900,33,904.5,33z"/>
	<text transform="matrix(1 0 0 1 909 53)" fill="#141000" text-anchor="middle" font-weight="bold" font-family="'DejaVu Sans'" font-size="12px">#</text>
</a>
<a id="fragment" xlink:href="#fragment">
	<rect x="946" y="35" fill="#332900" stroke="#332900" width="76" height="32"/>
	<rect x="944" y="33" fill="#FFEC9E" stroke="#332900" width="76" height="32"/>
	<text transform="matrix(1 0 0 1 982 53)" fill="#1A1400" text-anchor="middle" font-family="'DejaVu Sans'" font-size="12px">fragment</text>
</a>
</svg>

您可以点击不同的节点，查看 URL 的变化。以了解 URL 具体的构成。

下面正文开始。

---

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

为了使用 JavaScript 访问浏览器内的查询值，我们有一个名为 [`URLSearchParam`](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams) 的特殊 API，除了 IE，所有现代浏览器都支持该 API：

![URLSearchParam API 支持情况](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a31eaba3661b4b0ca02e0728a9f8f80b~tplv-k3u1fbpfcp-watermark.image?)

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

您可以编写两个方法来在查询字符串和查询对象之间相互转换：

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

在这之后，您可以配合 `encodeURI()` 和 `encodeURIComponent()` 对 URL 的特殊字符进行编码。

> 推荐：[encodeURI 与 encodeURIComponent 的区别](https://github.com/lio-zero/blog/blob/main/JavaScript/encodeURI%20%E4%B8%8E%20encodeURIComponent%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)
