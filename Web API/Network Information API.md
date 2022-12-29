# Network Information API

[Network Information API](https://developer.mozilla.org/en-US/docs/Web/API/Network_Information_API) 允许网页访问有关系统设备的网络连接信息，如连接类型（例如，wifi、cellular 等）、信号强度和数据传输速率。

使用 Network Information API 可以在网页上显示有关网络连接信息的内容，也可以根据网络状态来调整网页的行为。例如，可以在网络连接速度较慢时，选择加载更小的图像或不加载某些内容。

要使用 Network Information API，需要在网页中引入 `navigator.connection` 对象。这个对象的 `effectiveType` 属性可以返回当前网络的类型，可能的值包括 "slow-2g"、"2g"、"3g" 和 "4g"。它还有其他一些属性，详细内容请查阅 MDN。

以下示例展示了如何使用 Network Information API 显示当前网络连接的类型：

```js
if (navigator.connection) {
  const connection = navigator.connection
  console.log(connection.effectiveType)
}
```

Network Information API 还可以通过监听 `connection` 对象的 `change` 事件来跟踪网络状态的变化。这个事件会在网络连接类型或速度发生变化时触发。

```js
if (navigator.connection) {
  navigator.connection.addEventListener('change', () => {
    console.log(navigator.connection.effectiveType)
  })
}
```

需要注意的是，Network Information API 只能访问网络连接的一些信息，而无法访问具体的网络地址或个人信息。

## 浏览器的支持情况

[Network Information API 支持情况](https://caniuse.com/netinfo)：

![Network Information API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-cdfe20a877ead084.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Network Information API 还处于实验阶段，不是所有浏览器都支持。在使用之前，应该先检查浏览器是否支持这个 API，以避免在不支持的浏览器中出现错误。

## 最后

使用 Network Information API 可以在网页中创建更加高效和适应性强的应用，但要谨慎使用，以免影响用户体验。

## 更多资料

[How to detect internet speed in JavaScript](https://hackthestuff.com/article/how-to-detect-internet-speed-in-javascript) — 另一种检测网络速度的方法
