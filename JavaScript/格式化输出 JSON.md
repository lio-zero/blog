# 格式化输出 JSON

`JSON.stringify()` 是将 JavaScript 对象转换为 JSON 的规范方法。有许多 JavaScript 框架在内部都使用了`JSON.stringify()`，比如 Express 框架的 `res.json()` 和 Axios body 序列化。

但是，默认情况下，`JSON.stringify()` 输出的 JSON 格式，不带空格或颜色。在后面，我们将使用一个常用的 npm 包来丰富输出数据的色彩。

`JSON.stringify()` 基本用法如下：

```js
const think = { eat: '🥩', sleep: '😴' }

console.log(JSON.stringify(think)) // {"eat":"🥩","sleep":"😴"}

console.log(JSON.stringify(think, null, 2))
/*
{
  "eat": "🥩",
  "sleep": "😴"
}
*/
```

可以看到，我们使用 `JSON.stringify()` 输出的内容更具可读性。

## 制表符间距

你也可以传入一个 `"\t"` 制表符间距，格式化输出的数据。

```js
const think = { eat: '🥩', sleep: '😴' }

console.log(JSON.stringify(think, null, '\t'))

/*
{
  "eat": "🥩",
  "sleep": "😴"
}
*/
```

## Space 参数

`JSON.stringify` 的第三个参数用于控制间距。正是它提供了漂亮的字符串输出。

它允许两种类型的参数：`Number` 和 `String`。

### Number

如果 `Space` 是一个 `Number` 类型，则表示 `JSON.stringify` 将在每个键之前放置的相应的空格数。可以使用 `0` 到 `10` 之间的任意数字作为缩进。

```js
const think = { eat: '🥩', sleep: '😴' }

console.log(JSON.stringify(think, null, 2))
/*
{
  "eat": "🥩",
  "sleep": "😴"
}
*/
```

### String

或者，可以使用字符串作为缩进。最多允许 10 个字符。如果您尝试传递超过 10 个字符，它将只使用前 10 个字符。

```js
const think = { eat: '🥩', sleep: '😴' }

console.log(JSON.stringify(think, null, '  I Love '))
/*
{
  I Love "eat": "🥩",
  I Love "sleep": "😴"
}
*/
```

## Express 和 Axios

对于不直接调用 `JSON.stringify()` 的框架，通常有一个设置 `spaces` 参数的选项。例如，Express 有一个全局 [`'json spaces'` 选项](https://expressjs.com/en/4x/api.html#app.settings.table)，允许您为所有 `res.json()` 调用设置 `spaces`。

```js
const express = require('express')
const app = express()

// 设置 spaces
app.set('json spaces', 2)
app.get('*', (req, res) => res.json({ name: 'O.O', age: 20 }))

const axios = require('axios')

const res = await axios.get('http://localhost:3000', {
  transformResponse: (body) => body // 禁用 JSON 解析，使 res.data 为字符串
})

console.log(res.data)
/*
{
  "name": "O.O",
  "age": 20
}
*/
```

Axios 没有设置 JSON 格式的[显式选项](https://axios-http.com/docs/req_config)，但您可以使用 [`transformRequest` 选项](https://stackoverflow.com/questions/48819885/axios-transformrequest-how-to-alter-json-payload)自行处理 JSON 序列化。关键语法如下：

```js
const axios = require('axios')

const obj = {
  name: 'O.O',
  age: 20,
  address: 'xxx'
}

const res = await axios.post('http://localhost:5000', obj, {
  transformRequest: (data) => JSON.stringify(data, null, 2), // 发送漂亮的 JSON 格式
  transformResponse: (body) => body
})

console.log(res.data)
/*
{
  "name": "O.O",
  "age": 20
}
*/
```

## Prettyjson

[Prettyjson](https://www.npmjs.com/package/prettyjson) 以 YAML 样式格式化 JSON 数据。Prettyjson 仅在 CLI 上工作，如果将 Prettyjson 输出作为 HTTP 响应发送，则无法获得颜色。

下面是使用 Prettyjson 从 Node.js 打印 JSON 的示例：

```js
const prettyjson = require('prettyjson')

console.log(
  prettyjson.render({
    name: 'IU',
    age: 18,
    love: 'me',
    hobby: 'sing'
  })
)
```

效果如下：

![prettyjson](https://upload-images.jianshu.io/upload_images/18281896-8c47f57359cad5ca.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

您应该使用以上这类方法来使你想要突出显示的数据以提高可读性。

## replacer 参数

这里我们额外在说一下`JSON.stringify` 的第二个参数 **replacer**，我们可以使用它来转换结果。

它允许两种类型的参数：`Array` 和 `Function`。

### Array

```js
const user = {
  name: 'IU',
  age: 18,
  love: 'me',
  hobby: 'sing'
}

console.log(JSON.stringify(user, ['name', 'love'], 2))
/*
{
  "name": "IU",
  "love": "me"
}
*/
```

### Function

我们为每一项调用一次 `function`，你也可以循环每一项，并在每次传递时使用函数中定义的逻辑进行操作。

下面是一个示例，我跳过了值不是字符串的属性。换句话说，我只想显示值为数字的项。

```js
const user = {
  name: 'IU',
  age: 18,
  love: 'me',
  hobby: 'sing'
}

const replacer = function (key, value) {
  if (typeof value !== 'string') return value
  return undefined
}

console.log(JSON.stringify(user, replacer, 2))
/*
{
  "age": 18
}
*/
```
