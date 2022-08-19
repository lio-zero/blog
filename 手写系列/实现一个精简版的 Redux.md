# 实现一个精简版的 Redux

## Redux 的作用？

在开始之前，有必要了解一下 Redux 的作用。

[Redux](https://redux.js.org/) 是一个状态管理库（与 [Vuex](https://vuex.vuejs.org/index.html) 和 [Pinia](https://pinia.vuejs.org/) 类似）。它可以帮助您管理应用程序中的有状态信息。“有状态信息”只是一种花哨的说法，表示在应用程序使用期间需要持久保存并可用的信息。例如：用户名或应用程序是处于 `light` 模式还是 `dark` 模式等。（下面我们也会用到这个示例）

当您需要管理大型项目时，像 Redux 这样的状态管理库变得特别有用。许多人认为 Redux 是 React 的一部分或与 React 明确关联，但实际上它是自己的独立库，可以与 React 一起使用，也可以不使用 React。

## Redux 的基本原理

Redux 背后的基本思想是，您对状态信息有一个集中管理的位置，并且可以预测地更新状态。为此，Redux 具有以下基本结构：

- **state 对象** - `state` 对象包含应用程序的有状态信息。
- **action** - `action` 是为 Redux 提供更新状态所需信息的对象。按照约定，一个 `action` 对象可以具有 `type` 和 `payload` 属性。例如：如果您想将用户名设置为 `O.O`，则操作可能是 `{ action: 'SET_USER_NAME', payload: 'O.O' }`。
- **reducer** - `reducer` 是一个函数。它可以传入两个参数：**当前状态 `state` 对象和 `action` 对象**（如上所述）。reducer 使用 `action` 对象中提供的信息以及状态的当前版本，并返回状态的新版本。
- **store** - `store` 是一个对象，具有两个属性，且两者都是函数：`getState` 和 `dispatch`。`getState` 允许您访问最新的状态，`dispatch` 允许您调度 `action` 以更新该状态。

记住它们，下面我们会用到！

## React 最大的问题

Redux 最大的批评之一是它的学习曲线陡峭。但也不必过于担心，相信通过本文实现自己精简版的 Redux 后，您对上面提到的这几个概念和 Redux 的工作方式都会有一定程度的了解。

## 实现自己的 Redux

如果您使用过 Redux，那么您会知道通常使用库提供的 `createStore` 函数来创建 `store`。我们需要自己写一个！

如上所述，我们的 `store` 允许我们使用 `getState` 函数访问 `state` 对象。它还允许我们调度 `action` 更新状态。让我们基于这些知识创建一个 `createStore` 函数。

```js
function createStore() {
  let state = {}
  function getState() {
    return state
  }

  function dispatch(action) {
    // 基于 action 设置状态
  }

  return { getState, dispatch }
}
```

这是一个很好的开始！让我们做一些改进。首先，我们并不总是希望初始 `state` 为空对象 `{}`。相反，我们将让 `createStore` 接受一个名为 `initialState`（初始状态）的参数。

接下来，我们的 `dispatch` 函数必须对与我们传递给它的 `action` 关联，以便更新我们的状态。如上所述，`reducer` 满足此需求：

> `reducer` 是一个函数。它可以传入两个参数：**当前状态 `state` 对象和 `action` 对象**。reducer 使用 `action` 对象中提供的信息以及状态的当前版本，并返回状态的新版本。

因此，让我们将当前 `state` 对象与 `action` 一起传递给 `reducer`，并赋值给 `state` 变量。

以下是我们实现的两项改进：

```js
function createStore(reducer, initialState) {
  let state = initialState
  function getState() {
    return state
  }

  function dispatch(action) {
    state = reducer(state, action)
  }

  return { getState, dispatch }
}
```

这就是我们简化的 `createStore` 函数！更有经验的 Redux 用户可能会注意到，我们从 `createStore` 中省略了第三个参数。当您进入更高级的 Redux 时，这个参数变得很重要，但对于核心原则，我们仅使用前两个参数！

在使用 `createStore` 函数之前，我们需要一个 `reducer`。

这里我们创建一个 `reducer`，它可以设置用户名或亮/暗模式。

正如我们所讨论的，我们的 `reducer` 函数将当前 `state` 和 `action` 作为参数，并返回最新的 `state`。

```js
function reducer(state, action) {
  switch (action.type) {
    case 'SET_USER_NAME':
      return {
        ...state,
        name: action.payload
      }
    case 'SET_DISPLAY_MODE':
      return {
        ...state,
        displayMode: action.payload
      }
    default:
      return state
  }
}
```

让我们剖析一下我们在这里所做的事情。

我们的 `reducer` 接受 `state` 和 `action` 参数。我们有一个 `switch` 语句，它将根据 `action.type` 的值返回不同的内容（请记住，按照我们之前的讨论，我们的 `action` 对象具有 `type` 和 `payload`）。

如果 `action.type` 是 `'SET_USER_NAME'`，则返回状态的副本，并用提供的 `action.payload` 覆盖状态的 `name` 键。相反，如果 `action.type` 为 `'SET_DISPLAY_MODE'`，则返回状态的副本，但覆盖 `displayMode` 键。如果 `action.type` 不是这两个字符串之一，我们将返回未修改的状态。

> 可以将 reducer 看做一个状态机，**其中当前状态和分派操作的组合决定了是否实际计算了一个新的状态值**，而不是无条件地只计算了操作本身。这里涉及到了[有限状态机](https://en.wikipedia.org/wiki/Finite-state_machine)（Finite-state_machine），这里不详细介绍。如果您感兴趣，推荐阅读阮一峰老师的 [JavaScript 与有限状态机](https://www.ruanyifeng.com/blog/2013/09/finite-state_machine_for_javascript.html)。

这几乎就是我们所需要的，当然，这不是 redux 的全部，它还有 `Provider`、`connect` 等。

我们现在可以使用我们实现的 Redux 进行测试！

```js
function createStore(reducer, initialState) {
  let state = initialState
  function getState() {
    return state
  }

  function dispatch(action) {
    state = reducer(state, action)
  }

  return { getState, dispatch }
}

function reducer(state, action) {
  switch (action.type) {
    case 'SET_USER_NAME':
      return {
        ...state,
        name: action.payload
      }
    case 'SET_DISPLAY_MODE':
      return {
        ...state,
        displayMode: action.payload
      }
    default:
      return state
  }
}

const initialState = { name: 'Test', displayMode: 'light' }
const store = createStore(reducer, initialState)

console.log(store.getState()) // { name: 'Test', displayMode: 'light' }

store.dispatch({
  type: 'SET_USER_NAME',
  payload: 'O.O'
})

console.log(store.getState()) // { name: 'O.O', displayMode: 'light' }

store.dispatch({
  type: 'SET_DISPLAY_MODE',
  payload: 'dark'
})

console.log(store.getState()) // { name: 'O.O', displayMode: 'dark' }
```

## 最后

现在，我们有了这个 `store` 对象，它可以实现我们想要的一切：

- 我们有一种集中式的方式来访问有状态信息（通过调用 `store.getState()`）
- 通过调度操作（通过调用 `store.dispatch(action)`），我们有了一种可重复、可预测的方式来更新有状态信息。
