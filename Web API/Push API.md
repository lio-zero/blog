# Push API

[Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API) 允许 Web 应用接收服务器推送的消息，即使 Web 应用当前未在浏览器中打开或未在设备上运行。

使用 Push API，你可以向用户发送消息，将消息从服务器推送到客户端，即使用户没有浏览网站。

这使你能够发送通知和内容更新，让你拥有更积极的受众。

这是很大的提升，因为与原生应用相比，移动网络缺少的支柱之一是接收通知的能力，以及离线支持。

以下是 Can I Use 提供的 [Push API 支持情況](https://caniuse.com/?search=Push%20API)：

![Push API 支持情況](https://upload-images.jianshu.io/upload_images/18281896-4e8d39bd8df62a37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 它如何工作？

当用户访问你的 Web 应用时，你可以触发一个面板，询问是否允许发送更新。安装了一个 Service Worker，并在后台运行，以监听推送事件。

> 推送和通知是一个独立的概念和 API，有时由于 iOS 中使用的推送通知术语而混为一谈。基本上，当使用 Push API 接收到推送事件时，就会调用 [Notifications API](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Notification%20API.md)。

你的服务器将通知发送到客户端，如果获得权限，Service Worker 将接收推送事件。Service Worker 通过触发通知对此事件做出响应。

## 获得用户的权限

使用 Push API 的第一步是获得用户从你那里接收数据的权限。

许多网站对这个面板的实现很糟糕，在第一页加载时就显示出来了。用户还不相信你的内容是好的，他们通常会拒绝权限。需要有一个好的做法。

有 6 个步骤：

- 检查是否支持 Service Worker
- 检查是否支持 Push API
- 注册一个 Service Worker
- 向用户请求权限
- 订阅用户并获取 PushSubscription 对象
- 将 PushSubscription 对象发送到服务器

### 检查是否支持 Service Worker

```js
const hasSupportServiceWorker = () => Boolean('serviceWorker' in navigator)

// 不支持 service Worker 直接返回
if (!hasSupportServiceWorker) return
```

### 检查是否支持 Push API

```js
const hasSupportPushManager = () => Boolean('PushManager' in navigator)

// 不支持 Push API 直接返回
if (!hasSupportPushManager) return
```

### 注册 Service Worker

此代码注册位于 `worker.js` 域根目录中的文件中的 Service Worker：

```js
window.addEventListener('load', () => {
  navigator.serviceWorker.register('/worker.js').then(
    (registration) => {
      console.log(`Service Worker 注册已完成: ${registration.scope}`)
    },
    (err) => {
      console.log('Service Worker 注册失败', err)
    }
  )
})
```

你可以查阅 [Service Worker](https://github.com/lio-zero/blog/blob/main/Web%20API/Service%20Worker.md) 了解它是如何工作的。

### 向用户请求权限

现在 Service Worker 已注册，你可以请求权限。

实现这一点的 API 随着时间的推移发生了变化，它从接受回调函数作为参数变为返回 Promise，打破了向后和向前的兼容性，我们需要做一下兼容处理，因为我们不知道用户的浏览器实现了哪种方法。

我们调用 `Notification.requestPermission()`，代码如下：

```js
const askPermission = () => {
  return new Promise((resolve, reject) => {
    const permissionResult = Notification.requestPermission((result) => {
      resolve(result)
    })

    if (permissionResult) {
      permissionResult.then(resolve, reject)
    }
  }).then((permissionResult) => {
    if (permissionResult !== 'granted') {
      throw new Error('没有权限')
    }
  })
}
```

`permissionResult` 值是一个字符串，其值可以为：

- `granted`
- `default`
- `denied`

此代码使浏览器显示权限对话框：

![浏览器权限对话框](https://upload-images.jianshu.io/upload_images/18281896-6553d8f09248dede.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果用户点击**禁止按钮**，你将无法再请求用户的许可，除非他们在浏览器的**高级设置**面板中取消阻止该网站（这种情况不太可能发生）。

### 订阅用户并获取 PushSubscription 对象

如果用户允许我们，我们可以订阅它并调用 `registration.pushManager.subscribe()`：

```js
const APP_SERVER_KEY = 'XXX'

window.addEventListener('load', () => {
  navigator.serviceWorker.register('/worker.js').then(
    (registration) => {
      askPermission()
        .then(() => {
          const options = {
            userVisibleOnly: true,
            applicationServerKey: urlBase64ToUint8Array(APP_SERVER_KEY)
          }
          return registration.pushManager.subscribe(options)
        })
        .then((pushSubscription) => {
          // 现在，我们有了 pushSubscription 对象
        })
    },
    (err) => {
      console.log('Service Worker 注册失败', err)
    }
  )
})
```

`APP_SERVER_KEY` 是一个字符串，称为 **Application SERVER KEY** 或 **VAPID KEY**，用于标识应用程序公钥，是公钥/私钥对的一部分。

出于安全原因，它将作为验证的一部分，以确保你（并且只有你，而不是其他人）可以将推送消息发送回用户。

### 将 PushSubscription 对象发送到服务器

在前面的代码段中，我们得到了 `pushSubscription` 对象，它包含了向用户发送推送消息所需的全部内容。我们需要将此信息发送到服务器，以便以后能够发送通知。

首先，我们创建对象的 JSON 表示：

```js
const subscription = JSON.stringify(pushSubscription)
```

我们可以使用 Fetch API 将其发布到我们的服务器：

```js
const sendToServer = (subscription) => {
  return fetch('/api/subscription', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(subscription)
  })
    .then((res) => {
      if (!res.ok) throw new Error('发生错误')
      return res.json()
    })
    .then(({ data }) => {
      if (!(data && data.success)) throw new Error('发生错误')
    })
}

sendToServer(subscription)
```

在服务器端，`/api/subscription` 接口接收 POST 请求，并可以将订阅信息存储到其存储器中。

## 服务器端的工作方式

到目前为止，我们只讨论了客户端部分：**获得用户的权限以便在将来收到通知**。

**服务器呢？它应该做什么，以及如何与客户端交互？**

这些服务器端示例，我们使用最熟悉的 Node.js Express.js 作为基本 HTTP 框架，但你可以使用任何语言或框架编写服务器端 Push API 处理程序。

### 注册新的客户端订阅

当客户端发送新订阅时，请记住我们使用了 `/api/subscription` HTTP POST 接口，在 `body` 中以 JSON 格式发送 `PushSubscription` 对象详细信息。

我们初始化 Express.js：

```js
const express = require('express')
const app = express()
```

以下函数确保请求有效，具有 body 和接口属性，否则它会向客户端返回错误：

```js
const isValidSaveRequest = (req, res) => {
  if (!req.body || !req.body.endpoint) {
    res.status(400)
    res.setHeader('Content-Type', 'application/json')
    res.send(
      JSON.stringify({
        error: {
          id: 'no-endpoint',
          message: '订阅必须有一个端点'
        }
      })
    )
    return false
  }
  return true
}
```

下一个函数将订阅保存到数据库中，返回在插入完成（或失败）时解析的 Promise。`insertToDatabase` 函数是一个占位符，我们这里不讨论这些细节：

```js
const saveSubscriptionToDatabase = (subscription) => {
  return new Promise((resolve, reject) => {
    insertToDatabase(subscription, (err, id) => {
      if (err) {
        reject(err)
        return
      }

      resolve(id)
    })
  })
}
```

我们在下面的 POST 请求处理程序中使用这些函数。我们检查请求是否有效，然后保存请求，然后将 `data.success: true` 响应返回给客户端，或返回错误：

```js
app.post('/api/subscription', (req, res) => {
  if (!isValidSaveRequest(req, res)) return

  saveSubscriptionToDatabase(req, res.body)
    .then((subscriptionId) => {
      res.setHeader('Content-Type', 'application/json')
      res.send(JSON.stringify({ data: { success: true } }))
    })
    .catch((err) => {
      res.status(500)
      res.setHeader('Content-Type', 'application/json')
      res.send(
        JSON.stringify({
          error: {
            id: 'unable-to-save-subscription',
            message: '已收到订阅，但未能保存它'
          }
        })
      )
    })
})

app.listen(3000, () => {
  console.log('Listening on port 3000')
})
```

### 发送推送消息

现在服务器已经在其列表中注册了客户端，我们可以向其发送推送消息。让我们通过创建一个示例代码段来了解其工作原理，该代码段获取所有订阅并同时向所有订阅发送推送消息。

此示例中，我们会使用 [web-push](https://github.com/web-push-libs/web-push) 库来处理发送推送消息。

> 我们之所以使用库，是因为 Web Push 协议很复杂，而库允许我们抽象出许多低级代码，以确保我们能够安全、正确地处理任何边缘情况。

首先，我们初始化生成一个私钥和公钥，并将它们设置为 VAPID 详细信息：

```js
const webpush = require('web-push')
const vapidKeys = webpush.generateVAPIDKeys()

const PUBLIC_KEY = 'XXX'
const PRIVATE_KEY = 'YYY'

const vapidKeys = {
  publicKey: PUBLIC_KEY,
  privateKey: PRIVATE_KEY
}

webpush.setVapidDetails(
  'mailto:my@email.com',
  vapidKeys.publicKey,
  vapidKeys.privateKey
)
```

然后，我们设置一个 `triggerPush()` 方法，负责将推送事件发送给客户端。它只是调用 `webpush.sendNotification()` 并捕获任何错误。如果返回错误 HTTP 状态代码是 410，这意味着用户离开了，我们从数据库中删除该订阅者。

```js
const triggerPush = (subscription, dataToSend) => {
  return webpush.sendNotification(subscription, dataToSend).catch((err) => {
    if (err.statusCode === 410) {
      return deleteSubscriptionFromDatabase(subscription.id)
    } else {
      console.log('订阅已不再有效: ', err)
    }
  })
}
```

我们没有实现从数据库中获取订阅，但我们将其保留：

```js
const getSubscriptionsFromDatabase = () => {
  // stub
}
```

代码的核心是将 POST 请求回调到 `/api/push` 接口：

```js
app.post('/api/push', (req, res) => {
  return getSubscriptionsFromDatabase()
    .then((subscriptions) => {
      let promiseChain = Promise.resolve()
      for (let i = 0; i < subscriptions.length; i++) {
        const subscription = subscriptions[i]
        promiseChain = promiseChain.then(() =>
          triggerPush(subscription, dataToSend)
        )
      }
      return promiseChain
    })
    .then(() => {
      res.setHeader('Content-Type', 'application/json')
      res.send(JSON.stringify({ data: { success: true } }))
    })
    .catch((err) => {
      res.status(500)
      res.setHeader('Content-Type', 'application/json')
      res.send(
        JSON.stringify({
          error: {
            id: 'unable-to-send-messages',
            message: `发送推送失败 ${err.message}`
          }
        })
      )
    })
})
```

上面的代码所做的工作是：从数据库中获取所有订阅，然后对它们进行迭代，并调用我们前面解释的 `triggerPush()` 函数。

订阅完成后，我们会返回一个成功的 JSON 响应，除非发生错误并返回 500 错误。

### 现实场景中

除非你有一个非常特殊的用例，或者你只是想学习技术或者你喜欢 DIY，否则你不太可能建立自己的推送服务器。相反，你通常希望使用 [OneSignal](https://onesignal.com) 之类的平台，它可以透明地免费处理所有平台的 Push 事件，包括 Safari 和 iOS。

## 接收推送事件

**当从服务器发送推送事件时，客户端如何获取它？**

客户端很简单，我们只需要在 Service Worker 内部运行，并监听 `push` 事件：

```js
self.addEventListener('push', (event) => {
  console.log('data: ', event.data)
})
```

`event.data` 包含 PushMessageData 对象，该对象以你想要的格式公开检索服务器发送的推送数据的方法：

- `arrayBuffer()` - 作为 ArrayBuffer 对象
- `blob()` - 作为 Blob 对象
- `json()` - 解析为 JSON
- `text()` - 纯文本

你通常会使用 `event.data.json()`。

## 显示通知

它与 Notifications API 有一些交集，因为 Push API 的主要用例之一是显示通知。

在 Service Worker 中的 `push` 事件监听器中，我们需要向用户显示通知，并告诉事件等待浏览器显示后，函数才能终止。我们延长事件生存周期，直到浏览器显示完通知（直到 Promise 得到解决），否则 Service Worker 可能会在处理过程中停止：

```js
self.addEventListener('push', (event) => {
  const promiseChain = self.registration.showNotification('Hello, world!')
  event.waitUntil(promiseChain)
})
```

## 更多资料

[Web Push Arrives in Firefox 44](https://hacks.mozilla.org/2016/01/web-push-arrives-in-firefox-44/)
