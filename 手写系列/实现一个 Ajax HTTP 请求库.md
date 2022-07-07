# 实现一个 AJAX HTTP 请求库

使用 [UMD](https://github.com/lio-zero/blog/blob/main/JavaScript/UMD.md) 作为通用的模块系统：

```js
;(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // 检查 AMD
    define([], function () {
      return factory(root)
    })
  } else if (typeof exports === 'object') {
    // 检查 CommonJS
    module.exports = factory(root)
  } else {
    window.atomic = factory(root)
  }
})(
  typeof global !== 'undefined'
    ? global
    : typeof window !== 'undefined'
    ? window
    : this,
  function (window) {
    'use strict'
  }
)
```

提供一个默认设置选项：

```js
let settings

const defaults = {
  method: 'GET',
  data: {},
  headers: {
    'Content-type': 'application/x-www-form-urlencoded'
  },
  responseType: 'text',
  timeout: null,
  withCredentials: false
}
```

检查当前环境是否支持所需方法和 API：

```js
const supports = function () {
  return 'XMLHttpRequest' in window && 'JSON' in window && 'Promise' in window
}
```

将两个或多个对象合并在一起：

```js
const extend = function () {
  const extended = {}

  // 将对象合并到扩展对象中
  const merge = function (obj) {
    for (const prop in obj) {
      if (obj.hasOwnProperty(prop)) {
        if (Object.prototype.toString.call(obj[prop]) === '[object Object]') {
          extended[prop] = extend(extended[prop], obj[prop])
        } else {
          extended[prop] = obj[prop]
        }
      }
    }
  }

  // 循环遍历每个对象并进行合并
  for (let i = 0; i < arguments.length; i++) {
    const obj = arguments[i]
    merge(obj)
  }

  return extended
}
```

将文本响应解析为 JSON：

```js
const parse = function (req) {
  let result
  if (settings.responseType !== 'text' && settings.responseType !== '') {
    return { data: req.response, xhr: req }
  }
  try {
    result = JSON.parse(req.responseText)
  } catch (e) {
    result = req.responseText
  }
  return { data: result, xhr: req }
}
```

将对象转换为查询字符串：

```js
const param = function (obj) {
  // 如果已经是字符串或 FormData 对象，则按原样返回
  if (
    typeof obj === 'string' ||
    Object.prototype.toString.call(obj) === '[object FormData]'
  )
    return obj

  // 如果 content-type 设置为 JSON，则字符串化 JSON 对象
  if (
    /application\/json/i.test(settings.headers['Content-type']) ||
    Object.prototype.toString.call(obj) === '[object Array]'
  )
    return JSON.stringify(obj)

  // 否则，将对象转换为序列化字符串
  const encoded = []
  for (let prop in obj) {
    if (obj.hasOwnProperty(prop)) {
      encoded.push(
        encodeURIComponent(prop) + '=' + encodeURIComponent(obj[prop])
      )
    }
  }
  return encoded.join('&')
}
```

发送 XHR 请求，作为 promise 返回：

```js
const makeRequest = function (url) {
  // 创建 XHR 请求
  const request = new XMLHttpRequest()

  // 设置 promise
  const xhrPromise = new Promise((resolve, reject) => {
    // 设置监听器以处理已完成的请求
    request.onreadystatechange = () => {
      // 仅在请求完成时运行
      if (request.readyState !== 4) return

      // 防止超时错误被处理
      if (!request.status) return

      // 处理响应
      if (request.status >= 200 && request.status < 300) {
        // 如果成功
        resolve(parse(request))
      } else {
        // 如果失败
        reject({
          status: request.status,
          statusText: request.statusText,
          responseText: request.responseText
        })
      }
    }

    // 设置我们的 HTTP 请求
    request.open(settings.method, url, true)
    request.responseType = settings.responseType

    // 添加 headers
    for (let header in settings.headers) {
      if (settings.headers.hasOwnProperty(header)) {
        request.setRequestHeader(header, settings.headers[header])
      }
    }

    // 设置超时
    if (settings.timeout) {
      request.timeout = settings.timeout
      request.ontimeout = (e) => {
        reject({
          status: 408,
          statusText: 'Request timeout'
        })
      }
    }

    // 添加凭证
    if (settings.withCredentials) {
      request.withCredentials = true
    }

    // 发送请求
    request.send(param(settings.data))
  })

  // 取消 XHR 请求
  xhrPromise.cancel = () => {
    request.abort()
  }

  // 将请求作为 Promise 返回
  return xhrPromise
}
```

最终，安装 Atomic 并返回：

```js
const Atomic = (url, options) => {
  // 检查浏览器支持
  if (!supports()) throw 'Atomic: 此浏览器不支持此插件中使用的方法。'

  // 将选项合并为默认值
  settings = extend(defaults, options || {})

  // 提出请求
  return makeRequest(url)
}

return Atomic
```

当然，你还可以添加一些 loading、文件上传类型判断，以及身份凭证等，来补充你所需要的功能。

## 最终代码

```js
;(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    define([], function () {
      return factory(root)
    })
  } else if (typeof exports === 'object') {
    module.exports = factory(root)
  } else {
    window.atomic = factory(root)
  }
})(
  typeof global !== 'undefined'
    ? global
    : typeof window !== 'undefined'
    ? window
    : this,
  function (window) {
    'use strict'
    let settings

    // 默认设置
    const defaults = {
      method: 'GET',
      data: {},
      headers: {
        'Content-type': 'application/x-www-form-urlencoded'
      },
      responseType: 'text',
      timeout: null,
      withCredentials: false
    }

    /**
     * 特性测试
     * @return {Boolean} 如果为 true，则支持所需的方法和 API
     */
    const supports = function () {
      return (
        'XMLHttpRequest' in window && 'JSON' in window && 'Promise' in window
      )
    }

    /**
     * 将两个或多个对象合并在一起。
     * @param   {Object}   objects  要合并在一起的对象
     * @returns {Object}            默认值和选项的合并值
     */
    const extend = function () {
      const extended = {}

      // 将对象合并到扩展对象中
      const merge = function (obj) {
        for (let prop in obj) {
          if (obj.hasOwnProperty(prop)) {
            if (
              Object.prototype.toString.call(obj[prop]) === '[object Object]'
            ) {
              extended[prop] = extend(extended[prop], obj[prop])
            } else {
              extended[prop] = obj[prop]
            }
          }
        }
      }

      // 循环遍历每个对象并进行合并
      for (let i = 0; i < arguments.length; i++) {
        const obj = arguments[i]
        merge(obj)
      }

      return extended
    }

    /**
     * 将文本响应解析为 JSON
     * @private
     * @param  {String} req 响应
     * @return {Array}      responseText 的 JSON 对象，加上原始响应
     */
    const parse = function (req) {
      let result
      if (settings.responseType !== 'text' && settings.responseType !== '') {
        return { data: req.response, xhr: req }
      }
      try {
        result = JSON.parse(req.responseText)
      } catch (e) {
        result = req.responseText
      }
      return { data: result, xhr: req }
    }

    /**
     * 将对象转换为查询字符串
     * @link   https://blog.garstasio.com/you-dont-need-jquery/ajax/
     * @param  {Object|Array|String} obj 对象
     * @return {String}                  查询字符串
     */
    const param = function (obj) {
      // 如果已经是字符串或 FormData 对象，则按原样返回
      if (
        typeof obj === 'string' ||
        Object.prototype.toString.call(obj) === '[object FormData]'
      )
        return obj

      // 如果 content-type 设置为 JSON，则字符串化 JSON 对象
      if (
        /application\/json/i.test(settings.headers['Content-type']) ||
        Object.prototype.toString.call(obj) === '[object Array]'
      )
        return JSON.stringify(obj)

      // 否则，将对象转换为序列化字符串
      const encoded = []
      for (let prop in obj) {
        if (obj.hasOwnProperty(prop)) {
          encoded.push(
            encodeURIComponent(prop) + '=' + encodeURIComponent(obj[prop])
          )
        }
      }
      return encoded.join('&')
    }

    /**
     * 发送 XHR 请求，作为 promise 返回
     * @param  {String} url 请求 URL
     * @return {Promise}    XHR 请求 promise
     */
    const makeRequest = function (url) {
      // 创建 XHR 请求
      const request = new XMLHttpRequest()

      // 设置 promise
      const xhrPromise = new Promise(function (resolve, reject) {
        // 设置监听器以处理已完成的请求
        request.onreadystatechange = function () {
          // 仅在请求完成时运行
          if (request.readyState !== 4) return

          // 防止超时错误被处理
          if (!request.status) return

          // 处理响应
          if (request.status >= 200 && request.status < 300) {
            // 如果成功
            resolve(parse(request))
          } else {
            // 如果失败
            reject({
              status: request.status,
              statusText: request.statusText,
              responseText: request.responseText
            })
          }
        }

        // 设置我们的 HTTP 请求
        request.open(settings.method, url, true)
        request.responseType = settings.responseType

        // 添加 headers
        for (let header in settings.headers) {
          if (settings.headers.hasOwnProperty(header)) {
            request.setRequestHeader(header, settings.headers[header])
          }
        }

        // 设置超时
        if (settings.timeout) {
          request.timeout = settings.timeout
          request.ontimeout = function (e) {
            reject({
              status: 408,
              statusText: 'Request timeout'
            })
          }
        }

        // 添加凭证
        if (settings.withCredentials) {
          request.withCredentials = true
        }

        // 发送请求
        request.send(param(settings.data))
      })

      // 取消 XHR 请求
      xhrPromise.cancel = function () {
        request.abort()
      }

      // 将请求作为 Promise 返回
      return xhrPromise
    }

    /**
     * 安装 Atomic
     * @param {String} url      请求 URL
     * @param {Object} options 请求的一组选项 [可选]
     */
    const Atomic = function (url, options) {
      // 检查浏览器支持
      if (!supports()) throw 'Atomic: 此浏览器不支持此插件中使用的方法。'

      // 将选项合并为默认值
      settings = extend(defaults, options || {})

      // 提出请求
      return makeRequest(url)
    }

    // 返回公共方法
    return Atomic
  }
)
```

示例：

```js
;(async () => {
  const { data } = await atomic('https://jsonplaceholder.typicode.com/todos/1')
  console.log(data)
})()
```
