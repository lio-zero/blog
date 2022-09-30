# null 与 undefined 的区别

`undefined` 表示一个变量已经声明，但没有被赋给任何值，包括 `null`。

```js
let foo
console.log(foo) // undefined

// 对象没有赋值的属性
let obj = {}
obj.name // undefined
```

调用函数时没有传递需要使用到的参数，没有明确的返回值，都为 `undefined`。

```js
function foo(a) {
  console.log(a) // undefined
}

foo() // undefined
```

`null` 表示一个变量没有值。

```js
let foo = null
console.log(foo) // null
```

如果您熟悉原型链，您可能知道原型链的最终值为 `null`：

```js
Object.getPrototypeOf(Object.prototype) // null
```

**`undefined` 和 `null` 被认为是不同的类型。**

`undefined` 是类型本身，而 `null` 是一个对象的特殊值。

```js
console.log(typeof undefined) // 'undefined'
console.log(typeof null) // 'object'
```

由于它们是不同的类型，下面是相等和严格相等运算符在相互比较时的结果：

```js
null == undefined // true
null === undefined // false
```

您可能不知道的是，在使用 `JSON.stringify` 方法时，`JSON.stringify` 将自动省略值为 `undefined` 的数据，但保留了 `null`：

```js
JSON.stringify({
  name: 'O.O',
  address: null,
  age: undefined
})

// '{"name":"O.O","address":null}'
```

## 更多资料

[undefined 与 null 的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
