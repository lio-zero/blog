# React 不变性

在 React 中编程时，你可能会遇到一个概念，即不变性（Immutability），与之相反的是可变性（mutability）。

这是一个有争议的话题，但无论你如何看待不可变的概念，React 和它的大多数生态系统都在某种程度上推动了这一点，所以你至少需要理解它为什么如此重要以及它的含义。

在编程中，当变量的值在创建后不能更改时，它就是不可变的。

在操作字符串时，你已经在使用不可变变量。默认情况下，字符串是不可变的，实际上，当你更改它们时，会创建一个新字符串并将其分配给相同的变量名。

不可变变量永远不能被改变。要更新它的值，你需要创建一个新变量。

这同样适用于对象和数组。

要添加新项，你无需更改数组，而是通过连接旧数组和新项来创建新数组。

对象从不更新，而是在更改之前复制它。

> **推荐**：以上内容在[变量赋值与原始对象可变性](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%8F%98%E9%87%8F%E8%B5%8B%E5%80%BC%E4%B8%8E%E5%8E%9F%E5%A7%8B%E5%AF%B9%E8%B1%A1%E5%8F%AF%E5%8F%98%E6%80%A7.md)有介绍到，并提供了示例。

这在很多地方都适用于 React。

例如，你永远不应该直接改变组件的状态属性，而只能通过 `setState()` 方法。

在 Redux 中，你永远不会直接改变状态，而只能通过 `reducer`，也就是函数来改变状态。

**问题是，为什么?**

有多种原因，其中最重要的是：

- 突变可以集中化，就像 Redux 一样，这提高了调试能力并减少了错误来源。
- 代码看起来更清晰、更容易理解。你永远不会期望一个函数在你不知道的情况下改变某个值，这给了你可预测性。当一个函数不改变对象而只是返回一个新对象时，它被称为[纯函数](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E7%BA%AF%E5%87%BD%E6%95%B0.md)。
- 库可以优化代码，因为 JavaScript 在将旧对象引用替换为全新对象时速度更快，而不是改变现有对象。这将为你提升性能。

## 最后

你可以选择 [immutability-helper](https://github.com/kolodny/immutability-helper)、[Immutable](https://github.com/immutable-js/immutable-js) 或 [Immer](https://github.com/immerjs/immer) 这样成熟的库来创建不可变状态树。

## 更多资料

- [使用 Proxy 对象来健壮您的 JavaScript 不变性函数](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BD%BF%E7%94%A8%20Proxy%20%E5%AF%B9%E8%B1%A1%E6%9D%A5%E5%81%A5%E5%A3%AE%E6%82%A8%E7%9A%84%20JavaScript%20%E4%B8%8D%E5%8F%98%E6%80%A7%E5%87%BD%E6%95%B0.md)
- [为什么说 90% 的情况下，immer 完胜 immutable？](https://juejin.cn/post/7142814989307346981)
