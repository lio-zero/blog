# TypeScript 基础 — namespace 命名空间

随着第三方库在项目中的增加，我们经常会遇到全局命名空间被污染的问题，导致全局命名空间中组件之间的名称冲突。因此，我们需要使用命名空间组织代码块，以便唯一地标识变量、对象和类。

在本文中，我们将讨论命名空间、何时需要它们，以及如何使用它们来增强 TypeScript 代码的组织。

## 什么是命名空间？

命名空间是组织代码的范例，以便变量、函数、接口或类在局部范围内组合在一起，以避免全局范围内组件之间的命名冲突。这是减少全局范围污染的最常见策略之一。

虽然模块也用于代码组织，但命名空间很容易用于简单的实现。模块还提供了一些额外的好处，例如强大的代码隔离、对捆绑的强大支持、组件的重新导出以及命名空间不提供的组件重命名。

## 为什么我们需要命名空间？

命名空间具有以下优点：

- 代码可重用性 —— 不能低估命名空间对代码可重用性的重要性。
- 膨胀的全局作用域 —— 命名空间减少了全局作用域中的代码量，使其不那么臃肿。
- 第三方库 —— 随着依赖第三方库的数量不断增加，使用命名空间保护您的代码以防止您的代码和第三方库之间的同名冲突非常重要。
- 分布式开发 —— 随着分布式开发的流行，污染几乎是不可避免的，因为开发人员可以更容易地使用公共变量或类名。这会导致名称冲突和全局范围的污染。

## 探索 TypeScript 中的命名空间

鉴于 TypeScript 是 JavaScript 的超集，它的命名空间概念源自 JavaScript。

默认情况下，JavaScript 没有命名空间的规定，因此我们必须使用 IIFE（立即调用函数表达式）来实现命名空间：

```js
var Validation
;(function (Validation) {
  let name = 'O.O'
})(Validation || (Validation = {}))
```

这是用于定义命名空间的代码。同时，它与 TypeScript 的处理方式有所不同。

### 单文件命名空间

TypeScript 中命名空间使用 `namespace` 关键字来定义，语法格式如下：

```ts
namespace Validation {
  export interface ObjectValidator {}
}
```

以上定义了一个命名空间 `Validation`，如果我们需要在外部可以调用 `Validation` 中的类和接口，则需要在类和接口添加 `export` 关键字。

> **注意**：变量不应该使用 `export` 关键字，因为它不应该在命名空间之外被访问。

如果要在同一个文件内的另外一个命名空间调用语法格式为：

```ts
Validation.ObjectValidator
```

**单文件命名空间示例**

```ts
// a.ts
namespace Validation {
  export interface NumberValidator {
    num: Number
  }
}

// 同一个文件内命名空间引用
let obj: Validation.NumberValidator = {
  num: 1
}

console.log(obj) // { num: 1 }
```

### 多文件命名空间

命名空间可以在多个 TypeScript 文件之间共享。可以通过使用三斜杠 `///` 引用它，并 `reference` 标签的 `path` 指定对应的文件。语法格式如下：

```ts
/// <reference path = "xxx.ts" />
```

**多文件命名空间示例**

```ts
// b.ts
/// <reference path = "a.ts" />
let obj1: Validation.NumberValidator = {
  num: 2
}

console.log(obj1)
```

这里，我们引用了上面**单文件命名空间**的 `a.ts` 文件。

### 嵌套命名空间

命名空间支持嵌套，即你可以将命名空间定义在另外一个命名空间里头。

```ts
namespace namespace_name1 {
  export namespace namespace_name2 {
    const name = 'O.O'
    export function getName() {
      console.log(name)
    }

    export interface NumberValidator {
      a: Number
    }
  }
}

let obj: namespace_name1.namespace_name2.NumberValidator = {
  a: 1
}

namespace_name1.namespace_name2.getName() // O.O

console.log(obj) // { a: 1 }
```

### 命名空间别名

对于深度嵌套的命名空间，命名空间别名非常方便，可以保持整洁。

命名空间别名是使用 `import` 关键字定义的，如下所示：

```ts
import rename = namespace_name1.namespace_name2
rename.getName() // O.O
```

TypeScript 中**内部模块**现在叫做**命名空间**。 **外部模块**现在则简称为**模块**。

这是为了与 [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) 里的术语保持一致，(也就是说 `module X {` 相当于现在推荐的写法 `namespace X {`)

任何使用 `module` 关键字来声明一个内部模块的地方都应该使用 `namespace` 关键字来替换。这就避免了让新的使用者被相似的名称所迷惑
