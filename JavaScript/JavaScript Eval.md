# JavaScript Eval

> 先说明。[`eval`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval) 已过时，尽量不要在使用它。

`eval` 执行包含代码的字符串，例如：

```js
eval('var x = "Hello eval!"')
x // 'Hello eval!'
```

`eval` 存在几个问题：

- 安全性：您的字符串可以通过第三方脚本或用户输入注入其他命令。
- 调试：很难调试错误，您没有行号或明显的故障点。
- 优化：JavaScript 解释器不一定能预编译代码，因为它可能会发生变化。虽然解释器的效率越来越高，但几乎可以肯定它的运行速度会比本地代码慢。
- 性能：非常耗性能，它会执行 2 次，一次解析成 JS 语句，一次执行。

不幸的是，`eval` 非常强大，但经验不足的开发人员很容易过度使用该命令。

尽管有警告，但 `eval` 仍然可以工作（即使在严格模式下），但您通常可以避免它。

过去，它主要用于对 `JSON` 字符串进行反序列化：

```js
const json = '{"name":"O.O","age":20}'

eval('(' + json + ')') // {name: 'O.O', age: 20}
```

但现在我们有了更安全的 `JSON.parse` 方法：

```js
JSON.parse(json) // {name: 'O.O', age: 20}
```

## 更多资料

更多详细内容可以查看：

- [Eval：执行代码字符串](https://zh.javascript.info/eval)
- [Why is using the JavaScript eval function a bad idea?](https://stackoverflow.com/questions/86513/why-is-using-the-javascript-eval-function-a-bad-idea)
- [既然不提倡在 JavaScript 中使用 eval，为什么还要设计出来？](https://www.zhihu.com/question/545741796)
- [Reasons Why You Should Never Use eval() in JavaScript](https://www.digitalocean.com/community/tutorials/js-eval)
