# Cache API

[Cache API](https://developer.mozilla.org/zh-CN/docs/Web/API/Cache) 是 Service Worker 规范的一部分，是一种增强资源缓存能力的好方法。

它允许您缓存 URL 可寻址资源，这意味着资源、网页、HTTP API 响应。

它并不意味着缓存单个数据块，这是 [IndexedDB API](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API) 的任务。

[Cache API 支持情况](https://caniuse.com/?search=Cache%20API)：

![Cache API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-fa4b948b048befbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 检测 Cache API 是否可用

Cache API 通过 `caches` 对象公开。要检测 API 是否在浏览器中实现，只需使用以下命令检查其是否存在：

```js
if ('caches' in window) {
  console.log('支持 Cache API')
}
```

## 初始化缓存

使用 `caches.open` API，它返回一个带有缓存对象的 Promise 以供使用：

```js
caches.open('my-cache').then((cache) => {
  // 您可以开始使用缓存
})
```

`my-cache` 是我用来标识要初始化的缓存的名称。它就像一个变量名，你可以使用任何你想要的名字。

如果缓存尚不存在，`caches.open` 则创建它。

## 将项目添加到缓存

`cache` 对象公开了两种将项目添加到缓存的方法：`add` 和 `addAll`。

### cache.add()

`add` 接受单个 URL，并在调用时获取资源并缓存它。

```js
caches.open('my-cache').then((cache) => {
  cache.add('/api/todos')
})
```

为了允许对获取进行更多控制，您可以传递一个请求对象，而不是字符串，它是 [Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch) 规范的一部分：

```js
caches.open('my-cache').then((cache) => {
  const options = {
    // the options
  }
  cache.add(new Request('/api/todos', options))
})
```

### cache.addAll()

`addAll` 接受一个数组，并在所有资源都被缓存后返回一个 Promise。

```js
caches.open('my-cache').then((cache) => {
  cache.addAll(['/api/todos', '/api/todos/today']).then(() => {
    // 所有请求都已缓存
  })
})
```

### 手动获取和添加

`cache.add()` 自动获取资源并将其缓存。

Cache API 通过 `cache.put()` 对此提供了更细粒度的控制。你负责获取资源，然后告诉 Cache API 存储一个响应：

```js
const url = '/api/todos'

fetch(url).then((res) => {
  return caches.open('my-cache').then((cache) => {
    return cache.put(url, res)
  })
})
```

## 从缓存中检索项目

`cache.match()` 返回一个 Response 对象，其中包含有关请求的所有信息和网络请求的响应

```js
caches.open('my-cache').then((cache) => {
  cache.match('/api/todos').then((res) => {
    // res 是响应对象
  })
})
```

## 获取缓存中的所有项目

```js
caches.open('my-cache').then((cache) => {
  cache.keys().then((cachedItems) => {
    console.log(cachedItems)
  })
})
```

`cachedItems` 是一个 Request 对象数组，其中包含 `url` 属性中资源的 URL。

## 获取所有可用的缓存

`caches.keys()` 方法列出了每个可用缓存的键。

```js
caches.keys().then((keys) => {
  // keys 是一个包含键列表的数组
  console.log(keys)
})
```

## 从缓存中删除项目

给定一个 `cache` 对象，其 `delete()` 方法将从中删除缓存的资源。

```js
caches.open('my-cache').then((cache) => {
  cache.delete('/api/todos')
})
```

## 删除缓存

`caches.delete()` 方法接受一个缓存标识符，并在执行时从系统中清除缓存及其缓存项。

```js
caches.delete('my-cache').then(() => {
  console.log('已成功删除')
})
```
