# 实现 JSON.parse

之前在 [JavaScript Eval](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20Eval.md) 中有提到过，在没有 `JSON.parse` 方法之前，我们使用 eval 来对 JSON 字符串进行反序列化。

偶然间发现了一篇 [JSON.parse 三种实现方式](https://github.com/youngwind/blog/issues/115)，其中讨论了几种实现 `JSON.parse` 的方法。

其中讨论区提到了一种使用 `new Function` 实现的方法，如下：

```js
const json = '{"name":"O.O", "age":18}'
const obj = new Function('return ' + json)() // {name: 'O.O', age: 18}
```

这种实现方式很简便，但需要注意的是，如果网站开启了 CSP（内容安全策略），将会报错：

> Uncaught EvalError: Refused to evaluate a string as JavaScript because 'unsafe-eval' is not an allowed source of script in the following Content Security Policy directive: "script-src github.githubassets.com".

另外，你也可以阅读 [@ascoders](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/205.%E7%B2%BE%E8%AF%BB%E3%80%8AJS%20with%20%E8%AF%AD%E6%B3%95%E3%80%8B.md) 的 [精读《手写 JSON Parser》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/139.%E7%B2%BE%E8%AF%BB%E3%80%8A%E6%89%8B%E5%86%99%20JSON%20Parser%E3%80%8B.md)
