# 浏览器中使用原生的 ES 模块

通过在 `script` 标签上添加 `type="module"`，我们可以直接在浏览器中使用原生 ES 模块。

```html
<script type="module">
  import lodash from 'https://cdn.skypack.dev/lodash'
</script>
```

关于 ES 模块，详细内容请查阅 MDN [JavaScript 模块](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules)和 [JavaScript 模块化发展](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E6%A8%A1%E5%9D%97%E5%8C%96%E5%8F%91%E5%B1%95.md)。

还需要注意，普通的脚本和加上 `type="module"` 的脚本对 CORS（跨源资源共享）的处理方式不同。

如果你没有在服务器上设置允许服务第三方源的 CORS 策略，则请求将失败。

## 动态导入 — `import()`

在 ES11 之前，我们只能使用静态导入，它只接受模块路径的字符串。（上一节的方式）

ES11 引入 `import()` 动态导入方法，我们可以使用 Promise 有条件地导入模块。

这种方式也使得调试 `lodash` 这些类似的工具库更加方便了。

```js
const lodash = await import('https://cdn.skypack.dev/lodash')

lodash.get({ a: 3 }, 'a') // 3
```

你可以直接在浏览器控制台测试效果。需要注意的是，如果网站开启了 CSP（内容安全策略），将会失败。

## ImportMap

`import` 每次都需要输入完整的 URL，相对以前的裸导入，很不太方便。例如：

```js
import lodash from 'lodash'
```

它不同于 Node.js 可以依赖文件系统，层层寻找 `node_modules`：

```url
/home/app/packages/project-a/node_modules/lodash/index.js
/home/app/packages/node_modules/lodash/index.js
/home/app/node_modules/lodash/index.js
/home/node_modules/lodash/index.js
```

在 ES 模块中，可以通过 `importmap` 使得裸导入正常工作：

```html
<script type="importmap">
  {
    "imports": {
      "lodash": "https://cdn.skypack.dev/lodash"
    }
  }
</script>
```

此时可与以前同样的方式进行模块导入：

```js
import lodash from 'lodash'

import('lodash').then((_) => _.get({ a: 3 }, 'a'))
```

**那么通过裸导入如何导入子路径呢？**

```html
<script type="importmap">
  {
    "imports": {
      "lodash": "https://cdn.skypack.dev/lodash",
      "lodash/": "https://cdn.skypack.dev/lodash/"
    }
  }
</script>

<script type="module">
  import get from 'lodash/get.js'
</script>
```

> 推荐：[Everything You Need to Know About JavaScript Import Maps](https://www.honeybadger.io/blog/import-maps/)

## Import Assertion

在之前的文章中，我有提到过 Chrome 91 已经引入了 JSON 模块，可以在 ES 模块中引入 JSON 文件。

```html
<script type="module">
  import data from './data.json' assert { type: 'json' }

  console.log(data)
</script>
```

相关文章：

- [ES JSON 模块提案](https://github.com/lio-zero/blog/blob/main/JavaScript/ES%20JSON%20%E6%A8%A1%E5%9D%97%E6%8F%90%E6%A1%88.md)
- [在 ES 模块（Node.js）中导入 JSON 文件](https://github.com/lio-zero/blog/blob/main/Node/%E5%9C%A8%20ES%20%E6%A8%A1%E5%9D%97%EF%BC%88Node.js%EF%BC%89%E4%B8%AD%E5%AF%BC%E5%85%A5%20JSON%20%E6%96%87%E4%BB%B6.md)

## 关于 ES 模块的注意事项

- `module` 默认是 `defer` 的加载和执行方式
- `module` 存在跨域
- `module` 会存放在单独的域内，不会污染全局环境
- `module` 默认开启严格模式（`use strict`）

> 推荐：[JavaScript 中的模块发展方案](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E7%9A%84%E6%A8%A1%E5%9D%97%E5%8F%91%E5%B1%95%E6%96%B9%E6%A1%88.md)

## 最后

随着近几年浏览器对原生 ES 模块的普遍支持，社区出现了一种 Bundless 的构建方案，其具有代表性的包括 vite、snowpack 等。

关于 Bundless 的介绍，可以看看 [Bundle-less 的深入思考和实践](https://juejin.cn/post/7131925778911985701)

<!-- https://juejin.cn/post/7127449194389831716#heading-0 -->
