# 实现一个 EventEmitter

EventEmitter 是一种用于分发事件的机制，它允许你在应用程序中的不同部分之间分发事件，并为每个事件定义回调函数。

在 Node.js 中，`EventEmitter` 是 Node `events` 模块提供了一个类，它是实现 Node.js 事件驱动的基础。

实现 `EventEmitter` 类，我们需要知道其内部提供的一些方法，我在 [Node Events 模块](https://github.com/lio-zero/blog/blob/main/Node/Node.js%20Events%20%E6%A8%A1%E5%9D%97.md)有介绍到，本文不过多解释。

以下是它的一个简单实现。

首先，我们需要定义一个类来表示 EventEmitter：

```js
class EventEmitter {
  constructor() {
    this.events = {}
  }
}
```

接下来，我们需要为 `EventEmitter` 类添加一些方法来实现事件的分发。

```js
class EventEmitter {
  // ...

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = []
    }
    this.events[event].push(callback)
  }

  addListener(event, callback) {
    this.on(type, handler)
  }

  prependListener(type, handler) {
    if (!this.events[type]) {
      this.events[type] = []
    }
    this.events[type].unshift(handler)
  }
}
```

`addListener` 是 `on` 的别名，做着同样的工作。`prependListener` 类似，只不过它可以在其他监听器之前添加和调用。

然后是 `emit` 方法，它用于分发事件，并调用所有已注册的回调函数：

```js
class EventEmitter {
  // ...

  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach((callback) => {
        callback(data)
      })
    }
  }
}
```

我们可以添加 `off` 方法来取消事件的监听：

```js
class EventEmitter {
  // ...

  removeListener(type, handler) {
    if (!this.events[type]) {
      return
    }
    this.events[type] = this.events[type].filter((item) => item !== handler)
  }

  off(type, handler) {
    this.removeListener(type, handler)
  }
}
```

`off` 是 `removeListener` 的别名。

添加 `once` 方法，该方法在分发事件后立即取消注册：

```js
class EventEmitter {
  // ...

  once(event, callback) {
    const cb = (...args) => {
      callback(...args)
      this.off(event, cb)
    }
    this.on(event, cb)
  }
}
```

我们实现到这里，一个基本的 `EventEmitter` 类就已经实现了。

使用示例：

```js
const emitter = new EventEmitter()

// 在组件 A 中注册事件监听
emitter.on('someEvent', (data) => {
  console.log(data) // 'Hello, World!'
})

// 在组件 B 中分发事件
emitter.emit('someEvent', 'Hello, World!')

// 使用 off 方法来取消事件监听
// emitter.off('someEvent', callback)
```

当然，这并不是 Node `EventEmitter` 类的全部实现，它还有更多功能、逻辑需要处理。详细可以查看源码 [lib/events.js EventEmitter 的实现](https://github.com/nodejs/node/blob/main/lib/events.js#L218)。

## EventBus 和 EventEmitter 的区别

如果你看过[实现一个 Event Bus](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%20Event%20Bus.md) 这篇文章，你会发现它们实现上如此类似，它们都是用于分发事件的机制，但它们并不完全相同。

EventBus 是一种轻量级的事件分发机制，通常用于在单个应用程序中的不同部分之间传递消息。EventBus 通常实现为单例，并提供一些方法来注册和分发事件，例如 `on`、`emit` 和 `off`。

EventEmitter 是一种用于分发事件的机制，常用于 Node.js 中的模块间通信。EventEmitter 通常实现为一个类，提供类似于 EventBus 的方法来注册和分发事件，同样也有 `on`、`emit` 和 `off`。

EventBus 和 EventEmitter 都是发布订阅者模式（也称为观察者模式）的实现，都提供了 `on` 和 `emit` 等方法，用于注册和分发事件。

总的来说，EventBus 和 EventEmitter 都是用于在单个应用程序中的不同部分之间分发事件的机制。但它们的实现方式不同，EventBus 通常是单例，而 EventEmitter 通常是类。
