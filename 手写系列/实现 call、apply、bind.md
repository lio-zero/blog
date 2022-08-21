# 实现 call、apply、bind

## Call

> [`call()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 方法使用一个指定的 `this` 值和单独指定的一个或多个参数来调用一个函数。

**实现思路**：将要改变 `this` 指向的函数挂到目标 `this` 上执行并返回

```js
Function.prototype.myCall = function (context) {
  context = context || window
  context.fn = this
  const arg = [...arguments].slice(1)
  const result = context.fn(...arg)
  delete context.fn
  return result
}
```

示例：

```js
function getUserInfo(...data) {
  console.log(this.name, ...data)
}

const user = {
  name: 'IU',
  age: 18,
  love: 'me'
}

const data = [0, 1, 2]
getUserInfo.myCall(user, data)
getUserInfo.myCall(user, ...data)
```

## Apply

> [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法调用一个具有给定 `this` 值的函数，以及以一个副本（或类数组对象）的形式提供的参数。

**实现思路**：与 `call` 类似，但传递参数是一个数组。

```js
Function.prototype.myApply = function (context) {
  context = context || window
  context.fn = this
  let result
  // 是否有传递第二个参数
  if (arguments[1]) {
    result = context.fn(arguments[1])
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```

示例：

```js
getUserInfo.myApply(user, data) // IU 0 1 2
getUserInfo.myApply(user, ...data) // IU
```

> **注意**：您可能注意到了，`apply` 需要传递的是一个数组，如果使用多个参数或扩展运算符，它将会报错，但是这里它还是能输出结果。

## Bind

> [`bind()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) 方法创建一个新的函数，当被调用时，将其 `this` 关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。

**实现思路**：类似 `call`，但返回的是函数

```js
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  const _this = this
  const arg = [...arguments].slice(1)
  return function F() {
    // 处理函数使用 new 的情况
    if (this instanceof F) {
      return new _this(...arg, ...arguments)
    }
    return _this.apply(context, arg.concat(...arguments))
  }
}
```

示例：

```js
const boundGetUserInfo = getUserInfo.bind(user)

boundGetUserInfo(data) // IU 2 [0, 1, 2]
boundGetUserInfo(...data) // IU 2 1 2 3

const boundGetUserInfo2 = getUserInfo.myBind(user, 2)

boundGetUserInfo2(data) // IU 2 [0, 1, 2]
boundGetUserInfo2(...data) // IU 2 1 2 3
```

## 更多资料

- [JavaScript 深入之 call 和 apply 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/11)
- [JavaScript 深入之 bind 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/12)
- [JS 中的 call、apply、bind 方法详解](https://segmentfault.com/a/1190000018270750)
- [Implement your own — call(), apply() and bind() method in JavaScript](https://medium.com/@ankur_anand/implement-your-own-call-apply-and-bind-method-in-javascript-42cc85dba1b)
