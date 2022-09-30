# JavaScript RegExp 对象 — 正则表达式

> [**正则表达式（Regular Expression）**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)是一组由字母和符号组成的特殊文本，它可以用来从文本中找出满足你想要的格式的句子。

![regexp](https://upload-images.jianshu.io/upload_images/18281896-075994f1598bbddb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建正则表达式的两种方法：字面量、`RegExp` 构造函数

**字面量**，以斜杠表示开始和结束：

```js
const regex = /abc/
```

`RegExp` 构造函数：

```js
const regex = new RegExp('abc')
```

`RegExp` 构造函数还可以接受第二个参数，表示修饰符：

```js
const regex = new RegExp('abc', 'i')
```

## 实例方法

### test()

`test()` 方法返回一个布尔值，表示当前模式是否能匹配参数字符串。

```js
;/lit/.test('I am a lit') // true
```

### exec()

`exec()` 方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 `null`。

```js
const str = '_x_x'

/x/.exec(str) // ['x', index: 1, input: '_x_x', groups: undefined]
/y/.exec(str) // null
```

## 实例属性

- `ignoreCase` 只读属性。返回一个布尔值，表示是否设置了 `i` 修饰符。
- `multiline` 只读属性。返回一个布尔值，表示是否设置了 `m` 修饰符。
- `global` 只读属性。返回一个布尔值，表示是否设置了 `g` 修饰符。

```js
const reg = /abc/gim

reg.ignoreCase // true
reg.global // true
reg.multiline // true
```

> **Tips**：这些修饰符的作用下文有做解释。

`lastIndex` 可读可写。返回一个数值，表示下一次开始搜索的位置。

```js
;/(hi)?/g.lastIndex // 0
```

`source` 只读属性。返回正则表达式的字符串形式（不包括反斜杠）。

```js
;/abc/gim.source // "abc"
```

`unicode` 只读属性。属性表明正则表达式带有 `u` 修饰符。

```js
;/\u{61}/u.unicode // true
```

`sticky` ES6 新增的只读属性。表示是否设置了`y`修饰符。

```js
;/foo/y.sticky // true
```

`flags` ES6 新增。该属性返回一个字符串，由当前正则表达式对象的修饰符组成，以字典序排序（从左到右，即 `"gimuy"`）。

```js
;/foo/gi.flags / // "gi"
  bar /
  myu.flags.flags // "muy"
```

## 字符串的正则方法

### match()

`match()` 方法检索返回一个字符串匹配正则表达式的结果数组。

```js
'The fat cat sat on the mat.'.match(/a/g) // ["a", "a", "a", "a"]
```

### split()

`split()` 按照给定规则进行字符串分割，返回一个数组，包含分割后的各个成员。

语法：

```js
str.split(separator, [limit])
```

该方法接受两个参数，第一个参数是正则表达式，表示分隔规则，第二个参数是返回数组的最大成员数。

示例：

```js
'The fat cat sat on the mat.'.split(' ', 3) // ["The", "fat", "cat"]

'Hello 1 word. Sentence number 2.'.split(/(\d)/) // ["Hello ", "1", " word. Sentence number ", "2", "."]
```

### replace()

`replace()` 按照给定的正则表达式进行替换，返回替换后的字符串。

语法：

```js
str.replace(regexp|substr, newSubStr|function)
```

示例：

```js
const str = 'Twas the night before Xmas...'
const newstr = str.replace(/xmas/i, 'Christmas')

str // "Twas the night before Xmas..."
newstr // "Twas the night before Christmas..."
```

原字符串不会改变。

### search()

`search()` 按照给定的正则表达式进行搜索，返回一个整数，表示匹配开始的位置。如果没有任何匹配，则返回 `-1`。

```js
'The fat cat sat on the mat.'.search('at') // 5

'The fat cat sat on the mat.'.search('v') // -1
```

### matchAll()

`matchAll()` ES2020 新增方法。该方法返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器（Iterator）。

```js
const str = 'test1test2'
const reg = /t(e)(st(\d?))/g

for (const match of str.matchAll(reg)) {
  console.log(match)
}
// ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', groups: undefined]
// ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', groups: undefined]

// 转为数组
const array = [...str.matchAll(reg)]
// const array = Array.from(str.matchAll(reg))

array[0] // ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', groups: undefined]
array[1] // ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', groups: undefined]
```

## 元字符

| 元字符   | 描述                                                          |
| -------- | ------------------------------------------------------------- |
| `.`      | 匹配任意单个任何字符，除了换行符                              |
| `[ ]`    | 字符种类。匹配方括号内的任意字符。                            |
| `[^]`    | 否定的字符种类。匹配除了方括号里的任意字符。                  |
| `*`      | 匹配 `>=0` 个重复的在 `*` 号之前的字符。                      |
| `+`      | 匹配 `>=1` 个重复的 `+` 号前的字符。                          |
| `?`      | 标记 `?` 之前的字符为可选。                                   |
| `{n, m}` | 匹配 `num` 个大括号之前的字符或字符集 (n <= num <= m)。       |
| `(xyz)`  | 字符集，匹配与 `xyz` 完全相等的字符串。                       |
| `\`      | 转义字符，用于匹配一些保留的字符 `[ ] ( ) { } . \* + ? ^ $ \  |
| `^`      | 从开始行开始匹配。                                            |
| `$`      | 从末端开始匹配。                                              |

![管道符](https://upload-images.jianshu.io/upload_images/18281896-e1ea39893d001ced.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用或运算符（`|`）在用 Markdown 表格上显示有问题，贴出图片。（倒数第三个元字符描述缺少 `|` 同样问题）

### 分组

| `(abc)`   | 捕获组               |
| --------- | -------------------- |
| `(?:abc)` | 匹配，但不捕获 `abc` |

![(a|b)](https://upload-images.jianshu.io/upload_images/18281896-d14694efdd1fc9cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 量词

| `a*`     | 匹配 0 或更多     |
| -------- | ----------------- |
| `a+`     | 匹配 1 或更多     |
| `a?`     | 匹配 0 或 1       |
| `a{5}`   | 精确匹配 5        |
| `a{,3}`  | 最多匹配 3 个     |
| `a{3,}`  | 匹配 3 或更多     |
| `a{1,3}` | 1 和 3 之间的匹配 |

### 锚点

| `\G`      | 匹配开始       |
| --------- | -------------- |
| `^`、`\A` | 字符串开头     |
| `$`、`\Z` | 字符串结尾     |
| `\z`      | 字符串绝对结尾 |
| `\b`      | 单词边界       |
| `\B`      | 非单词边界     |
| `^abc`    | 以 `abc` 结尾  |
| `abc$`    | 以 `abc` 开头  |

### 转义字符

上面所提到的有特殊含义的元字符，如果要匹配它们本身，就需要在它们前面要加上反斜杠。比如要匹配 `+`，就要写成 `\+`。

| `\. \* \\` | 正则表达式使用的转义特殊字符 |
| ---------- | ---------------------------- |
| `\t`       | Tab 制表键                   |
| `\n`       | 换行符                       |
| `\r`       | 回车                         |

```js
;/1+1/
  .test('1+1')(
    // false

    /1\+1/
  )
  .test('1+1') // true
```

如果使用 `RegExp` 方法生成正则对象，转义需要使用两个斜杠，因为字符串内部会先转义一次。

```js
const reg = new RegExp('1+1')
reg.test('1+1') // false

const reg1 = new RegExp('1\\+1')
reg1.test('1+1') // true
```

### 字符集

字符集也叫做字符类。方括号 `[]` 用来指定一个字符集。 在方括号中使用连字符来指定字符集的范围。 在方括号中的字符集不关心顺序。

| `[abc]`       | `a`、`b` 或 `c` 中的任何一种        |
| ------------- | ----------------------------------- |
| `[a-z]`       | 介于和 `a` 和 `z` 之间的字符        |
| `[1-9]`       | 介于和 `1` 和 `9` 之间的数字        |
| `[[:print:]]` | 包括空格在内的任何可打印字符        |
| `[^abc]`      | 除了 `a`、`b` 或 `c` 以外的任何字符 |

## 简写字符集

| `.`  | 除换行符外的所有字符                                                     |
| ---- | ------------------------------------------------------------------------ |
| `\w` | 文字（字母、数字、下划线）。相当于 `[A-Za-z0-9_]`                        |
| `\d` | 数字。相当于 `[0-9]`                                                     |
| `\s` | 空白（任何空白字符，包括空格、制表符、换页符等）。相当于 `[ \f\n\r\t\v]` |
| `\W` | 非文字                                                                   |
| `\D` | 非数字                                                                   |
| `\S` | 非空白                                                                   |
| `\f` | 匹配一个换页符                                                           |
| `\n` | 匹配一个换行符                                                           |
| `\r` | 匹配一个回车符                                                           |
| `\t` | 匹配一个制表符                                                           |
| `\v` | 匹配一个垂直制表符                                                       |
| `\p` | 匹配 CR/LF（等同于 `\r\n`），用来匹配 DOS 行终止符                       |

## 标志

标志也叫模式修饰符，用来修改表达式的搜索结果，它们可以任意组合。

| 修饰符 | 描述                                                                                             |
| :----- | :----------------------------------------------------------------------------------------------- |
| `i`    | 忽略大小写。                                                                                     |
| `g`    | 全局匹配（查找所有匹配而非在找到第一个匹配后停止）。                                             |
| `m`    | 多行匹配：锚点元字符 `^` `$` 工作范围在每行的起始。                                              |
| `u`    | Unicode 模式                                                                                     |
| `y`    | 粘连（sticky），与 `g` 修饰符类似，也是全局匹配。`y` 修饰符确保匹配必须从剩余的第一个位置开始    |
| `s`    | ES2018 引入 `s` 修饰符，使得 `.` 可以匹配任意单个字符。 `dotAll` 模式。即点（dot）代表一切字符。 |

### 忽略大小写 (Case Insensitive)

修饰语 `i` 用于忽略大小写。

```js
'The fat cat sat on the mat.'.match(/The/gi) // ['The', 'the']
```

### 全局搜索 (Global search)

修饰符 `g` 常用于执行一个全局搜索匹配，即（不仅仅返回第一个匹配的，而是返回全部）。

```js
'The fat cat sat on the mat.'.match(/.(at)/g) // ['fat', 'cat', 'sat', 'mat']
```

### 多行修饰符 (Multiline)

多行修饰符 `m` 常用于执行一个多行匹配。

像之前介绍的 `(^,$)` 用于检查格式是否是在待检测字符串的开头或结尾。但我们如果想要它在每行的开头和结尾生效，我们需要用到多行修饰符 `m`。

```js
'The fat cat sat on the mat.'.match(/.at(.)?$/gm) // ['mat.']
```

## 零宽度断言（前后预查）

先行断言和后发断言都属于**非捕获簇**（不捕获文本 ，也不针对组合计进行计数）。 先行断言用于判断所匹配的格式是否在另一个确定的格式之前，匹配结果不包含该确定格式（仅作为约束）。

例如，我们想要获得所有跟在 `$` 符号后的数字，我们可以使用正后发断言 `(?<=\$)[0-9\.]*`。 这个表达式匹配 `$` 开头，之后跟着 `0,1,2,3,4,5,6,7,8,9,.` 这些字符可以出现大于等于 0 次。

```js
'$'.match(/(?<=\$)[0-9\.]*/) // ['', index: 1, input: '$', groups: undefined]
'$1'.match(/(?<=\$)[0-9\.]*/) // ['1', index: 1, input: '$1', groups: undefined]
```

| 符号  | 描述            |
| ----- | --------------- |
| `?=`  | 正先行断言-存在 |
| `?!`  | 负先行断言-排除 |
| `?<=` | 正后发断言-存在 |
| `?<!` | 负后发断言-排除 |

### `?=` 正先行断言

`?=` 正先行断言，表示第一部分表达式之后必须跟着 `?=...`定义的表达式。

返回结果只包含满足匹配条件的第一部分表达式。 定义一个正先行断言要使用 `()`。在括号内部使用一个问号和等号： `(?=...)`。

正先行断言的内容写在括号中的等号后面。

```js
'The fat cat sat on the mat.'.match(/(T|t)he(?=\sfat)/) // ['The', 'T', index: 0, input: 'The fat cat sat on the mat.', groups: undefined]
'The fat cat sat on the mat.'.match(/(T|t)he(?=\sfat)/) // ['The', 'T', index: 0, input: 'The fat cat sat on the mat.', groups: undefined]
```

### `?!` 负先行断言

`?!` 负先行断言，用于筛选所有匹配结果，筛选条件为 其后不跟随着断言中定义的格式。 正先行断言定义和负先行断言一样，区别就是 `=` 替换成 `!` 也就是 `(?!...)`。

```js
;/(T|t)he(?!\sfat)/.exec('The fat cat sat on the mat.') // ['the', 't', index: 19, input: 'The fat cat sat on the mat.', groups: undefined]
```

### `?<=` 正后发断言

`?<=` 正后发断言，用于筛选所有匹配结果，筛选条件为 其前跟随着断言中定义的格式。

```js
;/(?<=(T|t)he\s)(fat|mat)/.exec('The fat cat sat on the mat.') // ['fat', 'T', 'fat', index: 4, input: 'The fat cat sat on the mat.', groups: undefined]
```

### `?<!` 负后发断言

`?<!` 负后发断言，用于筛选所有匹配结果，筛选条件为 其前不跟随着断言中定义的格式。

```js
;/(?<!(T|t)he\s)(cat)/.exec('The fat cat sat on the mat.') // ['cat', undefined, 'cat', index: 8, input: 'The fat cat sat on the mat.', groups: undefined]
```

## 贪婪匹配与惰性匹配（Greedy vs lazy matching）

正则表达式默认采用贪婪匹配模式，在该模式下意味着会匹配尽可能长的子串。我们可以使用 `?` 将贪婪匹配模式转化为惰性匹配模式。

- `*?`：表示某个模式出现 0 次或多次，匹配时采用非贪婪模式。
- `+?`：表示某个模式出现 1 次或多次，匹配时采用非贪婪模式。

```js
'The fat cat sat on the mat.'.match(/(.*at)/) // ['The fat cat sat on the mat', 'The fat cat sat on the mat', index: 0, input: 'The fat cat sat on the mat.', groups: undefined]

'The fat cat sat on the mat.'.match(/(.*?at)/) // ['The fat', 'The fat', index: 0, input: 'The fat cat sat on the mat.', groups: undefined]
```

## 更多资料

- [xregexp](https://github.com/slevithan/xregexp) 扩展的 JavaScript 正则表达式
- [learn-regex](https://github.com/ziishaned/learn-regex)
- [regexr](https://regexr.com/) RegExr 是基于 HTML/JS 的工具，用于创建，测试和学习正则表达式。
- [RegExper](https://regexper.com/) 可以将正则表达式转成解释图片。
- [RegExLib](https://regexlib.com/) 收集各种常用的正则表达式，比如搜索 "email"，会返回该站点收集的所有关于 📪 正则表达式。
- [RegexBuddy](https://www.regexbuddy.com/)
- [RegexMagic](https://www.regexmagic.com/)
- [Rubular](https://rubular.com/)
- [OWASP Validation Regex Repository](https://owasp.org/www-community/OWASP_Validation_Regex_Repository)
- [CyrilEx Regex Tester](https://extendsclass.com/regex-tester.html)
- [一份 PDF 备忘单（英文）](https://appletree.or.kr/quick_reference_cards/Regular_Expressions/regex-cheatsheet.pdf)
- [The Complete Guide to Regular Expressions (Regex)](https://dev.to/coderpad/the-complete-guide-to-regular-expressions-regex-1m6)
- [Regex Cheat Sheet](https://dev.to/emmabostian/regex-cheat-sheet-2j2a)
- [iHateRegex](https://ihateregex.io/)
- [JS 正则表达式完整教程（略长）](https://juejin.cn/post/6844903487155732494)
- [正则表达式不要背](https://juejin.cn/post/6844903845227659271)
