# 实现 Object.assign

> **`Object.assign()`** 方法将所有可枚举（`Object.propertyIsEnumerable()` 返回 `true`）和自有（`Object.hasOwnProperty()` 返回 `true`）属性从一个或多个源对象复制到目标对象，返回修改后的对象。

- 目标对象不能是 `null` 或 `undefined`，否则将抛出错误
- 源对象可以有多个，使用剩余运算符收集，通过循环判断每个对象的是否为自身属性，添加到目标对象。如果有重名属性，靠前的属性将被覆盖
- `Object.assign` 返回对象等于目标对象

```js
Object.myAssign = function (target, ...source) {
  if (target === null || target === undefined) {
    throw new TypeError('Cannot convert undefined or null to object')
  }

  target = Object(target)

  for (let obj of source) {
    if (obj != null) {
      for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
          target[key] = obj[key]
        }
      }
    }
  }

  return target
}
```

示例：

```js
const user = { name: 'O.O' }

// 可枚举、自身数据
user.propertyIsEnumerable('name') // true
user.hasOwnProperty('name') // true

const result = Object.assign(user, { age: 20, hobby: ['eat', 'sleep'] })

console.log(result) // {name: 'O.O', age: 20, hobby: ['eat', 'sleep']}
console.log(user === result) // true
```
