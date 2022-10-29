# Broadcast Channel API

[Broadcast Channel API](https://developer.mozilla.org/en-US/docs/Web/API/Broadcast_Channel_API) 可以在同源情况下，实现 1 对 1 消息在不同窗口、Tab 页面、`iframe` 或 Web Worker 之间发送。

Broadcast Channel API 也可用于发送 1 对多消息，同时与多个实体通信。

以下 MDN 提供的 Broadcast Channel API 的原理图：

![Broadcast Channel API 的原理](https://developer.mozilla.org/en-US/docs/Web/API/Broadcast_Channel_API/broadcastchannel.png)

[Broadcast 支持情况](https://caniuse.com/?search=Broadcast)：

![Broadcast 支持情况](https://upload-images.jianshu.io/upload_images/18281896-17a6b106cd217502.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 基本用法

首先，初始化 `BroadcastChannel` 对象：

```js
const channel = new BroadcastChannel('test_channel')
```

要在频道（本例 `test_channel`）上发送消息，请使用 `postMessage()` 方法：

```js
channel.postMessage('Hey!')
```

消息可以是任何受支持的值：

- 所有基本类型，不包括 `Symbol`
- 数组
- 对象字面量
- String、Date、RegExp 对象
- Blob、File、FileList 对象
- ArrayBuffer、ArrayBufferView 对象
- FormData 对象
- ImageData 对象
- map 和 set 对象

要从频道接收消息，需要监听 `message` 事件：

```js
channel.onmessage = (event) => {
  console.log('Received', event.data)
}
```

此事件会为所有监听器触发，发送消息的监听器除外。

您可以使用以下方法关闭频道：

```js
channel.close()
```

以下是一个简单的示例：

```js
const hasSupport = () => Boolean('BroadcastChannel' in window)
const CHANNEL_NAME = 'web_api_channel'

function run() {
  const receiver = document.getElementById('test_channel')

  const bc = new BroadcastChannel(CHANNEL_NAME)
  bc.postMessage('I am superman!')
  bc.onmessage = (event) => {
    console.log(`在频道 "${event.data}" 上收到消息 "${CHANNEL_NAME}"`)

    receiver.innerText = event.data
  }
}
```

## 更多资料

[broadcast-channel](https://github.com/pubkey/broadcast-channel) 用于不同的浏览器选项卡或 nodejs-processes + LeaderElection 通道之间发送数据。
