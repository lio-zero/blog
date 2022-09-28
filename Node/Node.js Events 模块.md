# Node.js Events 模块

如果您在浏览器中使用 JavaScript，您就会知道用户的交互有多少是通过事件来处理的：鼠标单击、键盘按钮按下、对鼠标移动的反应等。

在后端，Node 为我们提供了使用 [`events`](https://nodejs.org/dist/latest-v16.x/docs/api/events.html) 模块构建类似系统的选项，您可以在其中创建、触发和监听自己的事件。

这个模块为我们提供了 `EventEmitter` 类，这是 Node.js 处理事件的关键。

## EventEmitter 对象

使用以下语法初始化 `EventEmitter` 对象：

```js
const EventEmitter = require('events')
const emitter = new EventEmitter()
```

除了许多其他对象外，该对象公开了 `on` 和 `emit` 方法。

- `emit` 用于触发事件
- `on` 用于添加将在触发事件时执行的回调函数。它有另一个别名 `addListener` 方法。

语法：

```js
emitter.on(eventName, callback)
emitter.emit(eventName)
```

## 发出和监听事件

您可以使用 `EventEmitter` 对象将事件处理程序分配给您自己的事件。

例如，创建一个 `say` 事件，它仅在控制台打印一条消息：

```js
emitter.on('say', () => {
  console.log('Hello')
})
```

当我们运行：

```js
emitter.emit('say')
```

事件处理程序函数被触发，控制台打印了 `'Hello'`。

## 向事件传递参数

通过将参数作为附加参数传递给 `emit()`，可以将参数传递给事件处理程序：

```js
emitter.on('say', (name) => {
  console.log(`Hello ${name}`)
})

emitter.emit('say', 'O.O')
```

多个参数:

```js
emitter.on('say', (name, age) => {
  console.log(`我的名字叫 ${name}，今年 ${age} 岁`)
})

emitter.emit('say', 'O.O', 20)
```

## 获取特定事件的事件处理程序

`emitter.listeners` 返回名为 `eventName` 事件的监听器数组的副本。

```js
emitter.on('say', () => {})
emitter.on('say', () => {})

emitter.listeners('say') // [ [Function: callback], [Function: callback] ]
```

## 仅监听一次事件

EventEmitter 对象还公开了 `once()` 方法，您可以使用该方法创建一次性事件监听器。

一旦触发该事件，监听器将停止监听。

例如：

```js
emitter.once('say', () => {
  console.log(`Hello`)
})

emitter.emit('say')
emitter.emit('say') // 不会触发
```

## 删除事件监听器

创建事件监听器后，可以使用 `removeListener()` 方法将其删除。

为此，我们必须先引用 `on` 的回调函数，也就是将其保存在变量中。

在这个例子中：

```js
emitter.on('say', () => {
  console.log('Hello')
})
```

提取回调：

```js
const callback = () => {
  console.log('Hello')
}

emitter.on('say', callback)
```

以便稍后您可以调用：

```js
emitter.removeListener('say', callback)

// 使用 eventNames 方法可以查看 say 事件是否被移除，这个方法下面会讲到
emitter.eventNames()
```

Node 10 还引入 `emitter.removeListener` 的别名 `emitter.off`：

```js
emitter.off('say', callback)
```

它们做着同样的事情。

您还可以使用以下方法一次性删除监听特定事件的 Event Emitter 对象的所有监听器：

```js
emitter.removeAllListeners('say')
```

## 设置 EventListener 对象的最大监听器数量

默认情况下，如果为特定事件添加了 10 个以上的监听器，EventEmitter 将打印警告。例如：

```js
emitter.on('say', () => {
  console.log('Hello')
})

emitter.on('say', () => {
  console.log('Hello')
})
// 此处省略 9 个以上的监听器
```

这是一个有用的默认值，有助于查找内存泄漏。

`emitter.setMaxListeners()` 方法允许修改此特定 EventEmitter 实例的限制。该值可以设置为 `Infinity`（或 `0`），表示监听器的数量不受限制。

```js
emitter.setMaxListeners(Infinity)
```

有 `get` 就有 `set`，`getMaxListeners` 方法可以 EventListener 对象的最大监听器数量。

```js
emitter.getMaxListeners(Infinity)
```

## 获取注册的事件

对 EventEmitter 对象实例调用 `eventNames()` 方法，它将返回一个字符串数组，该数组表示当前 EventListener 上注册的事件：

```js
emitter.on('say', () => {
  console.log('Hello')
})

emitter.eventNames() // [ 'say' ]
```

`listenerCount()` 返回作为参数传递的事件的监听器计数：

```js
emitter.listenerCount('say') // 1
```

## 在其他监听器之前/之后添加更多监听器

如果您有多个监听器，它们的顺序可能很重要。

EventEmitter 对象实例提供了一些处理顺序的方法。

- 当使用 `on` 或 `addListener` 添加监听器时，它将被添加到监听器队列的开头，并最后调用。使用 `prependListener` 可以在其他监听器之前添加和调用它。
- 当使用 `once` 添加监听器时，它将被添加到监听器队列的开头，并最后调用。使用 `prependOnceListener` 可以在其他监听器之前添加和调用它。

## `newListener` 事件

在将监听器添加到其内部监听器数组之前，EventEmitter 实例将触发 `newListener` 事件。

为 `newListener` 事件注册的监听器将传递事件名称和对添加的监听器的引用。

```js
const EventEmitter = require('events')
const emitter = new EventEmitter()

// 仅监听一次事件，这样我们就不会永远循环
emitter.once('newListener', (event, listener) => {
  console.log(event, listener) // event [ƒ ()]
  if (event === 'event') {
    // 在前面插入新的监听器
    emitter.on('event', () => {
      console.log('B')
    })
  }
})

emitter.on('event', () => {
  console.log('A')
})
emitter.emit('event')
// B
// A
```

我们可以看到，先打印 `B` 在打印 `A`。

这是因为事件在添加监听器之前触发有一个微妙但重要的副作用：在 `newListener` 回调中注册到相同名称的任何其他监听器都会在添加过程中的监听器之前插入。

## 捕获 EventEmitter 的错误事件

监听 `error` 事件可以捕获 EventEmitter 的错误事件。

```js
const EventEmitter = require('events')
const emitter = new EventEmitter()

emitter.on('error', (err) => {
  console.error('whoops! there was an error')
})

emitter.emit('error', new Error('whoops!'))
```

## 最后

以上是 Events 模块处理 Node 事件的一些简单用法。详细内容请查看文档，后续会不断进行补充。
