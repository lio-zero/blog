# JavaScript 中的自定义事件

在 JavaScript 中，可以通过两种方式创建自定义事件：

- 使用 [`Event`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/Event) 构造函数
- 使用 [`CustomEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent) 构造函数

也可以使用 `document.createEvent` 来创建自定义事件，但从函数返回的对象所公开的大多数方法已被弃用，这里就不进行介绍，如需了解请查阅 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createEvent)。

## 使用 `Event` 构造函数

可以使用 `Event` 构造函数创建自定义事件，如下所示：

```js
const myEvent = new Event('myevent', {
  bubbles: true,
  cancelable: true,
  composed: false
})
```

在上面的代码片段中，我们通过将事件名称传递给 `Event` 构造函数创建了一个事件 `myevent`。事件名称不区分大小写，因此 `myevent`、`myEvent` 和 `MyEvent` 都相同。

`Event` 构造函数还接受一个对象，该对象指定与事件有关的一些重要属性。

### `bubbles` 属性

`bubbles` 属性指定事件是否应该向上传播到父元素。将此设置为 `true` 意味着，如果在子元素中调度事件，则父元素可以监听该事件并基于该事件执行操作。这是大多数原生 DOM 事件的行为，但对于自定义事件，它默认设置为 `false`。

如果只希望在特定元素上调度事件，可以通过 `event.stopPropagation()` 停止事件的传播。

### `cancelable` 属性

顾名思义，`cancelable` 指定事件是否可取消。

原生 DOM 事件在默认情况下是可取消的，因此您可以对它们调用 `event.preventDefault()`，这将阻止事件的默认操作。如果自定义事件的 `cancelable` 设置为 `false`，则调用 `event.preventDefault()` 将不会执行任何操作。

### `composed` 属性

`composed` 属性指定事件是否应从 shadow DOM（使用 Web 组件时创建）冒泡到真实 DOM。如果 `bubbles` 设置为 `false`，则此属性的值无关紧要，因为您明确告诉事件不要向上冒泡。但是，如果要在 Web 组件中分派自定义事件并在真实 DOM 中的父元素上监听它，则需要将 `composed` 属性设置为 `true`。

使用此方法的一个缺点是无法将数据发送到监听器。然而，在大多数应用程序中，我们希望能够从事件发送到监听器的位置发送数据。为此，我们可以使用 `CustomEvent` 创建自定义事件，并传递数据。

您也不能使用原生 DOM 事件发送数据，只能从事件的目标获取数据。

## 使用 `CustomEvent` 构造函数

我们还可以使用 `CustomEvent` 构造函数创建自定义事件：

```js
const myEvent = new CustomEvent('myevent', {
  detail: {},
  bubbles: true,
  cancelable: true,
  composed: false
})
```

如上所示，通过 `CustomEvent` 构造函数创建自定义事件与使用 `Event` 构造函数创建事件类似。**唯一的区别**在于作为第二个参数传递给构造函数的对象。

当使用 `Event` 构造函数创建事件时，我们受到一个限制，即我们无法通过事件将数据传递给监听器。在这里，任何需要传递给监听器的数据都可以在 `detail` 属性中传递，该属性是在初始化事件时创建的。

## 在 JavaScript 中调度自定义事件

创建事件后，您需要能够调度它们。事件可以分派到任何扩展的对象，`EventTarget` 包括所有 HTML 元素、`document`、`window`等。

您可以像这样调度自定义事件：

```js
const myEvent = new CustomEvent('myevent', {
  detail: {},
  bubbles: true,
  cancelable: true,
  composed: false
})
```

要监听自定义事件，请向要监听的元素添加一个事件监听器，就像使用原生 DOM 事件一样。

```js
const ele = document.querySelector('#someElement')

ele.addEventListener('myevent', (event) => {
  console.log('我正在监听一个自定义事件')
})
```

## 派发事件

运行上面代码片段，你会发现不会有任何效果，这是因为还没有为目标元素派发自定义事件。

[`dispatchEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/dispatchEvent) 方法向一个指定的事件目标派发一个事件，并以合适的顺序同步调用目标元素相关的事件处理函数。

```js
ele.dispatchEvent(MyEvent)
```

运行后，将正常输出：**我正在监听一个自定义事件**。

这里有一个[示例](https://codepen.io/lio-zero/pen/mdBBxJZ)，展示如何在 JavaScript 中使用自定义事件。

## 更多资料

- [Creating and triggering events](https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events)
- [创建自定义事件](https://zh.javascript.info/dispatch-events)
