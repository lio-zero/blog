# open 模块

启动 express 服务时，我们使用 [`open`](https://www.npmjs.com/package/open) 包将在默认浏览器打开到 `localhost`：

```js
const app = require('express')()
const open = require('open')

app.use('*', (req, res) => {
  res.send('Hello World!')
})

const port = 3000
const host = 'localhost'

app.listen(port, host, (err) => {
  if (err) return
  open(`http://${host}:${port}`)
  console.log(`Listening at http://${host}:${port}`)
})
```
