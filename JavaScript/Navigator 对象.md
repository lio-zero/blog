# Navigator 对象

浏览器公开的 `window.navigator` 属性指向一个 navigator 对象，它是一个容器对象，包含有关访问者浏览器的信息。

标准属性包括：

- `cookieEnabled` — 返回一个布尔值，来表示当前页面是否启用了 cookie。
- `geolocation` — 指向 Geolocation API 使用的 Geolocation。
- `language` — 返回表示浏览器中当前用户活动偏好语言的字符串。
- `onLine` — 返回浏览器的联网状态。正常联网（在线）返回 `true`，不正常联网（离线）返回 `false`。（浏览器以不同的方式解释，请注意）
- `serviceWorker` — 分配给文档的 [ServiceWorkerContainer](https://developer.mozilla.org/zh-CN/docs/Web/API/ServiceWorkerContainer) 对象。
- `userAgent` — 返回当前浏览器的用户代理（user agent）标识符的名称。
- `languages` — 返回一个数组，数组内容表示网站访客所使用的语言。使用 BCP 47 语言标签来描述不同的语言。在返回的数组中，最适合当前用户的语言将会被排到数组的首位。它占时只在一些浏览器内被使用。
- etc

> 以上列出的标准属性都是只读的。

标准方法包括：

- `registerProtocolHandler()` — 一种让网站注册为协议处理程序的方法。

还有更多的方法和属性，它们要么已被弃用（一些浏览器还支持它们），要么是实验性的，要么是作为草案实现的，还没有最终完成，要么只是在一小部分浏览器上可用，所以我在这里没有包括它们，感兴趣的请查阅 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator) 了解它们。

## `navigator` 对象具有误导性

来自 `navigator` 对象的信息是具有误导性的，不应该被用于检测浏览器版本，这是因为：

- `navigator` 数据可被浏览器使用者更改
- 一些浏览器对测试站点会识别错误
- 浏览器无法报告晚于浏览器发布的新操作系统

## 浏览器检测

由于 `navigator` 可误导浏览器检测，使用对象检测可用来嗅探不同的浏览器。

由于不同的浏览器支持不同的对象，您可以使用对象来检测浏览器。例如，由于只有 Opera 支持属性 `window.opera`，您可以据此识别出 Opera 浏览器。

```js
if (window.opera) {
  // do something
}
```

## 如何检测浏览器版本（UA 检测）？

以下是检测是否为 Chrome 浏览器：

```js
const isChrome = navigator.userAgent.includes('Chrome')
```

## 检测页面是在移动/PC 设备上查看

使用正则表达式测试 `navigator.userAgent` 属性，以确定设备是移动设备还是 PC 设备。

```js
const detectDeviceType = () =>
  /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
    navigator.userAgent
  )
    ? 'Mobile'
    : 'Desktop'

detectDeviceType() // 'Mobile' or 'Desktop'
```

你可以打开控制台，切换为移动设备进行模拟测试。

另外，推荐阅读[检测是否为移动浏览器](https://github.com/lio-zero/blog/blob/main/JavaScript/%E6%A3%80%E6%B5%8B%E6%98%AF%E5%90%A6%E4%B8%BA%E7%A7%BB%E5%8A%A8%E6%B5%8F%E8%A7%88%E5%99%A8.md)。

## 检查当前用户是否为苹果设备

方法与上一节类似，只不过换成了 `platform` 属性。

```js
const isAppleDevice = /Mac|iPod|iPhone|iPad/.test(navigator.platform)

console.log(isAppleDevice)
```

**注意**，该属性已废弃，但一些浏览器还支持它。

## 检测当前用户的首选语言

- 使用 `navigation.language` 或 `navigation.languages` 的第一个，否则返回 `defaultLang`。
- 省略参数 `defaultLang`，将以 `'zh-CN'` 用作默认语言。

```js
const detectLanguage = (defaultLang = 'zh-CN') =>
  navigator.language ||
  (Array.isArray(navigator.languages) && navigator.languages[0]) ||
  defaultLang

detectLanguage() // 'zh-CN'
```
