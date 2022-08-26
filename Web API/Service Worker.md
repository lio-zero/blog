# Service Worker

[Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) 是渐进式 Web 应用的核心，因为它允许缓存资源和推送通知，这是迄今为止将原生应用的两个主要区别功能。

Service Worker 是您的网页和网络之间的可编程代理，提供拦截和缓存网络请求的能力，有效地使您能够为应用创建离线优先体验。

它是一种特殊的 [web worker](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Worker.md)，是一种与网页相关联的 JavaScript 文件，在 worker 上下文上运行，与主线程分离，具有非阻塞的优点，因此可以在不牺牲 UI 响应性的情况下进行计算。

它位于一个单独的线程上，没有 DOM 访问权限，也无法访问 [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) 和 [XHR API](https://github.com/lio-zero/blog/blob/main/Web%20API/XMLHttpRequest.md)，并且只能使用 [Channel Messaging API](https://github.com/lio-zero/blog/blob/main/Web%20API/Channel%20Messaging%20API.md) 与主线程通信。

Service Worker 可以与其他 Web API 一起使用的有：

- [Fetch API](https://github.com/lio-zero/blog/blob/main/Web%20API/Fetch%20API.md)
- [Cache API](https://github.com/lio-zero/blog/blob/main/Web%20API/Cache%20API.md)

它们仅在 HTTPS 协议页面上可用，但本地请求除外，本地请求不需要安全连接以便于测试。

## 后台处理

Service Worker 独立于与其关联的应用运行，并且可以在不活动时接收消息。

例如，它们可以在以下情况正常工作：

- 当您的移动应用处于后台时，未处于活动状态
- 当您的移动应用关闭时，即使不在后台运行
- 当浏览器关闭时，如果应用正在浏览器中运行

Service Worker 主要场景是：

- 它们可以用作缓存层来处理网络请求，并缓存脱机时使用的内容
- 允许推送通知
- Service Worker 仅在需要时运行，在不使用时停止

## 离线支持

传统上，Web 应用的离线体验一直很差。如果没有网络，网络移动应用通常无法运行，而本地移动应用能够提供工作版本或某种好消息。

这不是一条好消息，但这是没有网络连接的 Chrome 中网页的样子：

![image.png](https://upload-images.jianshu.io/upload_images/18281896-518cf264a81ae23a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也许这唯一的好处是你可以通过点击恐龙来玩一个免费的游戏，但它很快就会变得无聊。

> 您可以通过在浏览器地址栏输入：chrome://dino/ 快速开始游戏。您可以在[有趣的 Chrome 彩蛋：谷歌小恐龙游戏](https://chinese.freecodecamp.org/news/do-you-know-the-chrome-dino-game-millions-of-people-are-playing/)详细了解它。

在过去，HTML5 AppCache 已经承诺允许 Web 应用缓存资源并离线工作，但其缺乏灵活性和令人困惑的行为清楚地表明它不足以胜任这项工作，未能兑现其承诺（并且已经停止使用）。

Service Worker 是离线缓存的新标准。

### 在安装期间预缓存资源

在整个应用中重复使用的资源，如图像、CSS、JavaScript 文件，可以在第一次打开应用时安装。

这为所谓的 [App Shell](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure#app_shell) 架构奠定了基础。

### 缓存网络请求

使用 Fetch API，我们可以编辑来自服务器的响应，确定服务器是否不可访问，并提供来自缓存的响应。

## Service Worker 生命周期

Service Worker 需要经过 3 个步骤才能完全正常工作：

- 注册
- 安装
- 激活

### 注册

注册告诉浏览器 Service Worker 在哪里，并在后台开始安装。

注册 Service Worker 的示例代码位于 `worker.js`：

```js
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/worker.js').then(
      (registration) => {
        console.log('Service Worker注册完成范围: ', registration.scope)
      },
      (err) => {
        console.log('Service Worker 注册失败', err)
      }
    )
  })
} else {
  console.log('不支持 Service worker')
}
```

即使多次调用此代码，浏览器也只会在 Service Worker 是新的、之前未注册或已更新的情况下执行注册。

#### 作用域

`register()` 方法还接受一个作用域参数，该参数是一个路径，用于确定 Service Worker 可以控制应用的哪个部分。

它默认为包含 Service Worker 文件的文件夹中包含的所有文件和子文件夹，因此如果您将其放在根文件夹中，它将控制整个应用。在子文件夹中，它将仅控制在该路由下可访问的页面。

下面的示例通过指定 `/notifications/` 文件夹作用域来注册 worker。

```js
navigator.serviceWorker.register('/worker.js', {
  scope: '/notifications/'
})
```

`/` 很重要：在这种情况下，页面 `/notifications` 不会触发 Service Worker，而作用域如果是：

```js
{
  scope: '/notifications'
}
```

它会奏效的。

> **注意**：Service Worker 无法从文件夹中**启动**自己，如果其文件放在 `/notifications` 下，则无法控制 `/` 路径或不在 `/notifications` 下的任何其他路径。

### 安装

如果浏览器确定某个 Service Worker 已过时或以前从未注册过，它将继续安装它。

```js
self.addEventListener('install', (event) => {
  // ...
})
```

这是事件可以通过初始化缓存来准备要使用的 Service Worker，并使用 Cache API 缓存应用 App Shell 和静态资源。

### 激活

激活阶段是 Service Worker 成功注册和安装后的第三步。

此时，Service Worker 将能够处理新的页面加载。

它无法与已加载的页面交互，这意味着 Service Worker 仅在用户第二次与应用交互或重新加载已打开的页面时才有用。

```js
self.addEventListener('activate', (event) => {
  // ...
})
```

此事件的一个良好用例是清理旧缓存和与旧版本相关但在新版本的 Service Worker 中未使用的内容。

## 更新 Service Worker

要更新 Service Worker，只需要改变一个字节，当运行注册代码时，它将被更新。

一旦 Service Worker 被更新，它将不可用，直到所有加载了旧 Service Worker 的页面都被关闭。

这确保了在已经工作的应用/页面上不会出现任何中断。

刷新页面是不够的，因为旧的 worker 仍在运行，并且没有被删除。

## 获取事件

当在网络上请求资源时，将触发 `fetch` 事件。

这使我们能够在发出网络请求之前查看缓存。

例如，下面的代码段使用 Cache API 检查请求 URL 是否已存储在缓存的响应中，如果是这种情况，则返回缓存的响应。否则，它执行 `fetch` 请求并返回它。

```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      if (response) {
        // 在缓存中找到条目
        return response
      }
      return fetch(event.request)
    })
  )
})
```

## 后台同步

后台同步允许延迟传出连接，直到用户具有一个工作网络连接。

这是确保用户可以脱机使用应用并对其采取操作的关键，以及在连接打开时排队等待服务器端更新，而不是显示出试图获取信号的无休止的轮询。

```js
navigator.serviceWorker.ready.then((swRegistration) => {
  return swRegistration.sync.register('event1')
})
```

此代码在 Service Worker 中监听事件：

```js
self.addEventListener('sync', (event) => {
  if (event.tag == 'event1') {
    event.waitUntil(doSomething())
  }
})
```

`doSomething()` 返回一个 promise。如果失败，将安排另一个同步事件自动重试，直到成功。

这还允许应用在有可用的工作连接时立即从服务器更新数据。

## 推送事件

Service Worker 通过使用以下工具，使 Web 应用能够向用户提供本机推送通知：

- [Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [Notification API](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Notification%20API.md)

Push 和 Notification 实际上是两种不同的概念和技术，但它们结合起来提供了我们所知的推送通知。Push 提供了一种机制，允许服务器向 Service Worker 发送信息，Notification 是 Service Worker 向用户显示信息的方式。

由于 Service Worker 即使在应用未运行时也会运行，因此他们可以监听即将到来的推送事件，并提供用户通知，或更新应用的状态。

推送事件由后端通过浏览器推送服务（如 [Firebase](http://firebase.google.com) 提供的推送服务）启动。

以下是 Service Worker 如何监听传入推送事件的示例：

```js
self.addEventListener('push', (event) => {
  console.log('Received a push event', event)

  const options = {
    title: '我有个消息给你!',
    body: '这是消息的正文',
    icon: '/img/icon-192x192.png',
    tag: 'tag-for-this-notification'
  }

  event.waitUntil(self.registration.showNotification(title, options))
})
```

## 关于控制台日志的说明

如果您在 Service Worker 中有任何控制台日志语句（`console.log` 等），请确保启用 Chrome DevTools 或等效工具提供的保留日志功能。

否则，由于 Service Worker 在加载页面之前进行操作，并且在加载页面前清除了控制台，因此您将不会在控制台中看到任何日志。

## 最后

我最近在学习 Vite，并试图了解其源码。

我发现了一个很棒的示例 [browser-vite](https://github.com/divriots/browser-vite)，作者将 Vite 成功的运行到浏览器上，而我们知道 Vite 是依赖 node 才能运行的。

**它是如何实现的？**

如作者所描述的，它由 Service Worker 提供服务。当然，不止这一点，您可以在提供的博文 [Vite in the browser](https://divriots.com/blog/vite-in-the-browser) 找到它所有的一切。

这是一个不错的示例，将它作为学完基本 Service Worker 后的第一个用例把！

## 更多资料

- [Add a Service Worker to Your Site](https://css-tricks.com/add-a-service-worker-to-your-site/)
- [Service Worker](https://frontendian.co/service-workers)
- [Making A Service Worker: A Case Study](https://www.smashingmagazine.com/2016/02/making-a-service-worker/)
- [Service workers explained](https://github.com/w3c/ServiceWorker/blob/main/explainer.md)
- [借助 Service Worker 和 cacheStorage 缓存及离线开发](https://www.zhangxinxu.com/wordpress/2017/07/service-worker-cachestorage-offline-develop/)
- [Service Worker 模拟单页应用 SPA](https://itnext.io/your-single-page-app-is-now-a-polyfill-7881fb01694e)
- [Progressive web app structure](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)
