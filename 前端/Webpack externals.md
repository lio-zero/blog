# Webpack externals

[Webpack `externals`](https://webpack.js.org/configuration/externals/) 告诉 Webpack 从 bundle 中排除某个导入。`external` 通常用于排除将通过 CDN 加载的导入。

例如，假设您使用 Vue 和 Express 实现服务器端渲染，但客户端代码通过 CDN 导入 Vue。假设您有以下 `component.js` 文件：

```js
const Vue = require('vue')

module.exports = Vue.component('hello', {
  props: ['name'],
  template: '<h1>Hello, {{name}}</h1>'
})
```

以上 `component.js` 在 Node.js 中使用服务器端渲染效果非常好。但是，如何将上述组件与以下的 `index.html` 文件一起使用呢？

```html
<html>
  <body>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>

    <div id="content"></div>
    <script src="dist/component.min.js"></script>
    <script>
      new Vue({ template: '<hello name="World" />' }).mount(
        document.querySelector('#content')
      )
    </script>
  </body>
</html>
```

下面的 `webpack.config.js` 将 `vue` 添加为 `externals`，这意味着 Webpack 不会捆绑 Vue。相反，当 `component.js` 调用 `require('vue')` 时，Webpack 将返回 `global.Vue`。

```js
module.exports = {
  entry: {
    component: `${__dirname}/component.js`
  },
  output: {
    path: `${__dirname}/dist`,
    filename: '[name].min.js'
  },
  target: 'web',
  externals: {
    // 去掉 require('vue')，返回 global.Vue
    vue: 'Vue'
  }
}
```

## 排除 Node.js Polyfills

`externals` 的另一个用例是需要在 Node.js 中使用 polyfill 的浏览器 API，比如 `FormData`。如果您正在测试需要在 Node.js 中使用 `FormData` 的代码，您需要使用类似 [**form-data** npm 模块](https://www.npmjs.com/package/form-data)的 polyfill。

```js
const axios = require('axios')
const FormData = require('form-data')

const form = new FormData()
form.append('key', 'value')
const res = await axios.post('https://example.com/post', form)
console.log(res.data)
```

而 `FormData` 又是一个浏览器 API，所以在编译上述代码时不需要捆绑。因此，您可以将 `form-data` 添加到 `externals`：

```js
module.exports = {
  entry: {
    http: `${__dirname}/http.js`
  },
  output: {
    path: `${__dirname}/dist`,
    filename: '[name].min.js'
  },
  target: 'web',
  externals: {
    // 去掉 require('form-data')，返回 global.FormData
    'form-data': 'FormData'
  }
}
```
