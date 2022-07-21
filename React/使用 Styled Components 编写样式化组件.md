# 使用 Styled Components 编写样式化组件

[Styled Components](https://styled-components.com/) 是最流行的 CSS-in-JS 库，可让您编写常规 CSS 并将其附加到 JavaScript 组件。使用它，您可以使用您已经熟悉的 CSS，而不必学习新的样式结构。

本文将介绍 Styled Components 的一些功能，并提供一些示例帮助您理解。

## styles 继承

我们可以通过简单地将样式组件传递给 `styled` 函数来继承样式组件的样式。

```js
import styled from 'styled-components'

const Div = styled.div`
  padding: 10px;
  color: palevioletred;
`
```

这里我们有一个 `Div` 样式的组件。让我们创建另一个 `div` 元素来继承该组件的样式。

```js
const InheritedDiv = styled(Div)`
  border: 1px solid palevioletred;
`
```

`InheritedDiv` 将具有 `Div` 组件的样式以及它自己的样式。

```css
padding: 10px;
color: palevioletred;
border: 1px solid palevioletred;
```

## 传递 props

`props` 可以传递给样式化组件，就像它们与常规 React 组件（类或函数）一样。这是可能的，因为样式化组件实际上是 React 组件。

```js
const Button = styled.button`
  padding: 2px 5px;
  color: white;
  border-radius: 3px;
`

const Div = styled.div`
  padding: 10px;
  color: palevioletred;
  border: 1px solid palevioletred;
`
```

styled-components 创建了一个 React 组件，该组件渲染与 `styled` 对象中的属性相对应的 HTML 标签。

`Button` 将创建并渲染 `button` HTML 标签，而 `Div` 将创建并渲染 `div` 标签。它们是组件，所以我们可以向它们传递 `props`。

```html
<Button color="black">Click Me</Button>
<Div borderColor="green"></Div>
```

这将使样式化组件将包含 `color` 的 `props` 传递给 `Button` 组件，并将包含 `borderColor` 的 `props` 传递给 `Div` 组件。然后，我们可以使用一个函数在标签的模板字符串中获取 `props`。

```js
const Button = styled.button`
  padding: 2px 5px;
  color: ${(props) => (props.color ? props.color : 'white')};
  border-radius: 3px;
`

const Div = styled.div`
  padding: 10px;
  color: palevioletred;
  border: 1px solid ${(props) =>
      props.borderColor ? props.borderColor : 'palevioletred'};
`
```

标签模板字符串中的函数将接收一个 `props` 参数，它是传递给组件的 `props`。这使我们能够引用传递给 `Button` 和 `Div` 组件的 `color` 和 `borderColor`，从而使样式化组件的样式具有动态性。

## 主题化

styled-components 提供主题化功能，使您能够支持多种外观。

为此，我们将使用 `ThemeProvider` 组件。

```js
import { ThemeProvider } from 'styled-components'
```

让我们设置一个主题对象来保存我们想要应用于样式化组件的 CSS 样式。

```js
const theme = {
  boderColor: 'plum',
  color: 'plum',
  bgColor: 'plum'
}
```

主题对象保存 `border-color`、`color` 和 `bgColor` 的颜色。

现在，我们有两个组件：`Button` 和 `Div`。让我们使用它们的主题。

```js
const Button = styled.button`
  padding: 2px 5px;
  color: ${(props) => props.theme.color};
  border-radius: 3px;
`

const Div = styled.div`
  padding: 10px;
  color: ${(props) => props.theme.color};
  border: 1px solid ${(props) => props.theme.borderColor};
`
```

如您所见，他们正在访问 `props` 中的主题属性。`ThemeProvider` 将主题对象作为 `props` 传递给组件。

最后，我们将渲染 `ThemeProvider` 标签之间的 `Div` 和 `Button` 组件，并将主题对象传递给 `ThemeProvider` 中的主题 `props`。

```html
<ThemeProvider theme="{theme}">
  <Div>
    <Button>Click Me</Button>
  </Div>
</ThemeProvider>
```

`theme` 对象将在他们的 `props` 中提供给 `ThemeProvider` 的孩子。

现在，如果我们更改主题对象中的任何属性值，那么 `ThemeProvider` 将把更改传递给子对象，`Div` 和 `Button` 将相应地更改它们的样式。

## 全局样式

到目前为止，我们所做的大部分样式都是特定于组件的。styled-components 框架还允许您创建应用于所有样式化组件的全局样式。

创建一个 `globalStyles.js` 文件，导入 `createGlobalStyle` 并编写全局样式。

```js
import { createGlobalStyle } from 'styled-components'

export default createGlobalStyle`
  html {
    margin: 0;
  }

  body {
    margin: 0;
  }
`
```

接下来，将其导入 `main` 组件中。

```js
import GlobalStyle from './globalStyles'

function Home() {
  return (
    <div>
      <GlobalStyle />
      {/* 其他组件 */}
    </div>
  )
}
```

`GlobalStyle` 会在其余组件之前被渲染。这将在 `GlobalStyle` 之后的所有组件应用全局样式。

## 切换组件类型

样式化组件本质上是动态的。它们可以从创建和渲染一个 HTML 元素更改为另一个。

```js
const Button = styled.button`
  padding: 2px 5px;
  color: ${(props) => props.theme.color};
  border-radius: 3px;
`
```

`Button` 组件将创建并渲染一个按钮元素。在渲染按钮组件时，我们可以通过将 `as` props 传递给 `Button` 组件，并使用我们希望它更改为的任何 HTML 标签名来更改它。

```html
<Button as="a">Click Me</Button>
```

这将创建并渲染 `a` 标签。`as="a"` 将其从渲染 `Button` 元素更改为呈现 `a` 标签。

这也可以使用 `withComponent` 方法完成。

```js
const Button = styled.button`
  padding: 2px 5px;
  color: palevioletred;
  border-radius: 3px;
`

const Link = Button.withComponent('a')
```

`Link` 是一个样式化的组件，它将渲染 `a` 标签，并应用 `Button` 的 CSS 样式。

## 样式化常规组件

通过使用组件调用 `styled()` 方法，然后使用包含样式代码的模板祝福词，可以将常规组件转换为样式化组件。

```js
function Button(props) {
  return <button className={props.className}>{props.children}</button>
}
```

这里我们有一个 `Button` 组件，它渲染一个按钮元素。请注意，我们为按钮元素设置了一个 `className` 属性，并将其值分配给 `props.className`。因此继承的样式将应用于按钮元素。

要将此组件转换为样式化组件，请将其传递给 `styled()` 方法。

```js
Button = styled(Button)`
  padding: 2px 5px;
  border-radius: 3px;
  border: 1px solid palevioletred;
`
```

这将使用模板字符串中的 CSS 样式设置 `Button` 组件中的按钮元素的样式。`Button` 将使用以下 CSS 代码渲染按钮元素。

```css
padding: 2px 5px;
border-radius: 3px;
border: 1px solid palevioletred;
```

## 指定属性

您可以向样式化组件渲染的 HTML 元素添加属性。

例如，可以创建如下 `Input` 组件：

```js
const Input = styled.input`
  font-size: 14px;
  padding: 2px 5px;
  border: 1px solid green;
`
```

`Input` 将渲染和输入元素。输入元素有不同的类型，包括：

- `text`
- `number`
- `password`
- `email`

这些是在输入元素中使用 `type`属性指定的。要告诉 styled-component 您想要的输入元素类型，请使用 `attrs` 方法。

```js
const Input = styled.input.attrs({
  type: 'text'
})`
  font-size: 14px;
  padding: 2px 5px;
  border: 1px solid green;
`
```

这将创建一个 `text` 类型的输入元素。我们还可以向样式化组件添加其他属性。

```js
const Input = styled.input.attrs({
  type: 'text',
  placeholder: '在此处输入账号'
})`
  font-size: 14px;
  padding: 2px 5px;
  border: 1px solid plum;
`

const PasswordInput = styled.input.attrs({
  type: 'password',
  placeholder: '在此处输入密码'
})`
  font-size: 14px;
  padding: 2px 5px;
  border: 1px solid plum;
`
```

## 与其他 CSS 框架一起使用

最后，您可以将 styled-components 用于任何 CSS 框架。

例如，让我们创建一个具有 Bootstrap 样式的 `Button` 组件。

```js
const PrimaryButton = styled.button.attrs({
  className: 'btn btn-prmiary'
})`
  outline: none;
`
```

我们使用 `attrs` 方法向具有 `btn`、`btn-primary` 值的组件添加 `className` 属性。这将使 Bootstrap 将其样式应用于组件。

其他 CSS 框架也是如此。

```js
const MatButton = styled.button.attrs({
  className: 'mat-button'
})`
  outline: none;
`
```

上面的例子是一个 Material Design 风格的组件。

## 供应商前缀

Styled Components 会自动添加所有需要的供应商前缀，因此您无需担心这个问题。

## 更多资料

[The styled-components Happy Path](https://www.joshwcomeau.com/css/styled-components/)
