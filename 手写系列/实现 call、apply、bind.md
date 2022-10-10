# 实现 call、apply、bind

## Call

> [`call()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 方法使用一个指定的 `this` 值和单独指定的一个或多个参数来调用一个函数。

**实现思路**：将要改变 `this` 指向的函数挂到目标 `this` 上执行并返回。

```js
Function.prototype.myCall = function (ctx, ...args) {
  ctx = ctx === null || ctx === undefined ? globalThis : Object(ctx)
  // Symbol 是唯一的，防止 ctx 内存在重名 key
  const key = Symbol()
  // 之前，我们使用 ctx[key] = this，但这样在 ctx 中将存在调用 call 的函数
  // 我们可以使用 ES5 的 Object.defineProperty 添加 key
  // 并使用属性描述符 enumerable: false，表示 key 为不可枚举属性，也就不会显示在 ctx 中
  Object.defineProperty(ctx, key, {
    enumerable: false,
    value: this
  })

  const result = ctx[key](...args)
  delete ctx[key]
  return result
}
```

示例：

```js
function getUserInfo(data) {
  console.log(this.name, data)
}

const user = {
  name: 'IU',
  age: 18,
  love: 'me'
}

const data = [0, 1, 2]
getUserInfo.myCall(user, data) // IU [0, 1, 2]
```

## Apply

> [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法调用一个具有给定 `this` 值的函数，以及以一个副本（或类数组对象）的形式提供的参数。

**实现思路**：与 `call` 类似，但传递参数是一个数组。

```js
Function.prototype.myApply = function (ctx, args) {
  // apply 需要传递的是一个数组，如果传递的不是一个数组或使用了扩展运算符，将抛出报错。
  if (!Array.isArray(args))
    throw new TypeError('CreateListFromArrayLike called on non-object')

  ctx = ctx === null || ctx === undefined ? globalThis : Object(ctx)
  const key = Symbol()
  Object.defineProperty(ctx, key, {
    enumerable: false,
    value: this
  })
  let result
  // 是否有传递第二个参数
  if (args) {
    result = ctx[key](args)
  } else {
    result = ctx[key]()
  }
  delete ctx[key]
  return result
}
```

示例：

```js
getUserInfo.myApply(user, data) // IU 0 1 2
```

## Bind

> [`bind()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) 方法创建一个新的函数，当被调用时，将其 `this` 关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。

**实现思路**：类似 `call`，但返回的是函数。

```js
Function.prototype.myBind = function (ctx, ...outArgs) {
  ctx = ctx === null || ctx === undefined ? globalThis : Object(ctx)

  const key = Symbol()
  ctx[key] = this
  const _this = this

  return function F(...innerArgs) {
    // 处理函数使用 new 的情况
    if (this instanceof F) return new _this(...outArgs, ...innerArgs)

    return _this.apply(ctx, outArgs.concat(innerArgs))
  }
}
```

示例：

```js
function getUserInfo(args1, args2) {
  console.log(this.name, args1, args2)
}

const boundGetUserInfo = getUserInfo.bind(user)
boundGetUserInfo(data) // IU [ 0, 1, 2 ] undefined

const boundGetUserInfo2 = getUserInfo.myBind(user, 12)
boundGetUserInfo2(data) // IU 12 [ 0, 1, 2 ]
```

## 更多资料

- [JavaScript 深入之 call 和 apply 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/11)
- [JavaScript 深入之 bind 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/12)
- [JS 中的 call、apply、bind 方法详解](https://segmentfault.com/a/1190000018270750)
- [Implement your own — call(), apply() and bind() method in JavaScript](https://medium.com/@ankur_anand/implement-your-own-call-apply-and-bind-method-in-javascript-42cc85dba1b)
