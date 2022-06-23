# JavaScript 中的虚值是什么？

> 简单的来说虚值就是在转换为布尔值时变为 `false` 的值。

## 如何检查值是否虚值，或者说如何将值转换为布尔值？

使用 `Boolean` 方法或者 `!!` 运算符，将 [truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 或 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy) 值转换为布尔值。

- Falsy：`false`、`null`、`undefined`、`NaN`、`0 +0 -0`、空字符串（""、''、``）
- truthy：其他都为 `true`

```js
!!false // false
!!undefined // false
!!null // false
!!NaN // false
!!0 // false
!!'' // false

!!'hello' // true
!!1 // true
!!{} // true
!![] // true

// or

Boolean(false) // false
Boolean(undefined) // false
Boolean(null) // false
Boolean(NaN) // false
Boolean(0) // false
Boolean('') // false
```
