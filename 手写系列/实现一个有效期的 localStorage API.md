# 实现一个有效期的 localStorage API

其实就是对原生 API 进行一层封装。

大概思路：使用 `setItem` 时，存储用户设置的过期时间，当使用 `getItem` 时，将存储的过期时间与当前时间进行对比。如果大于当前时间，返回对应的值，否则使用 `removeItem` 将值移除，并返回 `null`

```js
function initLocalStorage() {
  // 默认过期时间为 1h
  localStorage.setItem = (key, value, time = 3600) => {
    const expiresTime = Date.now() + time * 1000
    const payload = {
      __data: value,
      __expiresTime: expiresTime
    }
    Storage.prototype.setItem.call(localStorage, key, JSON.stringify(payload))
  }

  localStorage.getItem = (key) => {
    const value = Storage.prototype.getItem.call(localStorage, key)
    if (typeof value === 'string') {
      const jsonVal = JSON.parse(value)
      if (jsonVal.__expiresTime) {
        if (jsonVal.__expiresTime > Date.now()) {
          return JSON.stringify(jsonVal.__data)
        } else {
          localStorage.removeItem(key)
          return null
        }
      }
    }
    return value
  }
}

initLocalStorage()
```

以上是一个大概的实现，您还可以对其进行进一步处理，例如：对传入的键值对进行加密处理等。

如果您还想深入了解，可以阅读：[你还在直接用 localStorage 么？该提升下逼格了](https://juejin.cn/post/7104301566857445412)。
