# 使用 Proxy 对象来健壮您的 JavaScript 不变性函数

虽然 JavaScript 允许我们突变（Mutation）对象，但我们可能会选择不允许自己（和其他程序员）这样做。

最好的例子之一就是在 React 应用程序中设置 `State`。如果我们改变当前 `State` 而不是当前状态的新副本，我们可能会遇到难以诊断的问题。

下面，我们将使用不可变代理函数来防止对象突变！

## 什么是对象突变？

在几天前，我写过一篇关于[变量赋值与原始/对象可变性](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%8F%98%E9%87%8F%E8%B5%8B%E5%80%BC%E4%B8%8E%E5%8E%9F%E5%A7%8B%E5%AF%B9%E8%B1%A1%E5%8F%AF%E5%8F%98%E6%80%A7.md)，如果你不是很了解，可以先点击链接查看一下，下面也会做简单的讲解。

对象突变是我们改变了一个对象或数组的属性。这与**重新分配**不同，在**重新分配**中，我们指向一个完全不同的对象引用。以下是几个突变与重新分配的例子：

```js
// Mutation
const person = { name: 'BO' }
person.name = 'OB'

// Reassignment
let user = { name: 'D.O', age: 18 }
user = { name: 'IY', type: 17 }
```

我们必须记住，这同样适用于数组：

```js
// Mutation
const course = ['JavaScript', 'HTML', 'CSS']
course[1] = 'Vue'
// course => ["JavaScript", "Vue", "CSS"]

// Reassignment
let user = ['D.O', 'LAY', 'KAI']
user = ['IU', 'BY']
// user => ["IU", "BY"]
```

## 对象突变的意外后果示例

既然我们已经知道了突变是什么，那么突变怎么会有意料之外的后果呢？让我们看看下面的例子。

```js
const person = { name: 'BO' }
const otherPerson = person
otherPerson.name = 'OB'

console.log(person) // { name: "OB" }
```

可以看到，`person` 和 `otherPerson` 都引用同一个对象，因此如果我们在 `otherPerson` 上更改 `name`，那么当我们访问 `person` 时，该更改将得到反映。

与其让我们自己（和我们项目中的开发人员）对这样的对象进行突变，还不如我们抛出一个错误呢？这就是我们不可变 `Proxy` 代理解决方案的用武之地。

## 不可变的代理解决方案

JavaScript `Proxy` 对象使我们可以使用的一种方便的元编程。它允许我们用自定义功能包装对象，例如对象上的 `getter` 和 `setter`。

对于我们的不可变代理，让我们创建一个函数，该函数接受一个对象并为该对象返回一个新的代理。当我们试图 `get` 该对象的属性时，我们检查该属性本身是否为对象。如果是，则以递归方式返回包装在不可变代理中的属性。否则，我们只返回属性。

当我们试图 `set` 设置代理对象的值时，简单地抛出一个错误，让用户知道他们不能 `set` 这个对象的属性。

下面是我们的不可变 `Proxy` 函数：

```js
const user = {
  name: 'Bo',
  friend: [{ name: 'IY', age: 18 }]
}

const immutable = (obj) =>
  new Proxy(obj, {
    get(target, prop) {
      return typeof target[prop] === 'object'
        ? immutable(target[prop])
        : target[prop]
    },
    set() {
      throw new Error('Immutable!')
    }
  })

const immutableUser = immutable(user)

const immutableFriend = immutableUser.friend[0]

immutableFriend.age = 80 // Error: Immutable!
```

可以看到，我们现在不能在一个不可变的对象上改变一个属性！

## 最后

现在，你已经了解了 Proxy 的一些用法。这是很棒的、健壮程序的解决方案。如果你想在你的应用程序中使用包含不可变的数据结构，你可以选择 [Immutable](https://github.com/immutable-js/immutable-js) 和 [Immer](https://github.com/immerjs/immer) 这样成熟的库。
