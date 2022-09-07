# TypeScript 命名空间

我们在项目开发中会不断的引入第三方库以方便开发，这会引起一个问题：全局命名空间被污染，导致全局命名空间中组件之间的名称冲突。因此，我们需要使用命名空间组织代码块，以便唯一地标识变量、对象和类。

在本文中，我们将讨论命名空间、何时需要它们，以及如何使用它们来增强 TypeScript 代码的组织。

## 什么是命名空间？

命名空间是组织代码的范例，以便变量、接口、函数或类在局部作用域内组合在一起，以避免全局作用域内组件之间的命名冲突。这是减少全局作用域污染的最常见策略之一。

虽然模块也用于代码组织，但命名空间是一种更加简单的实现。

> **扩展**：模块还提供了一些额外的好处，例如强大的代码隔离、对捆绑的支持、组件的重新导出以及命名空间不提供的组件重命名。

## 为什么需要命名空间？

命名空间具有以下优点：

- 代码复用性
- 减少全局作用域中的代码量，使其不那么臃肿
- 第三方库 — 随着依赖第三方库数量不断增加，使用命名空间保护您的代码以防止您的代码和第三方库之间的同名冲突非常重要
- 分布式开发 — 随着分布式开发的流行，污染几乎是不可避免的，因为开发人员可以更容易地使用公共变量或类名。这会导致命名冲突和全局作用域的污染

## 使用命名空间的设计注意事项

### 隐式依赖顺序

在使用某些外部库时使用命名空间将需要在您的代码和这些库之间隐式实现依赖关系。这导致您自己管理依赖项以便正确加载它们的压力，因为依赖项可能容易出错。

如果您发现自己处于这种情况，使用模块可以减轻您的压力。

### Node.js 应用

对于 Node.js 应用，建议使用模块而不是命名空间，因为模块是 Node 中封装和代码组织的实际标准。

### 非 JavaScript 内容导入

在处理非 JavaScript 内容时，推荐使用模块而不是命名空间，因为一些模块加载器（例如 [SystemJS](https://github.com/systemjs/systemjs) 和 AMD）允许导入非 JavaScript 内容。

### 遗留代码

当使用不再设计但不断修补的代码库时，建议使用命名空间而不是模块。

此外，在移植旧的 JavaScript 代码时，命名空间会派上用场。

### `module` 和 `namespace` 关键字

TypeScript 中**内部模块**现在叫做**命名空间**。**外部模块**现在则简称为**模块**。

这是为了与 [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) 里的术语保持一致，也就是说 `module module_name { ... }` 相当于现在推荐的写法 `namespace namespace_name { ... }`。

任何使用 `module` 关键字来声明一个内部模块的地方都应该使用 `namespace` 关键字来替换。这就避免了让新的使用者被相似的名称所迷惑。

## 探索 TypeScript 中的命名空间

现在我们对 TypeScript 命名空间是什么以及为什么需要它们有了共同的理解，我们可以更深入地了解如何使用它们。

鉴于 TypeScript 是 JavaScript 的超集，它的命名空间概念源自 JavaScript。

默认情况下，JavaScript 没有命名空间的规定，然而随着 JS 的发展，社区发掘了 IIFE（立即调用函数表达式）方法来实现命名空间：

```js
var user
;(function (user) {
  let name = 'O.O'
})(user || (user = {}))
```

这是用于定义命名空间的大量代码。然而，TypeScript 的处理方式有所不同。

### 单文件命名空间

在 TypeScript 中，命名空间是使用 `namespace` 关键字后跟选择的名称来定义的。

单个 TypeScript 文件可以具有所需的任意多个命名空间：

```ts
namespace User {}
namespace Person {}
```

正如我们所看到的，与使用 IIFE 的 JavaScript 命名空间实现相比，TypeScript 命名空间是一个语法糖。

函数、变量和类可以在命名空间内定义，如下所示：

```ts
namespace User {
  const name = 'O.O'
  function getName() {
    return `${name}`
  }
}

namespace Person {
  const name = 'D.O'
  function getName() {
    return `${name}`
  }
}
```

上面的代码允许我们使用相同的变量和函数名而不会发生冲突。

#### 访问命名空间外的变量、对象、函数和类

为了访问命名空间之外的函数或类，必须在函数或类名之前添加 `export` 关键字，如下所示：

```ts
namespace User {
  const name = 'O.O'
  export function getName() {
    return `${name}`
  }
}
```

注意，如果变量 `name` 在外部不应该被访问，那么不必为它添加 `export` 关键字。

现在，我们可以访问 `getName` 方法，如下所示：

```js
User.getName() // O.O
```

#### 嵌套命名空间

命名空间支持嵌套，即你可以将命名空间定义在另外一个命名空间内。

```ts
namespace namespace_name1 {
  export namespace namespace_name2 {
    const name = 'O.O'
    export function getName() {
      return `${name}`
    }

    export interface NumberValidator {
      age: number
    }
  }
}

// 访问套命名空间
let user: namespace_name1.namespace_name2.NumberValidator = {
  age: 20
}

namespace_name1.namespace_name2.getName() // O.O

console.log(user) // { age: 20 }
```

#### 命名空间别名

对于深度嵌套的命名空间，命名空间别名非常方便，可以保持整洁。

命名空间别名是使用 `import` 关键字定义的，如下所示：

```ts
import rename = namespace_name1.namespace_name2

rename.getName() // O.O
```

### 多文件命名空间

命名空间可以在多个 TypeScript 文件之间共享。可以通过使用三斜杠 `///` 引用它，并在 `reference` 标签的 `path` 属性上指定对应的文件。语法格式如下：

```ts
/// <reference path="path.ts" />
```

以下通过一个示例加深印象，假设我们有如下内容：

```ts
// constant.ts
export const name = 'O.O'

// user.ts
/// <reference path="constant.ts" />

export namespace User {
  export function getName() {
    return `${name}`
  }
}
```

在这里，我们必须引用 `constant.ts` 文件才能访问 `name`：

```ts
// index.ts
/// <reference path="constant.ts" />
/// <reference path="user.ts" />

User.getName() // O.O
```

请注意，我们是如何使用最高级别的命名空间开始引用的。这是如何处理多文件接口中的引用。TypeScript 在编译文件时将使用此顺序。

我们可以使用以下命令指示编译器将多个 TypeScript 文件编译成单个 JavaScript 文件：

```bash
tsc --outFile index.js index.ts
```

使用此命令，TypeScript 编译器将生成一个名为 `index.js` 的 JavaScript 文件。

## 最后

为了构建可扩展和可重用的 TypeScript 应用，TypeScript 命名空间非常方便，因为它们改进了我们应用的组织和结构。

## 更多资料

[TypeScript handbook: Namespaces](https://www.typescriptlang.org/docs/handbook/namespaces.html)
