# Web Vibration API

> 大多数现代移动设备包括振动硬件，其允许软件代码通过使设备摇动来向用户提供物理反馈。[Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API) 为 Web 应用程序提供访问此硬件（如果存在）的功能，如果设备不支持此功能，则不会执行任何操作。

如上所述，您可以使用 Vibration API 控制设备的振动能力。

根据 [Can I Use](https://caniuse.com/?search=Vibration%20API%20)，绝大部分主流浏览器都支持 Vibration API：

![image.png](https://upload-images.jianshu.io/upload_images/18281896-3fc4455223fd6263.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于以下的例子，你可以用手机查看 👉 [Vibration API 示例](https://codepen.io/lio-zero/pen/YzZxRWY)，测试其效果。**注意**：苹果自带的 Safari 不支持该功能，在测试时，**注意把手机的振动打开**。

## 一次振动

你可以通过指定单个值或仅由一个值组成的数组来一次振荡振动硬件：

```js
const oneVibrate = () => navigator.vibrate(200)
// or
const oneVibrate = () => navigator.vibrate([200])
```

以上两个例子都可以使设备振动 200 ms。

## 振动模式

你可以给定一组数组来描述设备振动和不振动的交替时间段。例如：

```js
const vibratePart = () => navigator.vibrate([500, 250, 500, 250, 500])
```

这会使设备振动 500 ms，然后暂停 250 ms，然后再次振动设备 500 ms。

**注意**：由于振动在每个振动周期结束时自动停止，因此您不必提供最后一个值去暂停，换句话说，数组长度只需要设置奇数个。

## 停止振动

当值为 0、空数组或数组元素全为 0 时，调用 [`Navigator.vibrate()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/vibrate) 将取消当前正在进行的任何振动模式。

## 持续振动

一些基于 `setInterval` 和 `clearInterval` 操作将允许您创建持续的振动：

```js
var vibrateInterval

// 在传递水平开始振动
const startVibrate = (duration) => navigator.vibrate(duration)

// 停止振动
const stopVibrate = () => {
  // 清除间隔并停止持续振动
  if (vibrateInterval) clearInterval(vibrateInterval)
  navigator.vibrate(0)
}

// 在给定的持续时间和间隔内开始持续振动
// 假设给定了一个数值
const startPeristentVibrate = (duration, interval) => {
  vibrateInterval = setInterval(function () {
    startVibrate(duration)
  }, interval)
}
```

当然上面的代码片段没有考虑到振动参数为数组情况；基于数组的持久性振动将需要计算数组项的和，并基于该数量创建周期（可能具有额外的延迟）。
