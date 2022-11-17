# ECMAScript

每当你阅读 JavaScript 时，你都会不可避免地看到以下术语之一：

- ES3
- ES5
- ES6、ES7 ... ES13
- ES2015、ES2016 ... ES2022
- ECMAScript 2015、ECMAScript 2016 ... ECMAScript 2022

**它们是什么意思呢？**

它们都指的是一个标准，称为 [ECMAScript](https://www.ecma-international.org/)。

ECMAScript 是 JavaScript 所基于的标准，通常缩写为 ES。

除了 JavaScript，其他语言也实现了 ECMAScript，包括：

- ActionScript（Flash 脚本语言），由于 Flash 于 2020 年正式停产，它正在失去生气
- JScript（微软脚本方言），因为当时 JavaScript 仅由 Netscape 支持，浏览器大战正处于顶峰，所以 微软不得不为 Internet Explorer 构建自己的版本

当然，JavaScript 是 ES 中最流行和使用最广泛的实现。

**为什么取这个奇怪的名字**？Ecma International 是瑞士标准协会，负责制定国际标准。

创建 JavaScript 时，它由 Netscape 和 Sun Microsystems（Java 的制造商）提供给 Ecma，他们给它命名为 ECMA-262 别名 ECMAScript。

> 根据 [Wikipedia](https://en.wikipedia.org/wiki/ECMAScript) 的说法，Netscape 和 Sun Microsystems 发布的这份新闻稿可能有助于确定名称选择，其中可能包括委员会成员微软的法律和品牌问题。

在 IE9 之后，微软停止将其在浏览器中的 ES 支持标记为 JScript，并开始将其称为 JavaScript。

所以到 201x 为止，支持 ECMAScript 规范的唯一流行语言是 JavaScript。

## 什么是 TC39？

TC39 是发展 JavaScript 的委员会。

TC39 的成员是涉及 JavaScript 和浏览器供应商的公司，包括 Mozilla、Google、Facebook、Apple、Microsoft 等。

## ECMASCript 语法提案的批准流程

每个标准版本的语法提案都必须经过 5 个阶段：

- Stage 0（Strawperson）— 展示阶段
- Stage 1（Proposal）— 征求意见阶段
- Stage 2（Draft）— 草案阶段
- Stage 3（Candidate）— 候选阶段
- Stage 4（Finished）— 定案阶段

当语法特性到达 Stage 4 时，该特性将随当年的版本发布，成为正式的 ECMAScript 标准。

但是，JS 引擎例如 V8（Chrome 和 Node.js）或 SpiderMonkey（Firefox）会试验性地支持 Stage 4 之前的语法特性，这样开发者可以进行测试和反馈。

详细内容请查阅 [The TC39 Process](https://tc39.es/process-document/)。

## ES 版本

我发现令人困惑的是，为什么有时 ES 版本是根据版本号而有时是根据年份来引用的，而年份恰好在数字上 -1，这增加了 JS/ES 的普遍困惑。

在 ES2015 之前，ECMAScript 规范通常以其版本命名。因此 ES5 是 2009 年发布的 ECMAScript 规范更新的正式名称。

**为什么会发生这种情况**？在 ES2015 诞生的过程中，名称从 ES6 更改为 ES2015，但由于这一更改时间较晚，人们仍然将其称为 ES6。

这张表应该清楚一点：

| 版    | 正式名字 | 发布日期      |
| ----- | -------- | ------------- |
| ES13  | ES2022   | 2022 年 6 月  |
| ES12  | ES2021   | 2021 年 6 月  |
| ES11  | ES2020   | 2020 年 6 月  |
| ES10  | ES2019   | 2019 年 6 月  |
| ES9   | ES2018   | 2018 年 6 月  |
| ES8   | ES2017   | 2017 年 6 月  |
| ES7   | ES2016   | 2016 年 6 月  |
| ES6   | ES2015   | 2015 年 6 月  |
| ES5.1 | ES5.1    | 2011 年 6 月  |
| ES5   | ES5      | 2009 年 12 月 |
| ES4   | ES4      | 弃            |
| ES3   | ES3      | 1999 年 12 月 |
| ES2   | ES2      | 1998 年 6 月  |
| ES1   | ES1      | 1997 年 6 月  |

ES3 至 ES5 中间有长达九年的时间没有更新，期间发布的 ES4 被弃用（至于为什么可以看最后提供的链接）。

可以看到，TC39 通常会在每月的 6 月份发布 ECMAScript 标准。

## ES Next

ES.Next 是一个始终表示 JavaScript 的下一个版本的名称。

所以在写这篇文章的时候，ES2022 已经发布了，ES.Next 就是 ES2023。

## 更多资料

- [从 ES4 历史做的一些不友好的揣测](https://segmentfault.com/a/1190000008550491)
- [ECMAScript 6 会重蹈 ECMAScript 4 的覆辙吗？](https://www.zhihu.com/question/24715618/answer/34813745)
