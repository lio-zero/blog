# React 组合组件

在编程中，组合允许您通过组合小而单一的函数来构建更复杂的功能。

例如，考虑使用 `map()` 从初始集合创建一个新数组，然后使用 `filter()` 过滤结果：

```jsx
const list = ['Express', 'Koa', 'Egg']
list.map((item) => item).filter((item) => item === 'E') // [ 'E', 'E' ]
```

在 React 中，组合让您拥有一些非常酷的优势。

您可以创建小而精简的组件，并使用它们在其上构建更多功能。

> 推荐：[JavaScript 组合函数](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E5%90%88%E6%88%90%E5%87%BD%E6%95%B0.md)。

## 创建组件的专用版本

使用外部组件来扩展和专门化更通用的组件：

```jsx
const Button = (props) => {
  return <button>{props.text}</button>
}

const SubmitButton = () => {
  return <Button text='Submit' />
}

const LoginButton = () => {
  return <Button text='Login' />
}
```

## 将方法作为 props 传递

例如，组件可以专注于跟踪点击事件，而当点击事件发生时实际发生的情况取决于容器组件：

```jsx
const Button = (props) => {
  return <button onClick={props.onClickHandler}>{props.text}</button>
}

const LoginButton = (props) => {
  return <Button text='Login' onClickHandler={props.onClickHandler} />
}

const Container = () => {
  const onClickHandler = () => {
    alert('clicked')
  }

  return <LoginButton onClickHandler={onClickHandler} />
}
```

## 使用 children

`props.children` 属性允许您将组件注入其他组件中。

组件需要 `props.children` 在其 JSX 中输出：

```jsx
const Sidebar = (props) => {
  return <aside>{props.children}</aside>
}
```

并且您可以以透明的方式将更多组件嵌入其中：

```jsx
<Sidebar>
  <Link title='First link' />
  <Link title='Second link' />
</Sidebar>
```

> **扩展**：如果您使用过 Vue，那么它类似于其中的[插槽](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E6%8F%92%E6%A7%BD.md)的概念。

## 高阶组件

当一个组件接收一个组件作为 props 并返回一个组件时，它被称为高阶组件。

您可以在我的另一篇文章 [React 高阶函数](https://github.com/lio-zero/blog/blob/main/React/React%20%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6.md)中了解更多关于高阶组件的信息。
