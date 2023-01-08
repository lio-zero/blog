# requestIdleCallback

[`requestIdleCallback`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback) 是浏览器提供给我们的空闲调度算法，它维护着一个队列，允许浏览器进入等待状态，直到它识别出一个空闲期。

`requestIdleCallback` 接收两个参数：回调函数和一个可选的选项对象。

- 回调函数会在浏览器空闲时调用，它接收一个参数，表示空闲时间的信息。这个函数通常用于执行那些计算量较大的但又不是很紧急的任务，比如统计或者埋点。不要去影响浏览器中优先级较高的任务，比如动画绘制、用户输入等。
- 选项对象可以用来设置一些可选的参数。例如，你可以用 `timeout` 选项来设置超时时间，表示如果回调函数在超时时间内没有被调用，那么它就会被立即执行。

下面是一个使用 `requestIdleCallback` 的例子：

```js
requestIdleCallback(
  () => {
    console.log('All work and no play makes Jack a dull boy!')
  },
  { timeout: 1000 }
)
```

这个例子会在浏览器空闲时调用回调函数，如果在 1 秒钟内浏览器没有空闲，那么回调函数就会立即执行。

## 模拟 `requestIdleCallback`

如果浏览器不支持 `requestIdleCallback`，你可以通过模拟这个 API 来达到同样的效果。

下面是一个模拟 `requestIdleCallback` 的例子：

```js
window.requestIdleCallback =
  window.requestIdleCallback ||
  function (cb) {
    return setTimeout(() => {
      const start = Date.now()
      cb({
        didTimeout: false,
        timeRemaining: () => {
          return Math.max(0, 50 - (Date.now() - start))
        }
      })
    }, 1)
  }
```

我们使用了 `setTimeout` 来模拟实现一个 `requestIdleCallback`，并通过计算时间差来模拟空闲时间。

`cancelIdleCallback` 使用 `clearTimeout` 模拟实现，用于取消已注册的回调函数：

```js
window.cancelIdleCallback =
  window.cancelIdleCallback ||
  function (id) {
    clearTimeout(id)
  }
```

需要注意，以上只是一个简单的模拟实现，它并不能与真正的 `requestIdleCallback` 完全相同。你可以根据自己的需要来改进这个模拟实现。

> 以上示例来自 [requestIdleCallback.js](https://gist.github.com/paullewis/55efe5d6f05434a96c36)。

## 使用案例

在很多前端框架和工具中，也有很多类似的实现。

[swr](https://github.com/vercel/swr) 有一个简单的模拟实现：

```js
const rIC = window['requestIdleCallback'] || ((f) => setTimeout(f, 1))
```

使用 [swr](https://github.com/vercel/swr) 中进行资源请求的 `revalidate`，也是通过 `rIC` 来提高性能。

React 的时间分片也是基于类似 `rIC` 而实现，然而因为 `rIC` 的兼容性及 50ms 流畅问题，React 自己实现了一个 [scheduler](https://github.com/facebook/react/tree/main/packages/scheduler)。

## 关于 50ms 问题

在某些浏览器上，`requestIdleCallback` 回调函数会在 50ms 的延迟之后执行。这个问题会导致回调函数的执行可能会被延迟，影响浏览器的性能。

这个问题在 Google Chrome 和 Microsoft Edge 中比较常见。它的原因是这些浏览器的 JavaScript 引擎会将所有的回调函数放在同一个任务队列中，并且每个任务之间会有 50ms 的延迟。这意味着，如果你希望在这些浏览器中避免 50ms 问题，你可以考虑使用其他方法来替代 `requestIdleCallback`。

例如，你可以使用 `requestAnimationFrame` 来替代 `requestIdleCallback`。[`requestAnimationFrame`](https://github.com/lio-zero/blog/blob/main/Web%20API/requestAnimationFrame.md) 是浏览器用于动画渲染的 API，它会在浏览器每一帧渲染之前调用回调函数。这意味着，你可以在每一帧渲染之前执行任务，避免 50ms 的延迟。

你也可以使用 `setTimeout` 来替代 `requestIdleCallback`。`setTimeout` 可以设置回调函数在特定的时间之后执行，你可以设置一个较小的时间值，来避免 50ms 的延迟。

## `requestIdleCallback` 需要注意的地方

使用 `requestIdleCallback` 时，要注意一些事项：

- 它并不是所有浏览器都支持的，你需要检查浏览器是否支持这个 API。
- 回调函数可能会在浏览器空闲时被调用，也可能会在浏览器忙碌时被调用。所以你不能保证回调函数一定会在浏览器空闲时被调用。
- 回调函数的执行可能会被延迟（50ms 问题），所以你不能依赖它来执行时间敏感的任务。
- 回调函数可能会被多次调用，所以你需要注意避免重复执行。
- 回调函数中不要操作 DOM，因为它本来就是利用的重排重绘后的间隙空闲时间，重新操作 DOM 又会造成重排重绘。
- 如果回调函数执行时间过长，可能会影响浏览器的性能。所以你需要尽量保证回调函数的执行时间尽量短。

## 最后

总的来说，`requestIdleCallback` 是一个很好的用来执行不是很重要的任务的方法，但是它并不适合所有情况。你需要谨慎地使用它，并且要注意上面提到的事项。

## 更多资源

- [Cooperative Scheduling of Background Tasks](https://w3c.github.io/requestidlecallback/#idle-periods)
- [Cooperative Scheduling with requestIdleCallback](https://hacks.mozilla.org/2016/11/cooperative-scheduling-with-requestidlecallback/)
