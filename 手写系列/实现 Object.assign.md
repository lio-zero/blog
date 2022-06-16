# 实现 Object.assign

> **`Object.assign()`** 方法将所有可枚举（`Object.propertyIsEnumerable()` 返回 `true`）和自有（`Object.hasOwnProperty()` 返回 `true`）属性从一个或多个源对象复制到目标对象，返回修改后的对象。

- 目标对象不能是 `null` 或 `undefined`，否则将抛出错误
- 源对象可有多个，使用剩余运算符收集，通过循环判断每个对象的是否为自身属性，添加到目标对象。

```js
Object.myAssign = function (target, ...source) {
  if (target == null) {
    throw new TypeError('Cannot convert undefined or null to object')
  }
  let ret = Object(target)
  source.forEach((obj) => {
    if (obj != null) {
      for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
          ret[key] = obj[key]
        }
      }
    }
  })
  return ret
}
```
