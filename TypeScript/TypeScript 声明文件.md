# TypeScript 声明文件

TypeScript 作为 JavaScript 的超集，在开发过程中不可避免要引用其他第三方的 JavaScript 库。

虽然通过直接引用可以调用库的类和方法，但是却无法使用 TypeScript 诸如类型检查等特性。

为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。

使用 `declare` 关键字来定义它的类型，帮助 TypeScript 判断我们传入的参数类型对不对。

`declare` 定义的类型只会用于编译时的检查，编译结果中将会被删除。

声明文件以 `.d.ts` 为后缀：

```txt
index.d.ts
```

声明文件模块的语法格式如下：

```ts
declare module Module_Name {}
```

TypeScript 引入声明文件语法格式：

```ts
/// <reference path="lib.d.ts" />
```

很多流行的第三方库的声明文件不需要我们定义，比如 jQuery 已经有人帮我们定义好了：[jQuery in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/jquery/index.d.ts)。

[TypeScript DefinitelyTyped 项目](https://github.com/lio-zero/blog/blob/main/TypeScript/TypeScript%20DefinitelyTyped%20%E9%A1%B9%E7%9B%AE.md)
