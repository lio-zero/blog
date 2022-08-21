# 实现一个 JavaScript 模板引擎

> 模板引擎（这里特指用于 Web 开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的 HTML 文档。

模板引擎以一种相当简单的方式工作：您创建一个模板，并使用适当的语法将变量传递给它。

例如：ES6 的模块字符串或 Underscore `_.template` 方法，它们都以类似的方式工作。有一些开发者将它们封装成更加健壮、优雅、功能更丰富的模板引擎。

常用的模板引擎有：

- [Pug](https://pugjs.org/)（以前叫 Jade）
- [EJS](https://github.com/mde/ejs)
- [HAML](http://haml.info/docs.html)
- [Handlebars](https://github.com/handlebars-lang/handlebars.js)
- etc

这些模版语言大多是相似的，都提供了用于展示数据的内容替换和过滤器的功能，且大部分模版引擎都支持自定义过滤器，以展示自定义格式的内容。

下面我们来研究一个相对简单的模板引擎示例。

## JavaScript 实现

以下实现的代码来自 [absurd/lib/processors/html/helpers/TemplateEngine.js](https://github.com/krasimir/absurd/blob/master/lib/processors/html/helpers/TemplateEngine.js)：

```js
const TemplateEngine = function (html, options) {
  // re 用于捕获所有以 <% 开头并以 %> 结尾的部分
  let re = /<%(.+?)%>/g,
    // reExp 用于捕获模板语法内的以 if、for... 开头的代码
    reExp = /(^( )?(var|if|for|else|switch|case|break|{|}|;))(.*)?/g,
    // 最终函数体为拼接完的 code，代码在 with 内运行。注意，严格模式下无法使用 with
    code = 'with(obj) { var r=[];\n',
    cursor = 0,
    result,
    match
  // add 函数的作用是，将所有没有被模板语法包裹的内容推入 r 数组中，而 JS 语法则不需要。
  const add = function (line, js) {
    js
      ? (code += line.match(reExp) ? line + '\n' : 'r.push(' + line + ');\n')
      : (code +=
          line != '' ? 'r.push("' + line.replace(/"/g, '\\"') + '");\n' : '')
    return add
  }
  // 使用 exec() 方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 null。
  // 循环调用 add 函数分离文本内容和模板语法
  while ((match = re.exec(html))) {
    // html.slice(cursor, match.index) 用于截取没有被 <% %> 包裹的元素
    // match[1] 获取被 <% %> 包裹内的 JS 代码
    add(html.slice(cursor, match.index))(match[1], true)
    cursor = match.index + match[0].length
  }
  // 最后在检查一遍是否完全转换完模板，一般情况都执行完了
  add(html.substr(cursor, html.length - cursor))
  // 在说一遍，code 保存函数的主体部分
  // return r.join(""); 将最终的内容转为字符串并返回
  // 使用 replace 将拼接的代码中包含的回车、制表符、换行符替换为空
  // 转义字符 — \r 回车、\t tab 制表符、\n 换行符，g 标志全局匹配，replace 替换匹配的元素
  code = (code + 'return r.join(""); }').replace(/[\r\t\n]/g, '')
  console.log(code) // <- 这里打印一下，一目了然
  try {
    // 调用 apply 方法改变函数 this 指向，并传递 options 数组
    result = new Function('obj', code).apply(options, [options])
  } catch (err) {
    console.error(`'${err.message}'`, ' in \n\nCode:\n', code, '\n')
  }
  return result
}
```

示例：

```js
const template =
  'My skills:' +
  '<% if(this.showSkills) { %>' +
  '<% for(let value of this.skills) { %>' +
  '<a href="#"><% value %></a>' +
  '<% } %>' +
  '<% } else { %>' +
  '<p>none</p>' +
  '<% } %>'

TemplateEngine(template, {
  skills: ['js', 'html', 'css'],
  showSkills: true
}) // My skills:<a href="#">js</a><a href="#">html</a><a href="#">css</a>
```

它还配有一篇文章，详细解读了如何使用 20 行代码实现 JavaScript 模板引擎：[JavaScript template engine in just 20 lines](https://krasimirtsonev.com/blog/article/JavaScript-template-engine-in-just-20-line)。

您可以在此基础上添加其他的逻辑，以丰富它的功能。

## 更多资料

- [underscore 系列之实现一个模板引擎(上)](https://github.com/mqyqingfeng/Blog/issues/63)
- [underscore 系列之实现一个模板引擎(下)](https://github.com/mqyqingfeng/Blog/issues/70)
- [Underscore.js `_.template`](http://underscorejs.org/#template)
- [JavaScript 模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Template_literals)
