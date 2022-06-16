# 实现 sleep 函数

`sleep()` 函数的作用是让一段程序进入不活跃状态并持续一段时间，很多编程语言都有自己的 `sleep` 方法，但 JavaScript 没有。

最接近的是 `setTimeout`，它在指定的一段时间后执行回调内的某些操作：

```js
const printNums = () => {
  console.log(1)
  setTimeout(() => console.log(2), 500)
  console.log(3)
}

printNums()
// 1
// 3
// 2
```

但你也可能也注意到了，它执行的顺序跟实际 `sleep` 不符合。

## 同步版本

使用 `new Date().getTime()` 可以在 `while` 循环中用于暂停执行一段时间。

```js
const sleepSync = (ms) => {
  const end = new Date().getTime() + ms
  while (new Date().getTime() < end) {
    /* do nothing */
  }
}

const printNums = () => {
  console.log(1)
  sleepSync(1000)
  console.log(2)
  console.log(3)
}

printNums()
```

## 异步版本

ES6 新增的 Promise API 和 ES7 的 `async/await` 可以方便我们实现的 `sleep` 方法：

```js
const sleep = (ms) => new Promise((r) => setTimeout(r, ms))

async function demo() {
  for (let i = 0; i < 5; i++) {
    console.log(`Waiting ${i} seconds...`)
    await sleep(i * 1000)
    console.log('1')
  }
  console.log('Done')
}

demo()
```

注意：

- `sleep` 返回的函数必须在异步函数中执行，并且必须使用 `await` 调用。
- `await` 仅暂停当前异步函数。这意味着它不会阻止脚本其余部分的执行，这在绝大多数情况下都是您想要的。

## 更多资料

- [What is the JavaScript version of sleep()?](https://stackoverflow.com/questions/951021/what-is-the-javascript-version-of-sleep)
