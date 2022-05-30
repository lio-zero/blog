# Object.is() 和 === 的区别

`Object.is()` 的行为与 `===`（严格相等操作符）相同，除了 `NaN`、`+0` 和 `-0`。

```js
+0 === -0 // true
Object.is(+0, -0) // false

NaN === NaN // false
Object.is(NaN, NaN) // true

Number.NaN === Number.NaN // false
Object.is(Number.NaN, Number.NaN) // true

NaN === Number.NaN // false
Object.is(NaN, Number.NaN) // true
```
