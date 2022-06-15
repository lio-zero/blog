# Node.js 中的 util.promisify() 方法

Node.js 内置的 `util` 模块有一个 [`promisify()` 方法](https://nodejs.org/api/util.html#util_util_promisify_original)，该方法将基于回调的函数转换为基于 Promise 的函数。这使您可以将 Promise 链和 `async/await` 与基于回调的 API 结合使用。

例如，Node.js 的 `fs` 模块在读取文件时，需要使用回调：

```js
const fs = require('fs')

fs.readFile('./package.json', function callback(err, buf) {
  const obj = JSON.parse(buf.toString('utf8'))
  console.log(obj.name) // 'Example' -> package.json 包名
})
```

我们可以使用 `util.promisify()` 将 `fs.readFile()` 的回调函数转换为返回 Promise 函数：

```js
const fs = require('fs')
const util = require('util')

// 将 fs.readFile() 转换为一个接受相同参数但返回 Promise 的函数。
const readFile = util.promisify(fs.readFile)

// 现在可以将 readFile() 与 await 一起使用！
const buf = await readFile('./package.json')

const obj = JSON.parse(buf.toString('utf8'))
console.log(obj.name) // 'Example'
```

## promisify 是如何工作的？

`util.promisify()` 在后台是如何工作的？npm 上有一个 [polyfill](https://www.npmjs.com/package/util.promisify)，您可以在这里阅读[完整的实现](https://github.com/ljharb/util.promisify/blob/master/implementation.js)。您也可以在这里找到 [Node.js 的实现](https://github.com/nodejs/node/blob/master/lib/internal/util.js#L277-L322)，不过为了便于理解，polyfill 更易于阅读。

`util.promisify()` 背后的关键思想是[向传入的参数添加回调函数](https://github.com/ljharb/util.promisify/blob/40a839a8db3d79699688d27f6613a827056428c8/implementation.js#L58-L59)。该回调函数解析或拒绝 promisified 函数返回的 Promise。

为了便于理解，下面是一个非常简化的 `util.promisify()` 自定义实现示例：

```js
const fs = require('fs')

// util.promisify() 的简化实现。不包括所有情况，不要在 prod 环境中使用此选项！
function promisify(fn) {
  return function () {
    const args = Array.prototype.slice.call(arguments)
    return new Promise((resolve, reject) => {
      fn.apply(
        this,
        [].concat(args).concat([
          (err, res) => {
            if (err != null) {
              return reject(err)
            }
            resolve(res)
          }
        ])
      )
    })
  }
}

// 将 fs.readFile() 转换为一个接受相同参数但返回 Promise 的函数。
const readFile = promisify(fs.readFile)

// 现在可以将 readFile() 与 await 一起使用！
const buf = await readFile('./package.json')

const obj = JSON.parse(buf.toString('utf8'))
console.log(obj.name) // 'Example'
```

那么这是什么意思呢？首先，`util.promisify()`向传入的参数添加 1 个额外参数，然后使用这些新参数调用原始函数。这意味着底层函数需要支持该数量的参数。因此，如果您要调用`myFn()`具有 2 个类型参数的 promisified 函数 `[String, Object]`，请确保原始函数支持`[String, Object, Function]`。

那么这意味着什么呢？首先，`util.promisify()` 向传入的参数添加一个额外参数，然后使用这些新参数调用原始函数。这意味着基础函数需要支持该数量的参数。因此，如果您使用 `[String, Object]` 类型的 2 个参数调用 promisified 函数 `myFn()`，请确保原始函数支持 `[String, Object, Function]`。

其次，`util.promisify()` 对函数上下文（`this`）有影响。

## 丢失上下文

丢失上下文（`this`）意味着函数调用以错误的值结束。丢失上下文是转换函数的常见问题：

```js
class MyClass {
  myCallbackFn(cb) {
    cb(null, this)
  }
}

const obj = new MyClass()
const promisified = require('util').promisify(obj.myCallbackFn)

const context = await promisified()
console.log(context) // 打印 undefined 而不是 MyClass 实例！
```

请记住，`this` 包含函数被调用时的属性的任何对象。因此，您可以通过将 promisified 函数设置为同一对象的属性来保留上下文：

```js
class MyClass {
  myCallbackFn(cb) {
    cb(null, this)
  }
}

const obj = new MyClass()
// 保留上下文，因为 promisified 是 obj 的属性
obj.promisified = require('util').promisify(obj.myCallbackFn)

const context = await obj.promisified()
console.log(context === obj) // true
```

## 更多资料

更多详细内容可以参阅 [Axel Rauschmayer 的文章](http://2ality.com/2017/05/util-promisify.html)。
