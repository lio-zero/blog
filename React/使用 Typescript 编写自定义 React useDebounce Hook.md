# 使用 Typescript 编写自定义 React useDebounce Hook

在 [React Hooks](https://github.com/lio-zero/blog/blob/main/React/React%20Hooks.md) 中，我们介绍了如何使用，以及它的一些常用内置的 React Hooks 钩子，并在最后一节中，我们了解如何自定义钩子。这使我们可以将组件逻辑抽象为可重用的函数。

在本文中，我们将使用 Typescript 编写一个自定义 `useDebounce` 钩子！

关于防抖，您可以看看我之前写的两篇文章：

- [节流和防抖](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E8%8A%82%E6%B5%81%E5%92%8C%E9%98%B2%E6%8A%96.md)
- [如何在用户停止输入 JavaScript 后执行函数](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%E7%94%A8%E6%88%B7%E5%81%9C%E6%AD%A2%E8%BE%93%E5%85%A5%20JavaScript%20%E5%90%8E%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0.md)

## 我们的自定义钩子应该如何工作？

我们可以通过多种方式编写钩子，但让它的用法看起来与 `setState` 钩子的用法相对类似：

```ts
const [value, setValue] = useState<string>('')
```

不同之处在于，我们希望：

- 指定防抖时间
- 同时获取当前值和防抖值

所以它应该看起来像这样：

```ts
const [debouncedValue, value, setValue] = useDebounce<string>('', 1000)
```

然后，我们可以使用 `useEffect` 钩子根据 `debouncedValue` 更改采取一些操作：

```ts
const [debouncedValue, value, setValue] = useDebounce<string>('', 1000)

useEffect(() => {
  // do something
}, [debouncedValue])
```

## `useDebounce` 类型

首先，让我们创建 `useDebounce` 函数，并确保正确定义了其参数和返回类型。我们希望确保钩子采用泛型，为 `initialValue`（初始值）、`debouncedValue`（防抖值）和 `value`（当前/最新值）提供类型。

我们的两个参数 `initialValue` 和 `time` 分别是泛型类型和数字类型。

最后，我们的函数将返回三个元素组成的数组。第一个元素 `debouncedValue` 是泛型类型，第二个元素 `value` 也将是泛型类型，最后一个元素将是我们的设置的 `React.Dispatch` 类型。

```ts
function useDebounce<T>(initialValue: T, time: number): [T, T, React.Dispatch<T>] {}
```

## 编写一个没有防抖的版本

我们的钩子需要保持两种状态：最新/当前值和防抖值。我们将使用 `useState` 钩子来维护这些状态。为了立即制作一个非常简单的版本，我们将使用 `useEffect` 钩子来确定最新值的更改时间，并立即更改防抖值。将来，我们将处理此过程的延迟问题。

```ts
import { useState, useEffect } from 'react'

function useDebounce<T>(initialValue: T, time: number): [T, T, React.Dispatch<T>] {
  const [value, setValue] = useState<T>(initialValue)
  const [debouncedValue, setDebouncedValue] = useState<T>(initialValue)

  useEffect(() => {
    setDebouncedValue(value)
  }, [value])

  return [debouncedValue, value, setValue]
}
```

## 添加防抖函数

接下来可以添加抖动函数了。为此，我们可以在 `useEffect` 中使用 `setTimeout`。这里的关键是从 `useEffect` 返回一个函数来取消超时。这个函数本质上是类组件 `componentWillUnmount` 钩子的等效函数！

```ts
import React, { useState, useEffect } from 'react'

function useDebounce<T>(initialValue: T, time: number): [T, T, React.Dispatch<T>] {
  const [value, setValue] = useState<T>(initialValue)
  const [debouncedValue, setDebouncedValue] = useState<T>(initialValue)

  useEffect(() => {
    const debounce = setTimeout(() => {
      setDebouncedValue(value)
    }, time)
    return () => {
      clearTimeout(debounce)
    }
  }, [value, time])

  return [debouncedValue, value, setValue]
}
```

[演示地址](https://code.juejin.cn/pen/7133521530922729484)

## 最后

对于 React 的 TypeScript 实践，有一个不错的项目 [React+TypeScript Cheatsheets](https://github.com/typescript-cheatsheets/react)，它为经验丰富的 React 开发人员提供 TypeScript 入门的备忘单。
