# 使用 Node.js 显示整个对象

在大多数情况下，我使用 JavaScript 的 `console.log()`。它可以快速打印出我们想要的东西。

但当我们想要处理更大的对象时，特别是嵌套三层或更多层的任何对象。这是我所指的：

```js
const myDeepObject = {
  one: {
    two: {
      three: {
        four: {
          five: {
            six: 'too too deep'
          }
        }
      }
    }
  }
}
```

如果我们尝试使用 `console.log` 访问这个深层次的对象，我们最终会在第三层得到一个 `[Object]`：

```js
console.log(myDeepObject) // { one: { two: { three: [Object] } } }
```

如果我们想要显示一个对象的整体，这样的效果显然不尽人意。

有效的解决方案是说那个`util` 模块的 `inspect()` 方法。

```js
const { inspect } = require('util')

inspect(myDeepObject, { depth: null })
```

我们将得到：

```js
/*
{
  one: {
    two: {
      three: { four: { five: { six: 'too too deep' } } }
    }
  }
}
*/
```

还有一种更多简便优雅方法，不需要额外的导入。`console` 有一个名为 `dir` 的方法，在显示对象时设置深度限制（或者不设置限制），该方法的语法与 `util.inspect()` 类似：

```js
console.dir(myDeepObject, { depth: null })
```

## JSON.stringify

我们还可以使用 `JSON.stringify()` 输出的内容更具可读性。

> 详细内容请查看：[格式化输出 JSON](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%A0%BC%E5%BC%8F%E5%8C%96%E8%BE%93%E5%87%BA%20JSON.md)

```js
const think = { eat: '🥩', sleep: '😴' }

console.log(JSON.stringify(think, null, 2))
/*
{
  "eat": "🥩",
  "sleep": "😴"
}
*/
```
