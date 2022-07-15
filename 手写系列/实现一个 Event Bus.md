# 实现一个 Event Bus

截取 [EventBus](https://github.com/greenrobot/EventBus) 的定义：

> EventBus 能够简化各组件间的通信，能有效的分离事件发送方和接收方，能够避免复杂且容易出错的依赖关系和生命周期问题。

简单的讲，组件间的通信，是一个触发与监听的过程。

以下是一个简单的实现：

```js
class EventEmitter {
  constructor() {
    // 存储事件
    this.events = this.events || new Map()
  }
  // 监听事件
  on(type, handler) {
    if (!this.events.get(type)) {
      this.events.set(type, handler)
    }
  }
  // 取消事件
  off(type) {
    if (this.events.get(type)) {
      this.events.delete(type)
    }
  }
  // 触发事件
  emit(type) {
    let handle = this.events.get(type)
    handle && handle.apply(this, [...arguments].slice(1))
  }
}
```

示例：

```js
let emitter = new EventEmitter()
const foo = (name) => {
  console.log(name)
}

emitter.on('name', foo)

// 取消监听
// emitter.off('name', foo)
// 清空所有事件
// emitter.events.clear()

emitter.emit('name', { name: 'O.O' })
```

你可以根据 [mitt](https://github.com/developit/mitt/blob/main/src/index.ts)，实现一个更加完美的 Event Bus。核心代码如下：

```js
function mitt(all) {
  all = all || new Map()

  return {
    all,

    on(type, handler) {
      const handlers = all.get(type)
      if (handlers) {
        handlers.push(handler)
      } else {
        all.set(type, [handler])
      }
    },

    off(type, handler) {
      const handlers = all.get(type)
      if (handlers) {
        if (handler) {
          handlers.splice(handlers.indexOf(handler) >>> 0, 1)
        } else {
          all.set(type, [])
        }
      }
    },

    emit(type, evt) {
      let handlers = all.get(type)
      if (handlers) {
        handlers.slice().map((handler) => {
          handler(evt)
        })
      }

      handlers = all.get('*')
      if (handlers) {
        handlers.slice().map((handler) => {
          handler(type, evt)
        })
      }
    }
  }
}
```

我们所实现的 Event Bus 对同一事件的监听进行过滤，而 mitt 将多次监听进行收集。

## 更多资料

[Node 的 EventEmitter 源码](https://github.com/nodejs/node/blob/main/lib/events.js)
