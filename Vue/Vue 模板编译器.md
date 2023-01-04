# Vue 模板编译器

[`vue-template-compiler`](https://www.npmjs.com/package/vue-template-compiler) 模块是将 vue 模板和 [SFC](https://vuejs.org/guide/scaling-up/sfc.html)（单文件组件）编译为 JavaScript 的工具。

大多数开发人员不会使用到 `vue-template-compiler`。但是像 Webpack 的 [`vue-loader`](https://www.npmjs.com/package/vue-loader) 这样的打包工具使用 `vue-template-compiler` 来完成实际编译 `.vue` 文件的繁重工作。

`vue-template-compiler` 有两个主要功能：

- 将模板转换为 [`render()` 函数](https://vuejs.org/guide/extras/render-function.html)
- 解析 SFC

## 编译模板为渲染函数

Vue 模板只是一个普通的字符串。`vue-template-compiler` 的 `compile()` 函数转换模板字符串，你可以将其用作组件的 `render()` 函数。

```js
const compiler = require('vue-template-compiler')
const { renderToString } = require('vue-server-renderer').createRenderer()

// 基于字符串模板编译 render() 函数
const { render } = compiler.compileToFunctions('<h1>Hello {{ message }}</h1>')

Vue.component('hello', {
  props: ['message'],
  render
})

const app = new Vue({
  template: '<hello message="Vue"></hello>'
})

const data = await renderToString(app)

console.log(data) // <h1 data-server-rendered="true">Hello Vue</h1>
```

## 解析 `.vue` 文件

`vue-template-compiler` 有一个 [`parseComponent()`](https://www.npmjs.com/package/vue-template-compiler#compilerparsecomponentfile-options) 函数，可以将单文件组件（`.vue` 文件）编译为 JavaScript。

```js
const compiler = require('vue-template-compiler')

const parsed = compiler.parseComponent(`
  <template>
    <h1>{{ message }}</h1>
  </template>
  <script>
    export default {
      data() {
        return { message: 'Hello Vue' }
      }
    }
  </script>
`)

// 包含 template 和 data 属性
const appComponent = Object.assign(
  { template: parsed.template.content },
  eval(parsed.script.content)
)

Vue.component('app', appComponent)

const app = new Vue({
  template: '<app />'
})

const data = await renderToString(app)

console.log(data) // <h1 data-server-rendered="true">Hello Vue</h1>
```
