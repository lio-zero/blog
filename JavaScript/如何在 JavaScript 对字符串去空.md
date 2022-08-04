# 如何在 JavaScript 对字符串去空

在 JavaScript 中 `trim` 用于删除空白。JavaScript 提供了我们几种 `trim()` 方法来去除这些空格。

从字符串中删除空白非常简单。要仅删除前导空格，可以使用 `trimStart()`。要删除尾随空格，可以使用 `trimEnd()`。或者用 `trim()` 将其全部删除

```js
const str = '   hello   '

string.trimStart() // "hello   "
string.trimEnd() // "   hello"
string.trim() // "hello"
```

所有 `trim()` 方法都将返回一个新字符串。这意味着原始字符串将保持原样。

```js
const string = '   hello   '

string.trimStart() // 'hello  '
string.trimEnd() // '   hello'
string.trim() // 'hello'

string // '   hello   '
```

其可以删除以下创建的空白：

- 空格
- 标签
- 无中断空格
- 行终止符字符

我们来看几个例子。

## 行终止符字符

> 除了空白符之外，行终止符也可以提高源码的可读性。不同的是，行终止符可以影响 JavaScript 代码的执行。行终止符也会影响自动分号补全的执行。在正则表达式中，行终止符会被 `\s` 匹配。

> 在 ECMAScript 中，只有下列 Unicode 字符会被当成行终止符，其他的行终止符（比如 Next Line、NEL、U+0085 等）都会被当成空白符。

| 编码   | 名称     | 缩写   | 说明                                              | 转义序列 |
| ------ | -------- | ------ | ------------------------------------------------- | -------- |
| U+000A | 换行符   | `<LF>` | 在 UNIX 系统中起新行                              | `\n`     |
| U+000D | 回车符   | `<CR>` | 在 Commodore 和早期的 Mac 系统中起新行            | `\r`     |
| U+2028 | 行分隔符 | `<LS>` | [Wikipedia](http://en.wikipedia.org/wiki/Newline) |          |
| U+2029 | 段分隔符 | `<PS>` | [Wikipedia](http://en.wikipedia.org/wiki/Newline) |          |

以上定义来自 MDN [Line terminators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#line_terminators)。我们来看看例子。

```js
'hello \n'.trim() // 'hello'
'hello \t'.trim() // 'hello'
'hello \r'.trim() // 'hello'
```

## 多行字符串

在 JavaScript 中，我们可以使用**模板字符串**轻松创建多行字符串。`trim` 方法同样可以去除空白部分。

```js
const multiLine = `
hello

`

multiline.trim() // 'hello'
```

## 多个单词

与多行相同，它只 `trim` 第一个单词的开头和最后一个单词的结尾。

```js
'  Hello IO  '.trim() // 'Hello IO'
```

## 多行多字

与多行相同，它只修剪第一个单词的开头和最后一个单词的结尾。

```js
const multiLine = `
Hello

IO

`
console.log(multiLine.trim()) // "Hello\n\nIO"
// 也就是 👇
/*
"Hello

IO"
*/
```

## trimStart 与 trimLeft

`trimStart` 用于删除前导空白。它还有一个别名 `trimLeft()`，两者做着相同的事情。

```js
const string = '   Hello   '

string.trimStart() // 'Hello   '
string.trimLeft() // 'Hello   '
```

## trimEnd 与 trimRight

`trimEnd` 删除尾部的空白。此方法的别名为 `trimRight()`。同样，两者做着相同的事情。

```js
const string = '   Hello   '

string.trimEnd() // '   Hello'
string.trimRight() // '   Hello'
```

### 选择

我应该该选择哪一个？[ECMAScript 规范](https://ecma-international.org/ecma-262/10.0/index.html#sec-string.prototype.trimstart)给出了很好的解答：

> 首选属性 `trimStart` 和 `trimEnd`。`trimLeft` 和 `trimRight` 属性主要是为了与旧代码兼容而提供的。

## 为什么会有别名？

因为 `trimLeft` 和 `trimRight` 首先被引入。然而，委员会决定提议改为 `trimStart` 和 `trimEnd`。这是因为它与其他内置方法 `padStart` 和 `padEnd` 更为一致。这至少对我来说很有意义的，我认为一致性是关键，使用相同的语言有助于减少混乱。

但出于 Web 兼容性的考虑，他们保留了旧的术语（`trimLeft` 和 `trimRight`）作为别名。因此，如果您的代码使用的是旧方法，没问题，它们仍然可以工作。但是，如果您有这个能力，我建议您将其更改为使用正式的方法，而不是别名，这样您的代码库中就不会有两种不同的方法。记住，这一切都是为了一致性。

## 其他解决方案

```js
const str = '  hi   '
str.replace(/ /g, '') // 'hi'
```

**注意**：此解决方案将删除字符串中的所有空白。

```js
const str = '  h i   '
str.replace(/ /g, '') // 'hi'
```

我们可以编写更加精确的正则，只匹配开头和结尾：

```js
/**
 * @desc 删除字符串两边的空格
 */
function remove_both_trim(str) {
  return str.replace(/^\s+|\s+$/g, '')
}

const str = '      Superman      '
removeBothTrim(str) // 'Superman'
```

## 浏览器支持

以下是它们的支持情况：

![trim](https://upload-images.jianshu.io/upload_images/18281896-8ed8ac1e0842972e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![trimStart](https://upload-images.jianshu.io/upload_images/18281896-05bbc887ed0742fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![trimEnd](https://upload-images.jianshu.io/upload_images/18281896-9101c65c54ac44e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
