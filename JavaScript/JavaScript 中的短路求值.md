# JavaScript 中的短路求值

通常，我们习惯于在 JavaScript 中使用 `if-else` 语句来执行条件变量赋值和处理控制流。下面，我们来看看使用**短路求值**来完成相同的事情。

## 什么是短路求值？

**短路求值（Short-Circuit Evaluation）**的语法已经有些熟悉了；它使用通常在 `if-else` 语句中看到的 `||` 或 `&&` 逻辑运算符，在 `if-else` 之外使用，**`||` 运算符可用于返回表达式中的第一个真值，而 `&&` 运算符可用于阻止执行后续代码。**

## 使用 || 运算符

下面是一个 `if-else` 语句转换为短路求值的示例。在本例中，我们希望获得用户的显示名。如果 `user` 对象有 `name` 属性，我们将使用它作为显示名。否则，我们只会称我们的用户为 `Guest`。

```js
const user = {}

let displayName

if (user.name) {
  displayName = user.name
} else {
  displayName = 'Guest'
}
console.log(displayName) // "Guest"
```

使用短路求值可以大大简化此过程：

```js
const user = {}
const displayName = user.name || 'Guest'
console.log(displayName) // "Guest"
```

因为 `user.name` 是 `undefined`，所以它是错误的。因此，短路求值返回 `Guest`！相反，如果 `user.name` 存在并且不是虚假的，则返回：

```js
const user = { name: 'D.O' }
const displayName = user.name || 'Guest'
console.log(displayName) // "D.O"
```

真正整洁的是，我们可以将这些条件联系起来。假设我们也可能有一个用户的昵称。我们的 `if-else` 条件可能会变得很难看：

```js
const user = { name: 'IU' }
let displayName

if (user.nickname) {
  displayName = user.nickname
} else if (user.name) {
  displayName = user.name
} else {
  displayName = 'Guest'
}

console.log(displayName) // "IU"
```

使用短路求值，这一切变得更加简单：

```js
const user = { name: 'IU' }
const displayName = user.nickname || user.name || 'Guest'
console.log(displayName) // "IU"
```

## 使用 && 运算符

`&&` 短路的工作原理有点不同。使用 `&&` 运算符，我们可以确保只有在前面的表达式是真实的情况下才运行后续表达式。这到底是什么意思？让我们看另一个例子！在本例中，如果服务器响应不成功，我们希望在控制台中显示一个错误。首先，我们可以使用 `if` 语句来实现这一点。

```js
const response = { success: false }
if (!response.success) {
  console.error('ERROR')
}
```

使用短路求值，可以完成以下任务：

```js
const response = { success: false }
!response.success && console.error('ERROR')
```

如果 `response.success` 为 `true`，那么 `!response.success` 则为 `false`。因此，整个表达式的计算结果为 `false`，而解释器无需查看 `&&` 右侧的任何内容。

## 潜在问题：合法的虚假值

假设你的应用程序可能知道也可能不知道一个用户有多少个孩子。您可以考虑使用以下短路评估：

```js
const numberOfChildrenDisplay = user.numberOfChildren || 'Unknown'
```

这里的问题在于，`0` 是已知子对象数的合法值，但 `0` 却是错误的！因此，以下行为将是不正确的：

```js
const user = { numberOfChildren: 0 }
const numberOfChildrenDisplay = user.numberOfChildren || 'Unknown'
console.log(numberOfChildrenDisplay) // "Unknown"
```

## 最后

对于变量赋值和控制流来说，短路求值可以是一个很好的简写形式，只要确保在使用假值时考虑它们的有效性即可！
