# string charAt(i) 和 string[i] 的区别

有两种方法可以访问字符串的单个字符：

- 使用 `charAt[index]` 方法
- 使用括号表示法，如 `'hello'[1]`

在这两种情况下，`'hello'[1]` 和 `'hello'.charAt(1)` 都将返回第二个字符 `e`。

## 区别

- 第二种方式是 ECMA5 的标准，在现代浏览器中受支持。在非常旧的浏览器（如 IE6、7）中不支持它。（我不认为我们仍然需要支持这些版本的 IE）。

- 以下表格列出了两种方法的预期结果：

| 方法                   | 索引的范围为 `0` 和 `string.length - 1` | 其他情况下  |
| ---------------------- | --------------------------------------- | ----------- |
| `string.charAt(index)` | 第一个和最后一个                        | `''`        |
| `string[index]`        | 第一个和最后一个                        | `undefined` |

如果不传递适当的索引（不是整数或超出范围），在某些边缘情况下，我们将得到不同的结果。

```js
'hello'[NaN] // undefined
'hello'.charAt(NaN) // 'h'

'hello'[undefined] // undefined
'hello'.charAt(undefined) // 'h'

'hello'[true] // undefined
'hello'.charAt(true) // 'e'

'hello'['00'] // undefined

// 返回 h，因为它将首先尝试将 00 转换为数字
'hello'.charAt('00')

'hello'[1.5] // undefined
// 返回 e，因为它将 1.23 四舍五入到数字 1
'hello'.charAt(1.23)
```

如果索引超出可选范围：

```js
'hello'[100] // undefined
'hello'.charAt(100) // ''
```

## 为什么 `'hello'.charAt(true)` 返回 `e`？

`charAt(index)` 方法首先尝试将索引转换为数字。由于 `Number(true) == 1`，`charAt(true)` 将返回一个索引位置的字符，即第二个字符。
