# 模拟 setInterval

`setInterval` 函数执行任务的间隔时间是不精确的。这意味着，即使你设置了每隔 1 秒钟执行一次任务，实际上任务的执行间隔可能会有所偏差。

这种偏差的原因是浏览器的 JavaScript 引擎可能会被其他进程或任务占用，导致定时器的执行被推迟。

而模拟 `setInterval` 的目的就是为了解决这个问题。

## 使用 `arguments.callee`

可以使用 [`arguments.callee`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Function/arguments) 属性来引用当前正在执行的函数。

```js
// 这里不能使用箭头函数，因为箭头函数没有 `argument` 对象
setTimeout(function () {
  console.log(new Date())
  setTimeout(arguments.callee, 1000)
}, 0)
```

但是，这种方式不推荐使用。

这是因为 `argument` 属性在 ECMAScript 5 中已被废弃，并且在严格模式下会抛出错误。

## 递归 + `setTimeout()`

如果你需要更精确的定时执行任务，可以使用 `setTimeout` 函数递归调用的方式来模拟周期性执行任务。这样，每次任务执行完后都会重新设置一个新的定时器，以保证下一次任务的执行间隔是 1 秒钟。

```js
function sayHello() {
  console.log('Hello')
  setTimeout(sayHello, 1000)
}

sayHello()
```

## 使用 `performance.now()` 或 `Date.now()`

使用 `performance.now()` 或 `Date.now()` 方法来计算时间间隔，并在适当的时候执行任务。

```js
let startTime = performance.now()

function sayHello() {
  console.log('Hello')
  startTime = performance.now()
}

function scheduleHello() {
  let currentTime = performance.now()
  let elapsedTime = currentTime - startTime
  if (elapsedTime >= 1000) {
    sayHello()
  }
  requestAnimationFrame(scheduleHello)
}

scheduleHello()
```

这种方法的优点是可以保证在浏览器中执行任务的间隔更加精确，但是会消耗更多的 CPU 资源。
