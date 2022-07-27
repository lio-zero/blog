# 使用 Prettier 格式化代码

[Prettier](https://prettier.io/) 是一个固执己见的代码格式化程序。

![Prettier 的标志](https://camo.githubusercontent.com/81ead5bb324aa1fda40ca50d59979442f87c1d559bfad2018ed02c8a0b9c1f40/68747470733a2f2f756e706b672e636f6d2f70726574746965722d6c6f676f40312e302e332f696d616765732f70726574746965722d62616e6e65722d6c696768742e737667)

它支持许多开箱即用的不同语法，包括：

- HTML
- CSS、SCSS 和 Less
- JavaScript
- Flow 和 TypeScript
- Vue
- JSX
- YAML
- JSON
- GraphQL
- Markdown

通过[插件](https://prettier.io/docs/en/plugins.html)，您可以将其用于 Python、PHP、Swift、Ruby 和 Java 等。

它与最流行的代码编辑器集成，包括 VS Code、Sublime Text、Atom 等。

## 更少的选项

我最近在学习了 Go，Go 最棒的地方之一是 [`gofmt`](https://pkg.go.dev/cmd/gofmt)，这是一个官方工具，可以根据通用标准自动格式化您的代码。

Go 代码的 95%（由统计数据组成）看起来完全一样，因为这个工具可以很容易地执行，而且由于风格是由 Go 维护人员为您定义的，您更有可能适应该标准，而不是坚持自己的风格。例如制表符与空格，或者在何处放置左括号。

这听起来像是一个限制，但它实际上非常强大。所有 Go 代码看起来都一样。

Prettier 与 `gofmt` 类似。

它的选项很少，而且大部分决定已经为您做出，因此您可以停止争论关于风格的任何问题，并专注于您的代码。

## 与 ESLint 的区别

[ESLint](https://eslint.org/) 是一个 linter，它不只是格式化，而且由于对代码的静态分析，它还突出了一些错误提示。

这是一个非常棒的工具，可以与 Prettier 一起使用。

ESLint 还强调了格式化问题，但由于它的可配置性更高，每个人都可以有一套不同的格式化规则。而 Prettier 为所有人提供了一个共同点。

现在，您可以自定义一些内容，例如：

- tab 宽度
- 单引号与双引号
- 行列数
- 尾随逗号

还有其他一些[选项](https://prettier.io/docs/en/options.html)，但 Prettier 试图控制这些定制的数量，以避免变得过于可定制。

## 安装

Prettier 可以从命令行运行，您可以使用 npm、yarn 和 pnpm 安装它。

Prettier 的另一个很好的用例是在您的 Git 存储库的 PR 上运行它，例如在 GitHub 上。

如果您使用受支持的编辑器，最好是直接从编辑器中使用 Prettier，每次保存时都会运行 Prettier 格式。

例如，VS Code 的 [Prettier 扩展](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)。

## Prettier 更适合初学者

如果您认为 Prettier 仅适用于团队或专业用户，那么您就错过了该工具的一个良好价值主张。

良好的风格会强化良好的习惯。

格式化是一个经常被初学者忽视的话题，但拥有清晰一致的格式化是您作为一名新开发人员成功的关键。

此外，即使您在 2 周前开始使用 JavaScript，使用 Prettier，您的代码 - 风格方面 - 看起来就像从 1998 年以来编写 JS 的 JavaScript Guru 编写的代码。

此外，即使您刚开始学习 JavaScript，从代码风格上看，您的代码也会变得更漂亮，就像一位资深大佬编写代码一样。
