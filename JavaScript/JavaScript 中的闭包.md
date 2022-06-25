# JavaScript 中的闭包

先看一下一些指南对闭包给出的定义：

> [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)：一个函数和对其周围状态（**lexical environment，词法环境**）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是**闭包**（**closure**）。也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域。在 JavaScript 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来。

> 现代 JavaScript 教程：[闭包](https://zh.javascript.info/closure)是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后。

> 《JavaScript 权威指南》：从技术的角度讲，所有的 JavaScript 函数都是闭包。

汤姆大叔翻译的关于闭包的文章中的定义：

ECMAScript 中，闭包指的是：

- 从理论角度：所有的函数。因为它们都在创建的时候就将上层上下文的数据保存起来了。哪怕是简单的全局变量也是如此，因为函数中访问全局变量就相当于是在访问自由变量，这个时候使用最外层的作用域。
- 从实践角度：以下函数才算是闭包：
  - 即使创建它的上下文已经销毁，它仍然存在（比如，内部函数从父函数中返回）
  - 在代码中引用了自由变量

之所以所有的 JavaScript 函数都是闭包，是因为它们可以访问外部作用域，但是大多数函数没有利用闭包的作用：状态的持久性。因此，闭包有时也被称为有状态函数。

## 几个简单的例子

```js
function foo() {
  const i = 0
  function baz() {
    console.log(i++)
  }
  return baz
}
```

上面例子中，我们可以看到外部 foo 函数包含了内部 baz 函数，内部函数还引用了外部函数的变量，并被返回出去。

我们调用此函数，初始化指向一个变量：

```js
let f = foo()
```

foo 函数被运行，baz 函数将被创建，然后返回。此时，我们的 f 变量只关注一件事，那就是 baz 函数。

为了正确地将其可视化，下面是 f 变量的外观：

![内存地址环境](https://upload-images.jianshu.io/upload_images/18281896-0ab9e22eda3849f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它仍然引用 baz 函数，但是 baz 函数还可以访问外部 foo 函数中存在的 i 变量。这个内部函数，因为它将外部函数中的相关变量包含在其区域（又名作用域）中，所以被称为闭包：

![闭包](https://upload-images.jianshu.io/upload_images/18281896-a92796695d8fb68f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从 f 变量的角度来看，foo 函数已经被回收了。因为 f 变量现在指向一个函数，所以你可以像平常调用函数一样，调用它：

```js
f() // 0
f() // 1
f() // 2
```

在每次 foo 被调用时，都会创建一个新的作用域，以存储 foo 运行时的变量。所以每次调用都会加 1。

另一个例子：

```js
function foo() {
  const i = 0
  function baz() {
    console.log(i++)
  }
  return baz
}

let f = foo()
let f1 = foo()

f() // 0
f() // 1

f1() // 0
f1() // 1
```

函数 `f` 和 `f1` 是通过 `foo` 的不同调用创建的。因此，它们具有独立的外部词法环境，每一个都有自己的 `i`。

函数内部如果返回一个函数，执行后，它检测到内部函依赖于外部函的变量。发生这种情况时，运行时确保**即使外部函数消失了**，外部函数中所需的任何变量仍可用于内部函数。

## 一个常见的场景

```js
for (var i = 1; i < 5; i++) {
  setTimeout(function () {
    console.log(i)
  }, 1000)
}
```

输出：`11, 11, 11, 11, 11`。

原因：`i` 是声明在全局作用域中的，定时器中的匿名函数也是执行在全局作用域中，因此输出全为 11。

解决：我们可以让 `i` 在每次迭代的时候，都产生一个私有的作用域，在这个私有的作用域中保存当前 `i` 的值。

```js
// 立即执行函数
for (var i = 1; i <= 10; i++) {
  ;(function () {
    var j = i
    setTimeout(function () {
      console.log(j)
    }, 1000)
  })()
}
```

改造：将每次迭代的 `i` 作为实参传递给自执行函数，自执行函数中用变量去接收

```js
for (var i = 1; i <= 10; i++) {
  ;(function (j) {
    setTimeout(function () {
      console.log(j)
    }, 1000)
  })(i)
}
```

ES6 中的 `let` 和 `const` 可以解决这个问题，其会产生块级作用域

```js
for (let i = 1; i <= 10; i++) {
  setTimeout(function () {
    console.log(i)
  }, 1000)
}
```

还有一个优雅的方法解决这个问题，我们可能会忘了 `setTimeout` 还可以传递第三个参数：

```js
for (var i = 1; i <= 10; i++) {
  setTimeout(
    function (i) {
      console.log(i)
    },
    1000,
    i
  )
}
```

## 另一道面试题

```js
var data = []

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i)
  }
}

data[0]() // 3
data[1]() // 3
data[2]() // 3
```

当执行到 `data[0]` 函数的时候，`for` 循环已经执行完了，`i` 使用 `var` 声明，是全局变量，此时的值为 3。

## 闭包的优缺点

**优点**：

- 可以避免使用全局变量，防止全局变量污染
- 让外部访问函数内部变量成为可能

**缺点**：

- 会造成内存泄漏 — 有一块内存空间被长期占用，而不被释放
- 导致变量不会被垃圾回收机制回收，造成内存消耗

**闭包要做的最重要的事情是即使函数的环境急剧变化或消失，函数也可以继续工作**。创建函数时作用域内的所有变量均被封闭并加以保护，以确保该函数仍然有效。对于非动态的语言（例如 JavaScript），这种行为是必不可少的，在 JavaScript 中您经常动态地创建，修改和销毁对象。

此外，闭包是用 JavaScript 存储不能从外部访问的私有数据的唯一方法。它们是 UMD（universal module definition）模式的关键，UMD 模式经常用于只公开公共 API 但保持实现细节私有的库中，以防止与其他库或用户自己的代码发生名称冲突。

## 闭包的应用场景

### setTimeout

原生的 `setTimeout` 传递的第一个函数不能带参数，通过闭包可以实现传参效果。

```js
function handler(str, time) {
  setTimeout(function () {
    console.log(str)
  }, time)
}

handler('hello', 1000)
```

定时器中有一个匿名函数，该匿名函数就有涵盖 `handler` 函数作用域的闭包，因此当 1 秒之后，该匿名函数能输出 `'hello'`。

### 函数防抖

> 防抖：无论触发多少次事件，动作只会执行一次。

**思路**：在规定时间内未触发第二次，则执行

```js
function debounce(fn, delay) {
  // 利用闭包保存定时器
  let timer = null
  return function () {
    let context = this
    let arg = arguments
    // 在规定时间内再次触发会先清除定时器后再重设定时器
    clearTimeout(timer)
    timer = setTimeout(function () {
      fn.apply(context, arg)
    }, delay)
  }
}
```

### 封装私有变量

闭包的应用比较典型是定义模块，我们将操作函数暴露给外部，而细节隐藏在模块内部：

```js
function module() {
  const arr = []
  const add = (val) => typeof val == 'number' && arr.push(val)
  const get = (i) => (i < arr.length ? arr[i] : null)
  return {
    add,
    get
  }
}
const f = module()
f.add(1)
f.add(2)
f.add('hi')

f.get(1) // 2
f.get(2) // null
```

## 更多资料

- [发现 JavaScript 中闭包的强大威力](https://juejin.im/post/6844903769646317576)
- [JavaScript 深入之闭包](https://github.com/mqyqingfeng/Blog/issues/9)
- [我从来不理解 JavaScript 闭包，直到有人这样向我解释它...](https://zhuanlan.zhihu.com/p/56490498)
- [破解前端面试（80% 应聘者不及格系列）：从闭包说起](https://juejin.im/post/6844903474212143117#heading-0)
- [深入理解 JavaScript 系列（16）：闭包（Closures）](https://www.cnblogs.com/TomXu/archive/2012/01/31/2330252.html)
- [JavaScript 深入之闭包](https://github.com/mqyqingfeng/Blog/issues/9)
- [A simple guide to help you understand closures in JavaScript](https://medium.com/@prashantramnyc/javascript-closures-simplified-d0d23fa06ba4) (Prashant Ram)
- [Master the JavaScript Interview: What is a Closure?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36) (JavaScript Scene)
- [How do JavaScript closures work?](https://stackoverflow.com/questions/111102/how-do-javascript-closures-work)
