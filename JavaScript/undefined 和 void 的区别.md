# undefined 和 void 的区别

`void` 是一个运算符，它对给定表达式求值，然后返回 `undefined`。

## 区别

在支持 ES5 的现代浏览器中，直接使用 `void` 运算符和 `undefined` 的值没有区别：

```js
void 0 === undefined // true
void 1 === undefined // true
void 'Foo' === undefined // true
```

但是，在运行 ES3 引擎的旧浏览器中，`undefined` 是一个全局属性，可以更改。

```js
// 在 ES3 中
console.log(undefined)
var undefined = 'foo'
console.log(undefined) // 'foo'
```

另一方面，不可能重写 `void` 操作符。因此，`void` 被用作 `undefined` 值的替换，以安全的方式获取 `undefined` 值。

在 ES5 中，不可能重写 `undefined`，因为它被 [`Writeable`](http://es5.github.io/#x15.1.1.3) 设置为 `false`。

## 扩展

- `void`是一个操作符，而不是一个函数。因此，我们不需要将表达式括在括号中。`void 0` 相当于 `void(0)`。

- 有些缩微器使用 `void 0` 来缩短 `undefined` 的长度。

- 如果使用立即调用的函数表达式（称为 IIFE），则可以使用 `void` 将 `function` 关键字视为表达式，而不是声明。

假设我们要执行以下 IIFE 函数：

```js
function run() {
  console.log('Executed')
}()
```

那么我们可以使用 `void`

```js
void (function run() {
  console.log('Executed')
})()
```

或将函数用括号括起来，如下所示：

```js
;(function run() {
  console.log('Executed')
})()
```

- 当与箭头函数一起使用时，可以使用 `void` 来避免副作用。

正如我们所知，ES6 箭头函数允许通过省略函数体中的大括号来使用函数的返回值。

```js
const sum = (a, b) => a + b
```

在某些情况下，我们不打算使用函数的返回值，因为它可能导致不同的行为。假设我们要处理一个按钮的点击事件：

```js
button.onclick = () => doSomething()
```

它照常工作，直到有人更改 `doSomething`，并使其返回 `boolean`。在返回 `false` 的情况下，将跳过 `click` 事件的默认行为，这可能是您不希望看到的。

将结果传递给 `void` 将确保无论执行函数的结果如何，它都不会改变箭头函数的行为：

```js
button.onclick = () => void doSomething()
```

在 React、Svelte 等现代库中可以看到使用 `void` 和 箭头函数的优势。

这些库允许我们在将组件装入 DOM 之后立即执行函数。例如，React 提供 [`useEffect`](https://reactjs.org/docs/hooks-reference.html#useeffect)，Svelte 具有 [`onMount`](https://svelte.dev/docs#onMount)。

如果我们在回调函数中返回一个函数，那么该函数将被调用以清理内容，在组件从屏幕上移除之前释放内存。

```js
// React 示例代码
useEffect(() => doSomething())
```

它可以在运行时产生 bug。为了避免这种情况，我们可以使用 `void`

```js
useEffect(() => void doSomething())
```

或使用大括号：

```js
useEffect(() => {
  doSomething()
})
```

## 建议

在以 `javascript:` 为前缀的 URL 中使用了 `void` 运算符。

默认情况下，浏览器将在遵循 `javascript:` URI 时计算代码，然后用返回值替换页面内容。

为了防止出现默认行为，代码必须返回 `undefined`。这就是我们看到以下代码的原因：

```html
<a href="javascript: void(0);" onclick="doSomething"> ... </a>
```

现在，不推荐使用 `javascript:` 协议。由于用户可以将未初始化的输入放入事件处理程序中，因此可能会产生安全问题：

```html
<a href="javascript: alert('unsanitized input')">...</a>
```

从 v16.9.0 开始，React 还[反对](https://reactjs.org/blog/2019/08/08/react-v16.9.0.html#deprecating-javascript-urls)使用 `javascript:` URL。
