# Object.getOwnPropertyNames() 与 Object.keys() 的区别

`Object.getOwnPropertyNames(obj)` 返回对象 `obj` 的所有属性（包括可枚举和不可枚举）。`Object.keys(obj)` 返回所有可枚举的属性。

`Object.defineProperty` 中的 `enumerable` 选项可定义属性是否可枚举。

在下面的例子中，`screen` 对象有两个属性：`width` 和 `height`。

```js
const screen = {
  width: 1200,
  height: 800
}

Object.getOwnPropertyNames(screen) // ['width', 'height']
Object.keys(screen) // ['width', 'height']
```

让我们再定义一个名为 `resolution` 的属性，但它被设置为 `enumerable: false`:

```js
Object.defineProperties(screen, {
  resolution: {
    enumerable: false,
    value: '2560 x 1440'
  }
})
```

那么，`resolution` 属性就不会出现在 `Object.keys` 的列表中：

```js
Object.getOwnPropertyNames(screen) // ['width', 'height', 'resolution']
Object.keys(screen) // ['width', 'height']
```

对于数组，`Object.getOwnPropertyNames` 包含一个名为 `length` 的额外属性，它是数组的大小。

```js
const animals = ['dog', 'cat', 'tiger']

Object.keys(animals) // ['0', '1', '2']
Object.getOwnPropertyNames(animals) // ['0', '1', '2', 'length']
```
