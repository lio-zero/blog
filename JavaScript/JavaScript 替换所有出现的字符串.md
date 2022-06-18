# JavaScript 替换所有出现的字符串

假设你有这样一句话 `I am superman.Are you superman?`，你要替换其中的 `Superman`，你该怎么做？

这里总结三种方法，用于替换上面句子中出现的所有 `Superman`。

## String.prototype.replace()

使用 `String.prototype.replace()` 方法在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

```js
let sentence = 'I am superman.Are you superman?'

const new_sentence = (str, search, replacement) =>
  str.replace(new RegExp(search, 'g'), replacement)
new_sentence(sentence, 'Superman', 'superwoman') // "I am superman.Are you superman?"
```

使用 `new RegExp` 是为了便于使用参数。使用 `g` 将**区分大小写**。如果**不想区分大小写**的替换，可以使用 `i`。

## String.prototype.replaceAll()

> [`String.prototype.replaceAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll) 方法返回一个新字符串，新字符串所有满足 `pattern` 的部分都已被 `replacement` 替换。`pattern` 可以是一个字符串或一个 `RegExp`， `replacement` 可以是一个字符串或一个在> 每次匹配被调用的函数。

```js
const new_sentence = (str, search, replacement) =>
  str.replaceAll(search, replacement)
new_sentence(sentence, 'Superman', 'superwoman') // "I am superman.Are you superman?"
```

## split 和 join

最快速的方法是使用 `String.prototype.split()` 拆分字符串，在使用 `Array.prototype.join()` 连接。

```js
const new_sentence = (str, search, replacement) =>
  str.split(search).join(replacement)
new_sentence(sentence, 'Superman', 'superwoman') // "I am superman.Are you superman?"
```

**注意**：在不考虑性能的情况下，使用 `split` 和 `join` 比正则和 `replaceAll` 更容易理解，简便。但还是不建议使用它，最好使用正则或者 `replaceAll`。
