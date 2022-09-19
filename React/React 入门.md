# React 入门

## 什么是 React？

React 是一个用于创建用户界面的开源前端 JavaScript 库。它是声明式的、高效的和灵活的，并且它坚持基于组件的方法，这使我们能够创建可重用的 UI 组件。

[React](https://reactjs.org/) 是一个开源的前端 JavaScript 库，用于构建用户界面或 UI 组件。它由 Facebook 和一个由个人开发者和公司组成的社区维护。它本质上是一个渲染组件的库，尽管它们不是传统的 web 组件。

React 使用一种名为 JSX（使用 JavaScript + XML）的语言，开发人员可以在 React 组件中包含所有前端代码（包括 HTML 和 CSS）。

React 最酷的一点是它完全关注组件渲染过程。我喜欢这一点，因为它在某种程度上遵循了[单一职责原则](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%E5%8D%95%E4%B8%80%E8%81%8C%E8%B4%A3%E5%8E%9F%E5%88%99%EF%BC%9F.md)。它将组件渲染在 web 应用中的屏幕上，但不需要担心路由、状态管理或构建整个前端应用所需的其他工具。

从头开始搭建 React 应用可能会有点复杂和难以承受。但您可以在其中学到很多，当你熟悉之后，您可以使用 `create-react-app` 等工具快速构建一个 React 开发环境。

## React 是如何工作的？

React 不是直接操作浏览器的 DOM，而是在内存中创建一个虚拟 DOM（Virtual DOM），在对浏览器 DOM 进行更改之前，它会在其中进行所有必要的操作。

React 会找出已经进行了哪些更改，并且只更改需要更改的内容。这个过程使用到了 diff 算法，通过 diff 算法可以避免一些不必要的 DOM 操作，从而提高了性能。

> 简单快速过一下，虚拟 DOM 是真实 DOM 的任何一种表示形式（例如 JS 的对象表示）。当我们在虚拟 DOM 树中更改某些内容时，我们会得到一个新的虚拟树。diff 算法比较这两种树（旧树和新树），找出差异，并且只对真实 DOM 进行必要的小改动，以反映虚拟 DOM。这种方式除了性能的提升外，也方便与其他环境集成。

本文是一篇 React 入门，这里不在过多解释。

## 如何使用？

### 引入 CDN

你可以直接在 HTML 文件中练习、编写 React，通过 CDN 引入的方式：

```html
<!-- 加载 React 和 React DOM 的 UMD 版本 -->
<script
  crossorigin
  src="https://unpkg.com/react@18/umd/react.development.js"
></script>
<script
  crossorigin
  src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"
></script>
<!-- 使用 JSX 需要用到的 Babel -->
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

一个简单的例子：

```html
<div id="root"></div>

<script type="text/babel">
  function Hello() {
    return <h1>Hello React!</h1>
  }

  ReactDOM.render(<Hello />, document.getElementById('root'))
</script>
```

并使用特殊的 `text/babel` [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)加载您的脚本。

> **Tips**：当然，上述提供的版本仅用于开发环境，您可以使用压缩优化后可用于生产环境的 React 版本。另外，我们为什么要在 `script` 标签上使用 `crossorigin` 属性，这些您都可以在 React 官网中找到。所以看完此文，建议刷一遍文档。

这种使用 React 的方式可以用于测试目的，它无需设置复杂的工作流程即可快速启动。但对于生产，您需要搭建一个 React 环境。

### 使用 `create-react-app`

运行以下命令来创建一个名为 `my-react-app` 的 React 应用：

```bash
npx create-react-app my-react-app
```

进入目录，并启动项目：

```bash
cd my-react-app
npm start
```

启动后将启动跳转到浏览器，效果如下：

![react](https://upload-images.jianshu.io/upload_images/18281896-fae94cbc59e6d36a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

找到 `src` 目录下的 `App.js` 文件，随意修改一些内容，浏览器将自动重新刷新为最新内容。

```jsx
function App() {
  return <div className='App'>Hello React!</div>
}
```

### 使用 Vite

最近在使用 Vite，它旨在为现代 web 项目提供更快、更精简的开发体验。它最初只是作为 [Vue 单文件组件（SFC）](https://vuejs.org/guide/scaling-up/sfc.html)的开发服务器，但现在已经发展成为一个无捆绑的 JavaScript 开发服务器。

这里不过多介绍，我们来看看如何安装 React 环境。

```bash
npm init vite@latest my-react-app
```

根据选项选择 react 即可，vite 会为您提供很轻量的 react 环境。

同样的进入目录，安装依赖包，然后启动项目：

```jsx
cd my-react-app
npm i
npm dev
```

下面我们来看一下 React 的一些基础用法。

> 额外的，您可以使用 [CodePen](https://codepen.io/) 和 [CodeSandbox](https://codesandbox.io/s) 这种在线代码编写平台。快速开发。

## React 渲染 HTML

React 的目标是以多种方式在网页中渲染 HTML。

React 通过使用一个名为 `ReactDOM.render()` 方法。

### render 函数

`ReactDOM.render()` 函数有两个参数，HTML 代码和一个 HTML 元素。

该函数的目的是在指定的 HTML 元素内显示指定的 HTML 代码。

以下将在 `id` 为 `"root"` 的元素内显示一个段落：

```jsx
ReactDOM.render(<p>Hello</p>, document.getElementById('root'))
```

> **注意**：元素 `id` 不必称为 `"root"`，但这是标准约定。

### 根节点

根节点是要在其中显示结果的 HTML 元素。它就像一个由 React 管理的内容的容器。

它不必是一个 `<div>` 元素，也不必具有 `id="root"`。

```jsx
// <main id="app"><main>
ReactDOM.render(<p>Hallo</p>, document.getElementById('app'))
```

## React 中引用 DOM 元素

要在 React 组件中访问 DOM 元素：

```jsx
function App() {
  const inputEl = useRef(null)
  const onButtonClick = () => {
    inputEl.current.focus()
  }
  return (
    <>
      <input ref={inputEl} type='text' />
      <button onClick={onButtonClick}>聚焦</button>
    </>
  )
}
```

使用到 `useRef` 钩子。关于 Hooks 内容可以查阅 [React Hooks](https://github.com/lio-zero/blog/blob/main/React/React%20Hooks.md)。

## React JSX

### 什么是 JSX？

[JSX](https://facebook.github.io/jsx/#sec-intro) 是 JavaScript 的语法扩展，类似 HTML/XML 的结构可以与 JavaScript 一起使用。它允许开发人员使用 HTML 语法组合 JavaScript 组件。这使得有一个清晰而熟悉的语法来定义具有属性的 DOM 树结构成为可能。

虽然它看起来很像 HTML，但实际上它属于一个 JS 文件。由于它是 JavaScript 的一个扩展，除非将其编译成普通 JavaScript，否则浏览器将无法理解它。所以你需要使用一个 JSX 编译器，比如 [Babel](https://babeljs.io/)。

由于我们这里使用了构建工具 Vite 搭建的环境，因此你需要关心需要安装什么依赖，就可以直接在项目中使用 JSX。

### 编写 JSX

JSX 允许我们用 JavaScript 编写 HTML 元素并将它们放置在 DOM 中，而无需任何 `createElement()` 或 `appendChild()` 方法。

JSX 将 HTML 标签转换为 react 元素。

你可以不使用 JSX，但是 JSX 使编写 React 应用变得更容易。

这里有两个例子。第一个使用 JSX，第二个不使用：

```jsx
// 使用 JSX
const elem = <h1>我喜欢使用 JSX!</h1>
ReactDOM.render(elem, document.getElementById('root'))

// 不使用 JSX
const elem = React.createElement('h1', {}, '我不使用 JSX!')
ReactDOM.render(elem, document.getElementById('root'))
```

JSX 是基于 ES6 的 JavaScript 语言的扩展，在运行时被编译为常规的 JavaScript。

### JSX 中的表达式

使用 JSX，您可以在花括号内编写表达式 `{ }`。

表达式可以是 React 变量、属性或任何其他有效的 JavaScript 表达式。JSX 将执行表达式并返回结果：

```jsx
const elem = <h1>使用 JSX 时，React 的性能提高了{5 + 5} 倍</h1>
```

### 插入一大段 HTML

要在多行上编写 HTML，请将 HTML 放在括号内：

```jsx
const elem = (
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
  </ul>
)
```

### 一个顶级元素

HTML 代码必须包含在一个顶级元素中。

所以如果你需要两个段落，你必须把它们放在一个父元素中。

```jsx
const elem = (
  <div>
    <p>我是一段文字</p>
    <p>我也是一段文字</p>
  </div>
)
```

如果 HTML 不正确，或者 HTML 缺少父元素，JSX 将抛出错误。这行不通：

```jsx
const elem = (
  <p>我是一段文字</p>
  <p>我也是一段文字</p>
)
```

解决此问题的一种“经典”方法是将组件和其他 HTML 元素包装在 `div` 内：

```jsx
const elem = (
  <div>
    <p>我是一段文字</p>
    <p>我也是一段文字</p>
  </div>
)
```

一种解决方案是返回一个 JSX 元素数组：

```jsx
const elem = [<p>我是一段文字</p>, <p>我也是一段文字</p>]
```

另一种解决方案是使用 Fragment（片段）：

```jsx
const elem = (
  <Fragment>
    <p>我是一段文字</p>
    <p>我也是一段文字</p>
  </Fragment>
)
```

最新的语法可以使用空的 HTML 标签：`<></>` 表示。

```jsx
const elem = (
  <>
    <p>我是一段文字</p>
    <p>我也是一段文字</p>
  </>
)
```

这将防止不必要地向 DOM 添加额外的节点。

> 推荐：[<> 与 React.Fragment 的区别](https://github.com/lio-zero/blog/blob/main/React/React.Fragment%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)

### 元素必须关闭

JSX 遵循 XML 规则，因此必须正确关闭 HTML 元素。

```jsx
const elem = <input type='text' />
```

如果 HTML 没有正确关闭，JSX 会抛出错误。

### 属性 `class` = `className`

`class` 属性在 HTML 中是一个常用 的属性，但是由于 JSX 被渲染为 JavaScript，并且 `class` 关键字是 JavaScript 中的保留字，所以你不能在 JSX 中使用它。

改用属性 `className`。

JSX 通过使用 `className` 解决了这个问题。当 JSX 被渲染时，`className` 会将属性转换为 `class` 属性。

```jsx
const elem = <h1 className='myclass'>Hello React</h1>
```

### 条件 — `if` 语句

React 支持 `if` 语句，但不支持 JSX。

为了能够在 JSX 中使用条件语句，您应该将 `if` 语句放在 JSX 之外，或者您可以使用三元表达式代替。

在 JSX 代码之外编写 `if` 语句：

```jsx
const x = 5
let text = 'Vue'
if (x < 10) {
  text = 'React'
}

const elem = <h1>{text}</h1>
```

改用三元表达式：

```jsx
const x = 5

const elem = <h1>{x < 10 ? 'React' : 'Vue'}</h1>
```

> 推荐一篇文章：[Good advice on JSX conditionals](https://thoughtspile.github.io/2022/01/17/jsx-conditionals/)
>
> **注意**：为了在 JSX 中嵌入 JavaScript 表达式，JavaScript 必须用大括号括起来，`{}`。

### JSX 自动转义

为了减轻 XSS 漏洞攻击的风险，JSX 强制在表达式中自动转义。

这意味着您在字符串表达式中使用 HTML 实体时可能会遇到问题。

您希望打印以下内容（`© 2020`）：

```jsx
<p>{'&copy; 2020'}</p>
```

但事实并非如此，它将打印 `&copy; 2020`，因为字符串被转义了。

要解决此问题，可以将实体移到表达式之外：

```jsx
<p>&copy; 2020</p>
```

或者通过使用一个常量来打印与您需要打印的 HTML 实体相对应的 Unicode 表示：

```jsx
<p>{'\u00A9 2020'}</p>
```

### 在 JSX 中添加注释

您可以使用表达式中的普通 JavaScript 注释向 JSX 添加注释：

```jsx
<p>
  {/* 注释 */}
  {
    // 另一个注释
  }
</p>
```

### 扩展属性

在 JSX 中，一个常见的操作是为属性赋值。

```jsx
<BlogPost title={data.title} date={data.date} />
```

你可以通过：

```jsx
<BlogPost {...data} />
```

由于 ES6 扩展运算符，`data` 对象的属性将自动用作属性。

表单和样式可往下看。

## React 组件

组件就像返回 HTML 元素的函数。

组件是独立且可重用的代码。它们的用途与 JavaScript 函数相同，但独立工作并返回 HTML。

组件有两种类型，`Class` 组件和 `Function` 组件。

在较旧的 React 代码库中，您可能会发现主要使用 `Class` 组件。现在建议将函数组件与 React 16.8 中添加的 Hooks 一起使用。

创建 React 组件时，组件的名称必须以大写字母开头。

### 类组件

类组件必须包含 `extends React.Component` 语句。此语句创建对 `React.Component` 的继承，并让您的组件访问 `React.Component` 的函数。

该组件还需要一个 `render()` 方法，该方法返回 HTML。

```jsx
class SayHello extends React.Component {
  render() {
    return <h2>Hello React!</h2>
  }
}
```

本文只给出这么一个简单的 `class` 创建组件的示例。

> **Tips**：除了本节内容，本文不在使用类组件。详细类组件用法请查阅官网。

### 函数组件

这是与上面相同的示例，但使用 `Function` 组件创建。

`Function` 组件也返回 HTML，其行为方式与 `Class` 组件非常相似，但 `Function` 组件可以使用更少的代码编写，更易于理解。

```jsx
function SayHello() {
  return <h2>Hello React!</h2>
}
```

### 渲染组件

现在我们的 React 有一个 `SayHello` 的组件，它返回一个 `<h2>` 元素。

要在您的应用中使用此组件，请使用与普通 HTML 类似的语法：`<SayHello />`

```jsx
ReactDOM.render(<SayHello />, document.getElementById('root'))
```

### Props

组件可以作为 `props` 传递，`props` 代表属性。

`props` 类似于函数参数，可以将它们作为属性发送到组件中。

```jsx
function SayHello(props) {
  return <h2>Hello {props.language}!</h2>
}

ReactDOM.render(<SayHello language='React' />, document.getElementById('root'))
```

### 组件中的组件

我们可以在其他组件内部引用组件：

```jsx
function SayHello() {
  return <h2>Hello React!</h2>
}

function Person() {
  return (
    <>
      <h1>我编写的代码输出的第一段内容</h1>
      <SayHello />
    </>
  )
}

ReactDOM.render(<Person />, document.getElementById('root'))
```

### 文件中的组件

React 就是重用代码，建议将组件拆分为单独的文件。

为此，请创建一个带有 `.js` 文件扩展名的新文件并将代码放入其中：

```jsx
function SayHello() {
  return <h2>Hello React!</h2>
}

export default SayHello
```

> **注意**：文件名必须以大写字符开头。

关于如何编写好一个组件，这里整理了一些的资料：

- [React.Component](https://reactjs.org/docs/react-component.html)
- [Components and Props](https://reactjs.org/docs/components-and-props.html)
- [Designing React Components, Best Practices](https://www.thisdot.co/blog/designing-react-components-best-practices)
- [Compound Components In React](https://www.smashingmagazine.com/2021/08/compound-components-react/)
- [How Are Function Components Different from Classes?](https://overreacted.io/how-are-function-components-different-from-classes/)
- [Refactoring a Complex React Component — 5 Best Practices to Write Efficient and Readable Components](https://levelup.gitconnected.com/refactoring-a-complex-react-component-5-best-practices-to-write-efficient-and-readable-components-b0d06f4f22b4)
- [寫 React Components 該注意的 6 個地方與技巧](https://medium.com/@chihsuan/%E5%AF%AB-react-components-%E8%A9%B2%E6%B3%A8%E6%84%8F%E7%9A%846%E5%80%8B%E5%9C%B0%E6%96%B9%E8%88%87%E6%8A%80%E5%B7%A7-faa2bb87a18e)

## React Props

Props 是传递给 React 组件的参数。

`props` 通过 HTML 属性传递给组件。`props`代表属性。

React Props 就像 JavaScript 中的函数参数和 HTML 中的属性。

要将 `props` 发送到组件中，请使用与 HTML 属性相同的语法：

```jsx
const elem = <SayHello language='React' />
```

组件将参数作为 `props` 对象接收：

```jsx
function SayHello(props) {
  return <h2>Hello {props.language}!</h2>
}
```

### 传递数据

`props` 也是您将数据从一个组件传递到另一个组件的方式，作为参数。

```jsx
function SayHello(props) {
  return <h2>Hello {props.language}!</h2>
}

function Person() {
  return (
    <>
      <h1>我编写的代码输出的第一段内容</h1>
      <SayHello language='React' />
    </>
  )
}

ReactDOM.render(<Person />, document.getElementById('root'))
```

如果您有要发送的变量，而不是上面示例中的字符串，则只需将变量名称放在大括号内：

```jsx
function SayHello(props) {
  return <h2>Hello {props.language}!</h2>
}

function Person() {
  const language = 'React'
  return (
    <>
      <h1>我编写的代码输出的第一段内容</h1>
      <SayHello language={language} />
    </>
  )
}

ReactDOM.render(<Person />, document.getElementById('root'))
```

或者如果它是一个对象：

```jsx
function SayHello(props) {
  return <h2>Hello {props.article.language}!</h2>
}

function Person() {
  const article = { language: 'React', price: 99 }
  const carInfo = { name: 'Ford', model: 'Mustang' }
  return (
    <>
      <h1>我编写的代码输出的第一段内容</h1>
      <SayHello language={article} />
    </>
  )
}

ReactDOM.render(<Person />, document.getElementById('root'))
```

> **注意**：React Props 是只读的！如果你试图改变它们的值，你会得到一个错误。

## React 事件

就像 HTML DOM 事件一样，React 可以根据用户事件执行操作。

React 与 HTML 具有相同的事件。例如：`click`、`change`、`onMouseOver` 等。

### 添加事件

React 事件以 camelCase 语法编写：用 `onClick` 代替 `onclick`。

React 事件处理程序写在花括号内，例如：`onClick={shoot}` 而不是 `onClick="shoot()"`。

```jsx
<button onClick={shoot}>发射!</button>
```

将 `shoot` 函数放入 `Football` 组件中：

```jsx
import ReactDOM from 'react-dom'

function Football() {
  const shoot = () => alert('好球!')

  return <button onClick={shoot}>发射!</button>
}

ReactDOM.render(<Football />, document.getElementById('root'))
```

### 传递参数

要将参数传递给事件处理程序，请使用箭头函数。

```jsx
function Football() {
  const shoot = (a: string) => {
    alert(a) // 球门
  }

  return <button onClick={() => shoot('球门!')}>发射!</button>
}
```

**注意**，不能像这样使用：

```jsx
<button onClick={shoot('球门')}>发射!</button>
```

因为 `onClick` 中的表达式将在挂载时执行。一旦应用启动，它将执行并打印"球门"，而不会等待你的点击。

### React 事件对象

事件处理程序可以访问触发函数的 React 事件。

在我们的示例中，事件是 `click` 事件。

```jsx
function Football() {
  const shoot = (a, e) => alert(e.type) // click

  return <button onClick={(e) => shoot('球门!', e)}>发射!</button>
}
```

### 高级：合成事件

在 React 高级指南中，有一个合成事件章节，以下是它的定义：

> [合成事件](https://reactjs.org/docs/events.html)（SyntheticEvent）是 React 模拟原生 DOM 事件所有能力的一个事件对象，即浏览器原生事件的跨浏览器包装器。React 根据 W3C 规范定义了合成事件。

这里不深入讨论它，但有一点需要注意，v16 和 v17 版本中的合成事件有差异，详细内容请查阅官网。

关于合成事件，这里也整理了一些的资料供大家参考：

- [探索 React 合成事件](https://segmentfault.com/a/1190000038251163)
- [大佬，怎么办？升级 React17，Toast 组件不能用了](https://zhuanlan.zhihu.com/p/380941094)
- [What’s the Difference Between Synthetic React Events and JavaScript Events?](https://betterprogramming.pub/whats-the-difference-between-synthetic-react-events-and-javascript-events-ba7dbc742294)
- [React 17 removes event pooling in the modern browsers](https://blog.saeloun.com/2021/04/06/react-17-removes-event-pooling-in-modern-system.html)

> 您可以先忽略这些链接，可以在其他基础部分看完，在回头了解它们。

### 示例：双击编辑文本

- 双击事件使用 `onDoubleClick`，并使用 `toggle` 状态变量，当双击元素时，显示不同的元素
- 使用 `onChange()` 事件侦听器来设置 `name` 变量的值
- 使用 `onKeyDown()` 拦截 `Enter` 或 `ESC` 按键事件并返回显示 `p` 元素

```jsx
import ReactDOM from 'react-dom'
import { useState } from 'react'

function App() {
  const [toggle, setToggle] = useState(true)
  const [name, setName] = useState('test')

  return (
    <>
      {toggle ? (
        <p
          onDoubleClick={() => {
            setToggle(false)
          }}
        >
          {name}
        </p>
      ) : (
        <input
          type='text'
          value={name}
          onChange={(event) => {
            setName(event.target.value)
          }}
          onKeyDown={(event) => {
            if (event.key === 'Enter' || event.key === 'Escape') {
              setToggle(true)
              event.preventDefault()
              event.stopPropagation()
            }
          }}
          autoFocus
        />
      )}
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## React 条件渲染

在 React 中，您可以有条件地渲染组件。有几种方法可以做到这一点。

### `if` 语句

我们可以使用 `if` 运算符来决定要渲染哪个组件。

我们有两个组件：

```jsx
function Car() {
  return <h1>Car!</h1>
}

function House() {
  return <h1>House!</h1>
}
```

现在，我们将创建另一个组件，它根据条件选择要渲染的组件：

```jsx
function Goal(props) {
  const isGoal = props.isGoal
  if (isGoal) {
    return <Car />
  }
  return <House />
}

ReactDOM.render(<Goal isGoal={false} />, document.getElementById('root'))
```

尝试将 `isGoal` 属性更改为 `true`：

```jsx
ReactDOM.render(<Goal isGoal={true} />, document.getElementById('root'))
```

### 逻辑 `&&` 运算符

另一种有条件地渲染 React 组件的另一种方法是使用 `&&` 运算符。

我们可以使用花括号在 JSX 中嵌入 JavaScript 表达式：

```jsx
import ReactDOM from 'react-dom'

function Garage(props: { cars }) {
  const cars = props.cars
  return (
    <>
      <h1>车库</h1>
      {cars.length > 0 && <h2>你的车库里有 {cars.length} 辆车。</h2>}
    </>
  )
}

const cars = ['Ford', 'BMW', 'Audi']

ReactDOM.render(<Garage cars={cars} />, document.getElementById('root'))
```

如果 `cars.length` 等于 `true`，则将渲染 `&&` 后面的表达式。

尝试清空 `cars` 数组：

```jsx
const cars = []
```

### 三元运算符

另一种有条件地渲染元素的方法是使用三元运算符。

```jsx
condition ? true : false
```

我们回到之前的例子，如果 `isGoal` 为 `true`，则返回 `Car` 组件，否则返回 `House` 组件：

```jsx
function Goal(props) {
  const isGoal = props.isGoal
  return <>{isGoal ? <Car /> : <House />}</>
}

ReactDOM.render(<Goal isGoal={false} />, document.getElementById('root'))
```

### 示例：点击时显示不同的组件

导入 `useState` 钩子，并声明了一个 `state` 变量，根据该变量和条件渲染，实现点击时显示不同的组件：

```jsx
import ReactDOM from 'react-dom'
import { useState } from 'react'

const AddTagButton = (props) => <button onClick={props.addTag}>添加标签</button>

const AnotherComponent = () => <p>某些内容</p>

function App() {
  const [state, setState] = useState('start')

  return (
    <>
      {state === 'start' && <AddTagButton addTag={() => setState('add-tag')} />}
      {state === 'add-tag' && <AnotherComponent />}
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

这种根据条件渲染组件的方式，开发者们称它为动态组件。

关于条件渲染，推荐阅读 [7 Ways of Achieving Conditional Rendering in React](https://blog.openreplay.com/7-ways-of-achieving-conditional-rendering-in-react) 这篇文章。

## React 渲染列表

在 React 中，您将使用某种类型的循环来渲染列表。

JavaScript `map()` 数组方法通常是首选方法。

```jsx
import ReactDOM from 'react-dom'

function Car(props) {
  return <li>I have a {props.brand}</li>
}

function User() {
  const cars = ['Ford', 'Ferrari', 'Rolls-Royce']
  return (
    <>
      <h1>您有什么车?</h1>
      <ul>
        {cars.map((name) => (
          <Car brand={name} />
        ))}
      </ul>
    </>
  )
}

ReactDOM.render(<User />, document.getElementById('root'))
```

当您在 Vite 中运行此代码时，它会起作用，但您会收到一条报错信息，提示没有为列表项提供 **key**。

### Key

`key` 允许 React 跟踪元素。

在 React diff 算法中，React 会借助元素的 `key` 值来判断，如果一个项目被更新或删除，只有该项目将被重新渲染，而不是整个列表，从而减少不必要的元素重新渲染。

每个兄弟姐妹/同级元素的 `key` 都必须是唯一的。但它们可以在全局范围内复制。

通常，`key` 应该是分配给每个项目的唯一 ID。但作为最后的手段，您可以使用数组索引作为 `key`。

将上述示例进行重构，以包含 `key`：

```jsx
import ReactDOM from 'react-dom'

function Cars(props) {
  return <li>I have a {props.brand}</li>
}

function User() {
  const cars = [
    { id: 1, brand: 'Ford' },
    { id: 2, brand: 'Ferrari' },
    { id: 3, brand: 'Rolls-Royce' }
  ]
  return (
    <>
      <h1>您有什么车?</h1>
      <ul>
        {cars.map((item) => (
          <Cars key={item.id} brand={item.brand} />
        ))}
      </ul>
    </>
  )
}

ReactDOM.render(<User />, document.getElementById('root'))
```

## React 表单

与 HTML 一样，React 使用表单允许用户与网页交互。

### 在 React 中添加表单

添加一个与任何其他元素一样具有 React 的表单：

```jsx
import ReactDOM from 'react-dom'

function MyForm() {
  return (
    <form>
      <label>
        输入你的名字:
        <input type='text' />
      </label>
    </form>
  )
}

ReactDOM.render(<MyForm />, document.getElementById('root'))
```

这将正常工作，表单将提交，页面将刷新。

但这通常不是我们希望在 React 中发生的事情。我们想阻止这种默认行为，让 React 控制表单。

### 处理表单

处理表单是指当数据改变值或被提交时如何处理数据。

在 HTML 中，表单数据通常由 DOM 处理。在 React 中，表单数据通常由组件处理。

当数据由组件处理时，所有数据都存储在组件状态中。可以通过在 `onChange` 属性中添加事件处理程序来控制更改。

我们可以使用 `useState` 钩子来跟踪每个输入值，并为整个应用提供“单一真实来源”。

```jsx
import { useState } from 'react'
import ReactDOM from 'react-dom'

function MyForm() {
  const [name, setName] = useState('')

  return (
    <form>
      <label>
        输入你的名字:
        <input
          type='text'
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
      </label>
    </form>
  )
}

ReactDOM.render(<MyForm />, document.getElementById('root'))
```

### 提交表单

您可以通过在 `<form>` 的 `onSubmit` 属性中添加事件处理程序来控制提交操作：

```jsx
import { useState } from 'react'
import ReactDOM from 'react-dom'

function MyForm() {
  const [name, setName] = useState('')

  const handleSubmit = (event) => {
    event.preventDefault()
    alert(`你输入的名字是: ${name}`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        输入你的名字:
        <input
          type='text'
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
      </label>
      <input type='submit' />
    </form>
  )
}

ReactDOM.render(<MyForm />, document.getElementById('root'))
```

### 多个输入字段

您可以通过向每个元素添加一个 `name` 属性来控制多个输入字段的值。

我们将使用一个空对象初始化我们的状态。

要访问事件处理程序中的字段，请使用 `event.target.name` 和 `event.target.value` 语法。

要更新状态，请在属性 `name` 周围使用方括号 `[]`。

```jsx
import { useState } from 'react'
import ReactDOM from 'react-dom'

function MyForm() {
  const [inputs, setInputs] = useState({})

  const handleChange = (event) => {
    const name = event.target.name
    const value = event.target.value
    setInputs((values) => ({ ...values, [name]: value }))
  }

  const handleSubmit = (event) => {
    event.preventDefault()
    console.log(inputs) // {username: 'O.O', age: '20'}
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        输入你的名字:
        <input
          type='text'
          name='username'
          value={inputs.username || ''}
          onChange={handleChange}
        />
      </label>
      <label>
        输入你的年龄:
        <input
          type='number'
          name='age'
          value={inputs.age || ''}
          onChange={handleChange}
        />
      </label>
      <input type='submit' />
    </form>
  )
}

ReactDOM.render(<MyForm />, document.getElementById('root'))
```

我们对两个输入字段使用相同的事件处理函数，我们可以为每个字段编写一个事件处理函数，但这让我们的代码更简洁，是 React 中的首选方式。

### textarea

React 中的 `textarea` 元素与普通 HTML 略有不同。

在 HTML 中，`textarea` 的值是开始标签 `<textarea>` 和结束标签 `</textarea>` 之间的文本。

```html
<textarea>
  文本区的内容。
</textarea>
```

在 React 中，`textarea` 的值放置在 `value` 属性中。我们将使用 `useState` 钩子来管理 `textarea` 的值：

```jsx
import { useState } from 'react'
import ReactDOM from 'react-dom'

function MyForm() {
  const [textarea, setTextarea] = useState('textarea 的内容位于 value 属性中')

  const handleChange = (event) => {
    setTextarea(event.target.value)
  }

  return (
    <form>
      <textarea value={textarea} onChange={handleChange} />
    </form>
  )
}

ReactDOM.render(<MyForm />, document.getElementById('root'))
```

### select

React 中的下拉列表或选择框也有点不同于 HTML。

在 HTML 中，下拉列表中的选定值由 `selected` 属性定义：

```html
<select>
  <option value="Ford">Ford</option>
  <option value="Volvo" selected>Volvo</option>
  <option value="Fiat">Fiat</option>
</select>
```

在 React 中，所选值由 `select` 标签上的 `value` 属性定义：

```jsx
function MyForm() {
  const [myCar, setMyCar] = useState('Volvo')

  const handleChange = (event) => {
    setMyCar(event.target.value)
  }

  return (
    <form>
      <select value={myCar} onChange={handleChange}>
        <option value='Ford'>Ford</option>
        <option value='Volvo'>Volvo</option>
        <option value='Fiat'>Fiat</option>
      </select>
    </form>
  )
}
```

通过对 `<textarea>` 和 `<select>` 进行这些细微的更改，React 能够以相同的方式处理所有输入元素。

### value 和 defaultValue

- `value` 属性始终保存字段的当前值
- `defaultValue` 属性保存创建字段时设置的默认值

当检查 `input.value` 和 `input.getAttribute('value')` 返回一个当前值和一个原始默认值时，这有助于解决常规 DOM 交互的一些奇怪行为。

如果我们希望为组件赋予一个初始值，需要将：

- `value` 替换为 `defaultValue`
- `checked` 替换为 `defaultChecked`

```html
<!-- ❌ -->
<input name="enable" type="checkbox" checked="checked" />
<!-- ✅ -->
<input name="enable" type="checkbox" defaultChecked="{true}" />
```

这里涉及到了受控组件和非受控组件，详细内容请查阅 [React 受控组件和非受控组件](https://github.com/lio-zero/blog/blob/main/React/React%20%E5%8F%97%E6%8E%A7%E7%BB%84%E4%BB%B6%E5%92%8C%E9%9D%9E%E5%8F%97%E6%8E%A7%E7%BB%84%E4%BB%B6.md)。

> 额外的，[Formik](https://github.com/jaredpalmer/formik) 库可以简化所有这些表单处理内容并自动化验证、错误处理等。

## React 路由

Vite 构建工具没有为我们添加路由，我们需要自己手动安装依赖。[React Router](https://github.com/remix-run/react-router) 是最流行的解决方案。

> **Tips**：本文使用 React Router 的最新版本。

React Router 其实有由三个包组成，这里我们要在 web 应用上使用，所以我们安装 `react-router-dom`。

添加 React 路由：

```bash
npm i react-router-dom
```

本节写一个简单的示例，在 `src` 文件夹中，我们将创建一个 `pages` 包含多个文件的文件夹：

- `Layout.js`
- `Home.js`
- `Blogs.js`
- `Contact.js`
- `NotFoundPage.js`

每个文件都将包含一个非常基本的 React 组件。

### 基本用法

现在我们将在我们的 `main.js` 文件中使用我们的路由器。

```jsx
// main.js
import ReactDOM from 'react-dom'
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import Layout from './pages/Layout'
import Home from './pages/Home'
import Blogs from './pages/Blogs'
import Contact from './pages/Contact'
import NotFoundPage from './pages/NotFoundPage'

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Layout />}>
          <Route index element={<Home />} />
          <Route path='blogs' element={<Blogs />} />
          <Route path='contact' element={<Contact />} />
          <Route path='*' element={<NotFoundPage />} />
        </Route>
      </Routes>
    </BrowserRouter>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

我们首先用 `<BrowserRouter>` 包装我们的内容，然后定义一个 `<Routes>`，它充当将在我们的应用中创建的所有单独路由的容器/父级。一个应用可以有多个 `<Routes>`（本示例仅使用一个）。

`<Route>` 用于创建单个路由。它包含两个属性：

- `path` 指定所需组件的 URL 路径。
- `element` 指定路由应该渲染的组件。

`<Route>` 可以嵌套。第一个 `<Route>` 具有 `/` 组件的路径，并渲染 `Layout` 组件。

嵌套的 `<Route>` 继承并添加到父路由。因此，`blogs` 路径与父路径合并，成为 `/blogs`。

`Home` 组件路由没有路径，但有 `index` 属性。将此路由指定为父路由的默认路由，即 `/`。

设置 `path` 为 `*` 将作为任何未定义 URL 的回退。这对于 404 错误页面是很好的。

### 页面/组件

`Layout` 组件具有 `<Outlet>` 和 `<Link>` 元素。

- `<Outlet>` 渲染当前选择的路由。
- `<Link>` 用于设置 URL 并跟踪浏览历史记录。

每当我们链接到内部路径时，我们将使用 `<Link>` 而不是 `<a href="">`。

`Layout` 路由是一个共享组件，可以在所有页面上插入公共内容，例如导航菜单。

```jsx
// Layout.js
import { Outlet, Link } from 'react-router-dom'

const Layout = () => {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to='/'>Home</Link>
          </li>
          <li>
            <Link to='/blogs'>Blogs</Link>
          </li>
          <li>
            <Link to='/contact'>Contact</Link>
          </li>
        </ul>
      </nav>
      <Outlet />
    </>
  )
}

export default Layout
```

```jsx
// Home.js
const Home = () => <h1>Home</h1>

export default Home
```

```jsx
// Blogs.js
const Blogs = () => <h1>Blog Articles</h1>

export default Blogs
```

```jsx
// Contact.js
const Contact = () => <h1>Contact Me</h1>

export default Contact
```

```jsx
// NotFoundPage.js
const NotFoundPage = () => <h1>404</h1>

export default NotFoundPage
```

## React CSS

使用 CSS 设置 React 样式的方法有很多，本问将详细介绍三种常用方法：

- 内联样式
- CSS 样式表
- CSS 模块

### 内联样式

要使用内联样式属性设置元素的样式，值必须是 JavaScript 对象：

```jsx
const Header = () => {
  return (
    <>
      <h1 style={{ color: 'plum' }}>Hello Style!</h1>
      <p>添加一点样式!</p>
    </>
  )
}
```

不推荐使用这种方式，这会让代码可读性变差。

> **注意**：在 JSX 中，JavaScript 表达式写在花括号内，由于 JavaScript 对象也使用花括号，所以上例中的样式写在两组花括号内 `{{}}`。

#### camelCased 属性名称

由于内联 CSS 是在 JavaScript 对象中编写的，因此带有连字符分隔符的属性，例如 `background-color`，必须使用驼峰式语法编写：

```jsx
const Header = () => {
  return (
    <>
      <h1 style={{ backgroundColor: 'plum' }}>Hello Style!</h1>
      <p>添加一点样式!</p>
    </>
  )
}
```

#### JavaScript 对象

您还可以创建带有样式信息的对象，并在样式属性中引用它：

```jsx
const Header = () => {
  const myStyle = {
    color: 'white',
    backgroundColor: 'plum',
    padding: '10px',
    fontFamily: 'Sans-Serif'
  }

  return (
    <>
      <h1 style={myStyle}>Hello Style!</h1>
      <p>添加一点样式!</p>
    </>
  )
}
```

### CSS 样式表

您可以在单独的文件中编写 CSS 样式，只需使用 `.css` 文件扩展名保存文件，然后将其导入您的应用。

```scss
// index.scss
body {
  background-color: plum;
  color: white;
  padding: 40px;
  font-family: Sans-Serif;
  text-align: center;
}
```

在您的应用中导入样式表：

```jsx
// main.js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'

const Header = () => {
  return (
    <>
      <h1>Hello Style!</h1>
      <p>添加一点样式!</p>
    </>
  )
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

### CSS 模块

向应用添加样式的另一种方法是使用 CSS 模块。

CSS 模块对于放置在单独文件中的组件很方便。

模块内的 CSS 仅适用于导入它的组件，您不必担心名称冲突。

创建带有 `.module.css` 扩展名的 CSS 模块，例如：`index.module.css`。

```scss
// index.module.css
.bigPlum {
  color: plum;
  padding: 40px;
  font-family: Sans-Serif;
  text-align: center;
}
```

在组件中导入样式表：

```jsx
import styles from './index.module.css'

const SayHello = () => {
  return <h1 className={styles.bigPlum}>Hello React!</h1>
}

export default SayHello
```

导入组件：

```jsx
// main.js
import ReactDOM from 'react-dom'
import SayHello from './SayHello.js'

ReactDOM.render(<SayHello />, document.getElementById('root'))
```

> **扩展**：近几年流行的 CSS-in-JS 也是一种编写组件样式的方式。如果感兴趣可以阅读[什么是 CSS-in-JS？](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20CSS-in-JS.md)以及[使用 Styled Components 编写样式化组件](https://github.com/lio-zero/blog/blob/main/React/%E4%BD%BF%E7%94%A8%20Styled%20Components%20%E7%BC%96%E5%86%99%E6%A0%B7%E5%BC%8F%E5%8C%96%E7%BB%84%E4%BB%B6.md)。

## React SASS

SASS 是一个 CSS 预处理器。

SASS 文件在服务器上执行并将 CSS 发送到浏览器。

> 详细内容可以查看我之前写过的一篇 [SASS 预处理器](https://github.com/lio-zero/blog/blob/master/CSS/SASS%20%E9%A2%84%E5%A4%84%E7%90%86%E5%99%A8.md)。

通过在终端中运行以下命令来安装 SASS：

```bash
npm i sass
```

现在我们可以在项目中包含 SASS 文件了！

创建 SASS 文件的方式与创建 CSS 文件相同，但 SASS 文件具有文件扩展名 `.scss`

在 SASS 文件中，您可以使用变量和其他 SASS 函数：

```scss
// index.scss
$myColor: red;

h1 {
  color: $myColor;
}
```

以与导入 CSS 文件相同的方式导入 SASS 文件：

```jsx
// main.js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.scss'

const Header = () => {
  return (
    <>
      <h1>Hello Style!</h1>
      <p>Add a little style!.</p>
    </>
  )
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

另外，你可以使用 [Autoprefixer](https://github.com/postcss/autoprefixer)，这样 CSS 样式默认会自动添加前缀。

## 最后

这里附带一张 React 学习路线图，您可以借鉴学习。

以下路线图来自 [React Developer](https://roadmap.sh/react)：

![React 开发路线图](https://upload-images.jianshu.io/upload_images/18281896-55f172a3c20bfe98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更多资料

- [React 官网](https://reactjs.org/)
- [Awesome React](https://github.com/enaqx/awesome-react)
- [86 张脑图，一口气看完 React](https://juejin.cn/post/7085145274200358949) — 一篇不错的脑图，整理的也挺全面
