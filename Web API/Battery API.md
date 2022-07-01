# Battery API

[Battery API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API) 方法提供有关设备的电池状态的信息。例如：允许访问以查看设备电池的电池电量，是否正在充电等信息。

![Battery API 支持情況](https://upload-images.jianshu.io/upload_images/18281896-9ff9d30ef48cf8ff.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 示例

以下是一个 🔋 电池的充电状态的示例：

```js
const h5 = document.querySelector('h5')
const batteryIndicator = document.querySelector('.indicator')
const hasSupport = () => Boolean('getBattery' in navigator)
const setClass = (element, classToAdd) =>
  (element.className = 'indicator ' + classToAdd)

async function initBattery() {
  const battery = await navigator.getBattery()

  function handleBattery() {
    try {
      // 电池属性更新时触发
      battery.addEventListener('chargingchange', updateUI)
      battery.addEventListener('levelchange', updateUI)
      battery.addEventListener('chargingtimechange', updateUI)
      battery.addEventListener('dischargingtimechange', updateUI)

      updateUI()
    } catch {
      console.warn('您的浏览器不支持此功能')
    }
  }

  function updateUI() {
    // 电池电量
    batteryIndicator.style.height = battery.level * 100 + '%'

    if (battery.charging) {
      h5.classList.add('batteryMan')
    } else {
      h5.classList.remove('batteryMan')
    }

    // 检查电池是否正在充电
    battery.charging
      ? setClass(batteryIndicator, 'charging')
      : setClass(batteryIndicator, 'notCharging')
  }

  handleBattery()
}

initBattery()
```

[示例地址](https://codepen.io/lio-zero/pen/OJpxrpK)
