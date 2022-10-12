# 实现 JSONP

现在，我们基本不会使用 JSONP 进行跨域了，因为有更好解决跨域的办法（如 CORS），但我们还是有必要了解 JSONP 的原理，以及它是如何实现的（毕竟面试很可能会问到）。

JSONP 的原理是利用 `script` 标签不受同源策略约束，可以用来进行跨域请求。

优点也显而易见，`script` 标签各个浏览器都兼容，但也有局限，只能用于 GET 请求。

## 实现

```js
const jsonp = ({ url, params, callbackName }) => {
  const generateUrl = () => {
    let dataSrc = ''
    for (let key in params) {
      dataSrc += `${key}=${params[key]}&`
    }
    dataSrc += `callback=${callbackName}`
    return `${url}?${dataSrc}`
  }

  const removeElement = (el) => el.parentNode.removeChild(el)

  return new Promise((resolve, reject) => {
    const scriptEle = document.createElement('script')
    scriptEle.src = generateUrl()
    document.body.appendChild(scriptEle)
    window[callbackName] = (data) => {
      resolve(data)
      removeElement(scriptEle)
    }
  })
}
```

JSONP 需要与后端配合，这里我们使用 Node 的 Expess 框架起一个简单的服务器：

```js
const app = require('express')()

app.get('/', (req, res) => {
  const { callback } = req.query
  const data = { name: 'lio' }
  res.send(`${callback}(${JSON.stringify(data)})`)
})

app.listen(3309)
```

示例：

```js
const result = jsonp({
  url: 'http://localhost:3309',
  params: {
    name: 'O.O',
    age: 18
  },
  callbackName: '_JSONP_CB_'
})

result.then(console.log) // {name: 'lio'}
```
