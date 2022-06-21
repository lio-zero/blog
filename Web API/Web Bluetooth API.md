# Web Bluetooth API

[Web Bluetooth API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Bluetooth_API) 允许网站通过 GATT（**Generic Attribute Profile**） 客户端与附近用户选择的低功耗蓝牙（[BLE](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Bluetooth_low_energy)）设备进行安全和隐私保护的通信 API。基于 Promise 规范。

> 维基百科：
>
> - [BLE](https://zh.wikipedia.org/wiki/%E8%93%9D%E7%89%99%E4%BD%8E%E5%8A%9F%E8%80%97)：**低功耗蓝牙**（**Bluetooth Low Energy**，或称**Bluetooth LE**、**BLE**，旧商标**Bluetooth Smart**）也称**蓝牙低能耗**、**低功耗蓝牙**，是[蓝牙技术联盟](https://zh.wikipedia.org/wiki/%E8%97%8D%E7%89%99%E6%8A%80%E8%A1%93%E8%81%AF%E7%9B%9F)设计和销售的一种[个人局域网](https://zh.wikipedia.org/wiki/%E5%80%8B%E4%BA%BA%E5%8D%80%E5%9F%9F%E7%B6%B2%E7%B5%A1)技术，旨在用于医疗保健、[运动健身](https://zh.wikipedia.org/wiki/%E9%AB%94%E9%81%A9%E8%83%BD)、信标、安防、家庭娱乐等领域的新兴应用。相较[经典蓝牙](https://zh.wikipedia.org/wiki/%E8%97%8D%E7%89%99)，低功耗蓝牙旨在保持同等通信范围的同时显著降低功耗和成本。
>
> - GATT 是一个在蓝牙连接之上的发送和接收很短的数据段的通用规范。

其还在实验阶段，在生产中使用之前，请仔细检查浏览器兼容性表。以下是 [Can I Use](https://caniuse.com/?search=Web%20Bluetooth) 给出的兼容情况：

![Web Bluetooth API 兼容情况](https://upload-images.jianshu.io/upload_images/18281896-0e5ad83219d0a864.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 注意：此 API 在 Web Worker 中不可用。

## 基础用法

下面，我们将使用一个简单的示例，来看看如何获取一台 BLE 设备的基本信息。

**注意**：为了方便实现效果，我们自己在本地手写一个配置选项。

我们使用 `navigator.bluetooth.requestDevice()` 方法并为函数提供一个配置对象，该对象含有关我们要使用哪个设备，以及都有哪些服务可用的信息。它将返回一个 `Promise` 对象。如果没有蓝牙设备选择界面，则此方法返回与条件匹配的第一个设备。

```html
<button class="btn">点我！</button>
```

```js
let options = {
  filters: [
    { services: ['heart_rate'] },
    { services: [0x1802, 0x1803] },
    { services: ['c48e6067-5295-48d3-8d5c-0395f61792b1'] },
    { name: '设备名' },
    { namePrefix: '前缀' }
  ],
  optionalServices: ['battery_service']
}

const button = document.querySelector('.btn')
button.addEventListener('click', function () {
  navigator.bluetooth
    .requestDevice(options)
    .then((device) => {
      console.log('Got device:', device.name)
      // 在此处实现设备调用
    })
    .catch((err) => {
      console.log('Error: ' + err)
    })
})
```

效果如下：

![匹配设备](https://upload-images.jianshu.io/upload_images/18281896-ae49381c97050407.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更多资源

目前，已经有很多例子可以实践和借鉴，你可以在 [demos](https://github.com/WebBluetoothCG/demos) 或 [Web Bluetooth Samples](https://googlechrome.github.io/samples/web-bluetooth/) 查看更多例子，如：Web 蓝牙打印机、LED 显示器、赛车等。
