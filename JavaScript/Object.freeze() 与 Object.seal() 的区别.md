# Object.freeze() 与 Object.seal() 的区别

`Object.freeze()` 与 `Object.seal()` 两者都用于防止 JavaScript 对象被修改。

```js
const freeze = Object.freeze({ name: 'O.O' })
const seal = Object.seal({ name: 'O.O' })

freeze.name = 'D.O' // freeze { name: 'O.O' }
seal.name = 'D.O' // seal { name: 'D.O' } -> 注意：这里已经被修改了

delete freeze.name // freeze { name: 'O.O' }
delete seal.name // freeze { name: 'O.O' }

freeze.age = 18 // freeze { name: 'O.O' }
seal.age = 18 // seal { name: 'O.O' }

const nums = Object.freeze([1, 2, 3])
nums // [1, 2, 3]
nums.push(4) // TypeError
```

如果要防止添加新属性和删除现有属性，那么这两种方法都可以满足您的需要。但是，如果要防止现有属性被更改，则必须使用 [`Object.freeze()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)。原因就是 [`Object.seal()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal) 只将现有 `configurable` 属性标记为不可配置，这意味着只要它们是可写的，就可以更改它们的值。

|                   | 添加 | 读取 | 更改 | 删除 |
| :---------------: | :--: | :--: | :--: | :--: |
| `Object.freeze()` |  ❌  |  ✅  |  ❌  |  ❌  |
|  `Object.seal()`  |  ❌  |  ✅  |  ✅  |  ❌  |

> **注意**：这两种方法都对对象执行浅**冻结/密封**。这意味着嵌套对象和数组不会被冻结或密封，可以进行修改。为了防止这种情况发生，您可以像本文所述的那样对对象进行深度冻结。
