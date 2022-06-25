# TypeScript DefinitelyTyped 项目

广泛的 JavaScript 生态系统中的 npm 包并不总是有可用的类型。

有时项目不再维护，有时他们不感兴趣、不同意或没有时间使用 TypeScript。

## 在 TypeScript 中使用非类型化 npm 包

由于缺少类型，将非类型化的 npm 包与 TypeScript 一起使用将不是类型安全的。

为了帮助 TypeScript 开发人员使用这些包，有一个社区维护的项目叫做 [DefinitelyTyped](http://definitelytyped.org/)。

DefinitelyTyped 是一个为没有类型的 npm 包提供 TypeScript 定义的中央存储库的项目。

```bash
npm i -D @types/jquery
```

安装声明包后，通常不需要其他步骤来使用类型，TypeScript 会在使用包本身时自动选择类型。

当缺少类型时，VS Code 等编辑器通常会建议安装此类包。
