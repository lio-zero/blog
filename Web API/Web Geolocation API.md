# Web Geolocation API

[Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)（地理定位 API）允许用户在需要时向 Web 应用程序提供用户的地理位置。出于隐私原因，用户需要获得报告位置信息的权限。

它是基于权限的，要求用户批准在一个网站和一个请求的基础上共享该数据。它还需要 SSL 证书，尽管在本地运行时可以不使用 SSL 证书。

## 用途

实际上 JavaScript 可以捕获你的经度和纬度，并可以发送到后端 Web 服务器，做一些奇特的位置感知的事情，比如：

- 在地图上显示用户的位置、用户附近的兴趣点或标注一些其他信息
- `watchPosition` 方法可以用于路线导航（GPS）

如今，大多数浏览器和移动设备都支持地理定位 API。以下是 [Can I Use](https://www.caniuse.com/?search=geolocation) 给出的兼容情况：

![Geolocation API 兼容情况](https://upload-images.jianshu.io/upload_images/18281896-c816f3e05fdbb900.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **注意**：地理位置功能涉及用户隐私，出于安全考虑，当一个 Web 页尝试获取地理位置信息时，会请求用户批准地理位置访问权限。用户可能会拒绝，浏览器将不会共享你的定位数据。另外，该 API 只能在 HTTPS 环境使用。

浏览器通过 `navigator.geolocation` 属性提供该 API，该属性将返回一个 Geolocation 对象。该对象具有以下三个方法。

- `Geolocation.getCurrentPosition()` — 返回一个 Position 对象，表示用户的当前位置。
- `Geolocation.watchPosition()` — 指定一个监听函数，每当用户的位置发生变化，就执行该监听函数。
- `Geolocation.clearWatch()` — 取消 `watchPosition()` 方法指定的监听函数。

下面我们将来讲讲这三个方法。

## 获取用户的位置

我们可以使用 `navigator.geolocation.getCurrentPosition()` 方法来获取用户的位置：

```js
navigator.geolocation.getCurrentPosition(success, error, options)
```

该方法接受三个参数。

- `success` — 用户同意给出位置时的回调函数，它的参数是一个 Position 对象。
- `error` — 用户拒绝给出位置时的回调函数，它的参数是一个 PositionError 对象。该参数可选。
- `options` — 参数对象，该参数可选。

Position 对象有两个属性。

- `Position.coords` — 返回一个 Coordinates 对象，表示当前位置的坐标。
- `Position.timestamp` — 返回一个对象，代表当前时间戳。

PositionError 对象主要有两个属性。

- `PositionError.code` — 整数，表示发生错误的原因。`1`表示无权限，有可能是用户拒绝授权；`2`表示无法获得位置，可能设备有故障；`3`表示超时。
- `PositionError.message` — 字符串，表示错误的描述。

参数对象 `option` 可以指定三个属性。

- `enableHighAccuracy` — 是否返回高精度结果。如果设为 `true`，可能导致响应时间变慢或（移动设备的）功耗增加；反之，如果设为 `false`，设备可以更快速地响应。默认值为 `false`。
- `timeout` — 正整数，表示等待查询的最长时间，单位为毫秒。默认值为 `Infinity`。
- `maximumAge` — 正整数，表示可接受的缓存最长时间，单位为毫秒。如果设为 `0`，表示不返回缓存值，必须查询当前的实际位置；如果设为 `Infinity`，必须返回缓存值，不管缓存了多少时间。默认值为 `0`。

```js
const options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
}

const success = (pos) => {
  const crd = pos.coords
  console.log(`经度：${crd.latitude} 度`)
  console.log(`纬度：${crd.longitude} 度`)
  console.log(`海拔：${crd.altitude} 米`)
  console.log(`经度和纬度属性的精度：${crd.accuracy} 米`)
  console.log(`海拔的精确度：${crd.altitudeAccuracy} 米`)
  console.log(`设备的速度：${crd.speed} 米/秒`)
  console.log(`设备运行的方向：${crd.heading} 度`)
}

// 处理错误和拒绝
const error = (err) => {
  console.warn(`ERROR(${err.code}): ${err.message}`)
}

navigator.geolocation.getCurrentPosition(success, error, options)

// 经度: xxx 度
// 纬度: xxx 度
// 海拔：xxx 米
// 经度和纬度属性的精度：xxx 米
// 海拔的精确度：xxx 米
// 设备的速度：xxx 米/秒
// 设备运行的方向：xxx 度
```

询问允许获取位置：

![询问允许获取位置](https://upload-images.jianshu.io/upload_images/18281896-c22b4bd32f82a9dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用上面的代码，浏览器将会询问您是否透露自己的地理位置信息。

> **注意**：只能在 HTTPS 中获取到该值，你可以在有 HTTPS 的网站，打开控制台进行测试，测试完后马上关闭。

![HTTP 不可使用该 API](https://upload-images.jianshu.io/upload_images/18281896-d127d43866343e5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上警告是在不安全的 HTTP 下运行该语句的情况。

如果使用的是谷歌，可以在设置的**隐私设置**和安全性管理网站授予地理位置的情况

![管理](https://upload-images.jianshu.io/upload_images/18281896-81f5bce07bbacdf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Coordinates 对象

上述获取用户位置的例子中，我们正是通过 `Position.coords` 属性来使用 `Coordinates` 对象，该坐标接口用于表示设备在地球上的位置和海拔，以及计算这些属性的精确度。

以下属性都返回浮点数，全部为只读属性：

- `Coordinates.latitude` — 表示纬度。
- `Coordinates.longitude` — 表示经度。
- `Coordinates.altitude` — 表示相对于海平面的位置海拔（单位：米）。如果实现无法提供数据，则此值可以为 `null`。
- `Coordinates.accuracy` — 表示经度和纬度属性的精度（单位：米）。
- `Coordinates.altitudeAccuracy` —表示海拔的精度（单位：米）。此值可以为 `null`。
- `Coordinates.speed` — 表示设备的速度（单位：米/秒）。此值可以为 `null`。
- `Coordinates.heading` — 表示设备运行的方向（单位：度）。表示设备离正北方向有多远。0 度表示正北，方向是顺时针方向确定的（这意味着东是 90 度，西是 270 度）。如果 `Coordinates.speed` 为 0，`heading` 属性返回 `NaN`。如果设备无法提供标题信息，则此值为 `null`。

## Geolocation.watchPosition()

`Geolocation.watchPosition()` 对象指定一个监听函数，每当用户的位置发生变化，就是自动执行这个函数。

```js
navigator.geolocation.watchPosition(success[, error[, options]])
```

该方法接受三个参数。

- `success` — 表示监听成功的回调函数，该函数可以传入一个 Position 对象作为参数。
- `error` — 可选，表示监听失败的回调函数，该函数可以传入一个 PositionError 对象作为参数。
- `options` — 可选，表示监听的参数配置对象。

该方法返回一个整数值，表示监听函数的编号。该整数用来供`Geolocation.clearWatch()`方法取消监听。

```js
let target = {
  latitude: 0,
  longitude: 0
}

let options = {
  enableHighAccuracy: false,
  timeout: 5000,
  maximumAge: 0
}

const success = (pos) => {
  const crd = pos.coords

  if (target.latitude === crd.latitude && target.longitude === crd.longitude) {
    console.log('Congratulation, you reach the target')
    navigator.geolocation.clearWatch(id)
  }
}

const error = (err) => {
  console.warn('ERROR(' + err.code + '): ' + err.message)
}

const id = navigator.geolocation.watchPosition(success, error, options)
```

## Geolocation.clearWatch()

`Geolocation.clearWatch()` 方法用来取消 `Geolocation.watchPosition()` 方法指定的监听函数。

```js
navigator.geolocation.clearWatch(id)
```

该参数由 `Geolocation.watchPosition()` 返回的监听函数的编号获得。
