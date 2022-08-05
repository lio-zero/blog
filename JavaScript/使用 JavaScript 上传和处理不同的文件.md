# 使用 JavaScript 上传和处理不同的文件

在本文中，我们将研究带有 `#upload` ID 的 `form` 元素。

## JSON 文件

它包含一个字段 `#file`，带有一种 `type` 类型。使用 `file` 类型的字段可以指定 `accept` 参数，并使用逗号分隔的接受文件类型列表。出于我们的目的，我们将把上传限制为 `.json` 文件。

```html
<form id="upload">
  <label for="file">文件上传</label>
  <input type="file" id="file" accept=".json" />
  <button>上传</button>
</form>
```

我将使用一个简单的 `userInfo.json` 文件进行测试。

```json
{
  "name": "O.O",
  "age": "18",
  "hobby": ["Eat", "sleep", "programming"]
}
```

### 监听上传

首先，我们将使用一些 DOM 操作基础知识来检测用户何时提交文件。

我们将使用 `document.querySelector()` 方法获取`#upload` 和 `#file` 元素，并将它们分别保存到 `form` 和 `file` 变量中。

```js
let form = document.querySelector('#upload')
let file = document.querySelector('#file')
```

然后，我们将使用 `Element.addEventListener()` 方法来监听 `form` 元素上的 `submit` 事件。我们将使用 `handleSubmit()` 函数作为回调函数。

```js
form.addEventListener('submit', handleSubmit)
```

在 `handleSubmit()` 函数内部，我们要做的第一件事是 `event.preventDefault()` 阻止表单重新加载页面。

```js
/**
 * 处理提交事件
 * @param  {Event} event 事件对象
 */
function handleSubmit(event) {
  // 阻止表单重新加载页面
  event.preventDefault()
}
```

接下来，我们将检查 `file` 字段是否有要使用该文件处理的实际文件 `file.value` 属性，然后检查其 `length` 属性。

如果没有文件，我们将使用 `return` 操作符结束回调函数。

```js
function handleSubmit(event) {
  event.preventDefault()

  // 如果没有文件，什么都不要做
  if (!file.value.length) return
}
```

现在，我们已经准备好上传文件了。

### 使用 JavaScript 上传和处理 JSON 文件

FileReader API 是一组异步方法，允许您处理和读取文件内容。

我们要做的第一件事是使用 `new FileReader()` 构造函数创建一个新的 FileReader 实例，并将其分配给 `reader` 变量。

```js
function handleSubmit(event) {
  event.preventDefault()
  if (!file.value.length) return

  // 创建新的 FileReader() 对象
  let reader = new FileReader()
}
```

要真正读取文件，我们可以使用 `FileReader.readAsText()` 方法。

我们在 `reader` 上调用它，并将文件作为参数传递给 `reader`。我们可以使用文件字段上的 `files` 属性访问该文件。这将返回一个数组，因为 `[type="file"]` 可以支持多个文件。

我们将使用括号表示法获取第一个（在本例中是唯一的）文件。

```js
function handleSubmit(event) {
  event.preventDefault()
  if (!file.value.length) return
  let reader = new FileReader()

  // 读取文件
  reader.readAsText(file.files[0])
}
```

这个 API 是异步的，所以我们需要将 `onload` 事件处理程序附加到 `reader` 对象。每当 `reader` 读取文件时，都会运行该命令。

这需要在我们真正读取文件之前声明。

为了让我们的代码更有条理，我们将使用一个命名函数：`logFile()`。我们不希望它立即运行，所以在将其分配给事件时，我们不使用括号（`()`）。

```js
function handleSubmit(event) {
  event.preventDefault()
  if (!file.value.length) return
  let reader = new FileReader()

  // 将回调事件设置为在读取文件时运行
  reader.onload = logFile

  reader.readAsText(file.files[0])
}
```

`logFile()` 函数接收来自 FileReader 对象的隐式 `event` 参数，因此我们将其添加为参数。

`event.target.result` 属性将是上传的 JSON 文件的字符串化版本。我们可以将它传递给`JSON.parse()`方法以从它获取 JSON 对象。

出于我们的目的，我们将记录字符串和解析的 JSON。您可能希望在应用程序中使用 JSON 文件的属性，将其保存到 `localStorage`，或者对其执行其他操作。

```js
function logFile(event) {
  let str = event.target.result
  let json = JSON.parse(str)
  console.log('string', str)
  console.log('json', json)
}
```

现在，每当用户提交 JSON 文件时，我们的代码都会读取它并将其记录到控制台。

## 图像文件

### 更新我们的表单

对于本文，我们将更改 `[type="file"]` 输入接受的文件类型。

我们想上传图像文件，所以我们将使用 `image/*` 作为 `accept` 的值。如果愿意，您可以将特定的文件扩展名指定为逗号分隔的列表，例如 `.png, .jpg`。

```html
<form id="upload">
  <label for="file">文件上传</label>
  <input type="file" id="file" accept="image/*" />

  <button>上传</button>
</form>
```

对于演示，我还想在 UI 中显示图像，因此我将添加一个 `#app` 元素，我们可以将图像渲染到其中。

```html
<div id="app"></div>
```

### 使用 JavaScript 上传图片

首先，我们要更新我们的 `handleSubmit()` 函数。

我们将不使用 `FileReader.readAsText()` 方法，而是使用 `FileReader.readAsDataURL()` 方法，再次传入文件。这将把文件作为 base64 编码的 [Data URL](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/Data%20URL.md) 进行处理。

```js
function handleSubmit(event) {
  event.preventDefault()
  if (!file.value.length) return
  let reader = new FileReader()
  reader.onload = logFile
  reader.readAsDataURL(file.files[0])
}
```

接下来，我们将使用 `document.querySelector()` 方法获取 `#app` 元素并将其保存到变量中。

```js
let app = document.querySelector('#app')
```

在 `logFile()` 事件处理程序函数中，`event.target.result` 是图像文件的 Data URL 字符串。您可以将其保存到 `localStorage`，将其发送到服务器等等。

因为我们想在 UI 中显示它，所以我们将使用 `document.createElement()` 方法来创建一个 `img` 元素。然后，我们将从方法返回 `event.target.result` 的 `FileReader.readAsDataURL()` 设置为 `src` 属性。

最后，我们将使用 `Node.append()` 方法将其注入 DOM。

```js
function logFile(event) {
  let str = event.target.result
  let img = document.createElement('img')
  img.src = str
  app.append(img)
  console.log(str)
}
```

## 更多资料

[前端文件上传](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%89%8D%E7%AB%AF%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0.md)
