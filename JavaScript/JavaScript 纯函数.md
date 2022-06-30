# JavaScript 纯函数

你可能听过一个术语叫**纯函数（Pure Function）**，它是一个非常重要的概念，我们下面将来介绍它。

## 两项标准

纯函数必须满足两个条件：

- 对于相同输入具有相同的输出
- 无副作用

## 相同输入的相同输入

我们首先考虑一个函数，它对于相同的输入没有相同的输出：

```js
let str = 'Hello'

const f = (name) => `${str} ${name}`

console.log(f('IU')) // Hello IU
```

我们可以看到，`f` 函数的输出可以更改，即使该函数的输入 `name` 保持不变（我们所要做的就是更改 `str` 变量的值）。

对于相同的输入，我们如何使输出相同？这就像确保 `f` 也是函数的参数一样简单：

```js
const f = (str, name) => `${str} ${name}`

console.log(f('KAI', 'LAY')) // KAI LAY
```

现在，我们无法使 `f` 函数返回不同的结果，除非我们更改其中一个输入参数。

## 无副作用

副作用是当函数改变了函数作用域之外的内容时。还是一样，我们先来看看**有副作用**规则的函数示例：

```js
const user = {
  password: '123456'
}

let isValid = false

const validate = (user) => {
  if (user.password.length > 4) {
    isValid = true
  }
}
```

我们可以看到，我们正在明显地更改 `isValid` 变量，该变量不在 `validate` 函数的作用域之内。

**注意**，如果你的函数没有返回任何内容，则表明它可能有副作用！

```js
function hello() {
  console.log('Hello World!')
}
```

那么，如何消除这种副作用呢？我们可以从函数本身返回用户是否有效，而不是更改外部 `isValid` 变量：

```js
const user = {
  password: '123456'
}

const validate = (user) => user.password.length > 4

const isValid = validate(user)
```

就是这样，我们的 `validate` 函数不再改变其作用域之外的任何内容。

还有，DOM 操作，任何回调或读/写文件，也存在副作用：

```js
function calculate(a, b) {
  document.write('Hello world!')
  return a * b
}

console.log(calculate(2, 5))
```

额外的，你可以对外部状态进行克隆，而不会使它为非纯函数。

```js
let name = ['O.O', 'K.O']

function fullName(newName, name) {
  let clonedName = [...name]
  clonedName[clonedName.length] = newName
  return clonedName
}

console.log(fullName('O.K', name)) // ['O.O', 'K.O', 'O.K']
```

示例中，我们克隆了 `name` 并更新它，这并没有依赖 `name` 变量。因此，它是一个纯函数。

## 为什么纯函数是好的？

您可能有一些直觉，这些纯函数概念是好的，尤其是如果您亲身经历了它们为什么不好的经验。封装逻辑的能力越强，测试就越容易，而无需担心更改会影响什么，更改就越容易。

让我们考虑一下测试我们的 `f` 函数。在最初的形式中，我们可以做出以下断言：

```js
describe('f', function () {
  it('action', function () {
    expect(f('IU')).toEqual('Hello IU')
  })
})
```

但是，如果我们将 `str` 变量更改为 `arr`，测试将失败！这不应该发生；输出函数工作正常，但它失败了，因为它依赖于超出其作用域的变量。当我们将 `str` 作为 `f` 函数的参数时，这个问题就消失了，因为使用该函数所需的所有信息都必须作为测试中的参数提供。

现在让我们考虑一下我们的用户验证示例。我们怎么去测试它呢？如果函数的外部变量发生了更改，则可能很难断言该变量。此外，如果我们将 `isValid` 变量的默认值更改为 `true`，则函数将失败。外部变量发生变化绝对是不对的!
