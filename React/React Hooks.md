# React Hooks

Hooks 在 16.8 版本中被添加到 React，允许函数组件访问状态和其他 React 特性。因此，通常不再需要类组件。

尽管 Hooks 通常会替换类组件，但没有计划从 React 中删除类。

## 什么是 Hooks？

Hooks 允许我们“挂钩”到 React 特性，例如状态和生命周期方法。

```js
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

function FavoriteColor() {
  const [color, setColor] = useState('red')

  return (
    <>
      <h1>我最喜欢的颜色是 {color}!</h1>
      <button type='button' onClick={() => setColor('blue')}>
        Blue
      </button>
      <button type='button' onClick={() => setColor('red')}>
        Red
      </button>
      <button type='button' onClick={() => setColor('pink')}>
        Pink
      </button>
      <button type='button' onClick={() => setColor('green')}>
        Green
      </button>
    </>
  )
}

ReactDOM.render(<FavoriteColor />, document.getElementById('root'))
```

使用 React 提供的钩子前，我们需要从 `react` 中 `import` 钩子。

这里我们使用 `useState` 钩子来跟踪应用程序状态。状态通常指需要跟踪的应用程序数据或属性。

## 挂钩规则

钩子有 3 条规则：

- Hooks 只能在 React 函数组件内部调用。
- Hooks 只能在组件的顶层调用。
- Hooks 不能是有条件的

> **注意**：钩子在 React 类组件中不起作用。

下面我们来看看 React 提供的一些 Hooks。

## `useState` Hooks

React `useState` 钩子允许我们跟踪函数组件中的状态。状态通常指应用程序中需要跟踪的数据或属性。

### 导入 `useState`

要使用 `useState` 钩子，我们首先需要将 `import` 它到我们的组件中。

```js
import { useState } from 'react'
```

我们从 react 中解构 `useState`，因为它是一个命名导出。

### 初始化 `useState`

我们通过在函数组件中调用 `useState` 来初始化状态。

`useState` 接受初始状态并返回两个值：

- 当前状态。
- 更新状态的函数。

```js
import { useState } from 'react'

function FavoriteColor() {
  const [color, setColor] = useState('')
}
```

第一个值 `color` 是我们当前的状态。第二个值 `setColor` 是用于更新状态的函数。这些名称是变量，可以任意命名。

最后，我们将初始状态设置为空字符串：`useState('')`

### 读取状态

我们现在可以在组件中的任何位置包含我们的状态。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function FavoriteColor() {
  const [color, setColor] = useState('red')

  return <h1>我最喜欢的颜色是 {color}!</h1>
}

ReactDOM.render(<FavoriteColor />, document.getElementById('root'))
```

### 更新状态

为了更新我们的状态，我们使用定义好的 `setColor` 状态更新程序函数。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function FavoriteColor() {
  const [color, setColor] = useState('red')

  return (
    <>
      <h1>我最喜欢的颜色是 {color}!</h1>
      <button type='button' onClick={() => setColor('blue')}>
        Blue
      </button>
    </>
  )
}

ReactDOM.render(<FavoriteColor />, document.getElementById('root'))
```

> **注意**：我们不应该直接更新状态。例如：不允许使用 `color="red"`。

### 状态可以持有什么

`useState` 钩子可以用来跟踪字符串、数字、布尔值、数组、对象以及它们的任意组合！

我们可以创建多个状态 Hook 来跟踪单个值。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function User() {
  const [name, setName] = useState('O.O')
  const [age, setAge] = useState(20)
  const [year, setYear] = useState(1998)

  return (
    <>
      <h1>个人信息</h1>
      <p>
        我叫{name}，今年{age}岁，生于{year}年。
      </p>
    </>
  )
}

ReactDOM.render(<User />, document.getElementById('root'))
```

或者，我们可以只使用一个状态并包含一个对象！

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function User() {
  const [user, setUser] = useState({
    name: 'O.O',
    age: 20,
    year: 1998
  })

  return (
    <>
      <h1>个人信息</h1>
      <p>
        我叫{user.name}，今年{user.age}岁，生于{user.year}年。
      </p>
    </>
  )
}

ReactDOM.render(<User />, document.getElementById('root'))
```

由于我们现在正在跟踪单个对象，因此在渲染组件时需要引用该对象，然后引用该对象的属性。（例如：`user.name`）

### 更新状态中的对象和数组

当状态更新时，整个状态都会被覆盖。

如果我们只想更新用户的年龄呢？

如果我们只调用 `setUser({ age: 18 })`，这将从我们的状态中删除 `name` 和 `year`。

我们可以使用 JavaScript 扩展运算符来帮助我们。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function User() {
  const [user, setUser] = useState({
    name: 'O.O',
    age: 20,
    year: 1998
  })

  const updateUser = () => {
    setUser((previousState) => {
      return { ...previousState, age: 18 }
    })
  }

  return (
    <>
      <h1>个人信息</h1>
      <p>
        我叫{user.name}，今年{user.age}岁，生于{user.year}年。
      </p>
      <button onClick={updateUser}>18</button>
    </>
  )
}

ReactDOM.render(<User />, document.getElementById('root'))
```

因为我们需要状态的当前值，所以我们将一个函数传递给 `setUser` 函数。此函数接收上一个值。

然后我们返回一个对象，展开 `previousState` 并仅覆盖 `age`。

## `useEffect` Hooks

`useEffect` Hook 允许您在组件中执行副作用。副作用的一些示例如：获取数据、直接更新 DOM 和定时器。

`useEffect` 接受两个参数。第二个参数是可选的。

以定时器为例，使用 `setTimeout()` 计算初始渲染后的 1 秒：

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'

function Timer() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    setTimeout(() => {
      setCount((count) => count + 1)
    }, 1000)
  })

  return <h1>我已经渲染了 {count} 次！</h1>
}

ReactDOM.render(<Timer />, document.getElementById('root'))
```

可是等等！！它一直在计数，即使它应该只计数一次！

`useEffect` 在每个渲染上运行。这意味着当计数发生变化时，会发生渲染，然后触发另一个效果。

这不是我们想要的。有几种方法可以控制副作用何时运行。

我们应该始终包含接受数组的第二个参数。我们可以选择将依赖项传递给该数组中的 `useEffect`。

没有任何依赖：

```js
useEffect(() => {
  // 在每个渲染上运行
})
```

一个空数组：

```js
useEffect(() => {
  // 仅在第一次渲染时运行
}, [])
```

`prop` 或 `state` 值：

```js
useEffect(() => {
  // 在第一次渲染和任何依赖项值更改时运行
}, [prop, state])
```

所以，为了解决这个问题，让我们只在初始渲染上运行这个效果。

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'

function Timer() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    setTimeout(() => {
      setCount((count) => count + 1)
    }, 1000)
  }, []) // <- 在此处添加空括号

  return <h1>我渲染了 {count} 次!</h1>
}

ReactDOM.render(<Timer />, document.getElementById('root'))
```

下面是一个依赖于变量的 `useEffect` 钩子的示例。如果 `count` 变量更新，效果将再次运行：

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'

function Counter() {
  const [count, setCount] = useState(0)
  const [calculation, setCalculation] = useState(0)

  useEffect(() => {
    setCalculation(() => count * 2)
  }, [count]) // <- 在这里添加 count 变量

  return (
    <>
      <p>总数: {count}</p>
      <button onClick={() => setCount((c) => c + 1)}>+</button>
      <p>计算: {calculation}</p>
    </>
  )
}

ReactDOM.render(<Counter />, document.getElementById('root'))
```

如果存在多个依赖项，则应将它们包含在 `useEffect` 依赖项数组中。

### 清理 Effect

有些效果需要清理以减少内存泄漏。

超时、订阅、事件监听器和其他不再需要的效果应该被处理。

我们通过在 `useEffect` 钩子的末尾包含一个返回函数来实现这一点。

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'

function Timer() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    let timer = setTimeout(() => {
      setCount((count) => count + 1)
    }, 1000)

    return () => clearTimeout(timer)
  }, [])

  return <h1>我渲染了 {count} 次!</h1>
}

ReactDOM.render(<Timer />, document.getElementById('root'))
```

> **注意**：要清除定时器，我们必须为其命名。

## `useContext` Hooks

React Context 是一种全局管理状态的方法。

与单独使用 `useState` 相比，它可以与 `useState` 钩子一起使用，在深度嵌套的组件之间更容易地共享状态。

### 问题

状态应由堆栈中需要访问状态的最高父组件持有。

举例来说，我们有许多嵌套组件。堆栈顶部和底部的组件需要访问状态。

要在没有上下文的情况下实现这一点，我们需要将状态作为 `props` 传递给每个嵌套组件。这被称为 **prop drilling**。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

function Comp1() {
  const [user, setUser] = useState('O.O')

  return (
    <>
      <h1>{`Hello ${user}!`}</h1>
      <Comp2 user={user} />
    </>
  )
}

function Comp2({ user }) {
  return (
    <>
      <h1>组件 2</h1>
      <Comp3 user={user} />
    </>
  )
}

function Comp3({ user }) {
  return (
    <>
      <h1>组件 3</h1>
      <Comp4 user={user} />
    </>
  )
}

function Comp4({ user }) {
  return (
    <>
      <h1>组件 4</h1>
      <Comp5 user={user} />
    </>
  )
}

function Comp5({ user }) {
  return (
    <>
      <h1>组件 5</h1>
      <h2>{`Hello ${user} again!`}</h2>
    </>
  )
}

ReactDOM.render(<Comp1 />, document.getElementById('root'))
```

即使组件 2-4 不需要状态，但它们也必须传递状态才能到达组件 5。

### 解决方案

解决方案是创建上下文。

要创建上下文，必须导入 `createContext` 并对其进行初始化：

```js
import { useState, createContext } from 'react'
import ReactDOM from 'react-dom'

const UserContext = createContext()
```

接下来，我们将使用 Context Provider 来包装需要状态上下文的组件树。

### Context Provider

在 Context Provider 中包装子组件并提供状态值。

```js
function Comp1() {
  const [user, setUser] = useState('O.O')

  return (
    <UserContext.Provider value={user}>
      <h1>{`Hello ${user}!`}</h1>
      <Comp2 user={user} />
    </UserContext.Provider>
  )
}
```

现在，该树中的所有组件都可以访问用户上下文。

### 使用 `useContext` Hooks

为了在子组件中使用上下文，我们需要使用 `useContext` 钩子访问它。

首先，导入 `useContext`：

```js
import { useState, createContext, useContext } from 'react'
```

然后，您可以访问所有组件中的用户上下文：

```js
function Comp5() {
  const user = useContext(UserContext)

  return (
    <>
      <h1>组件 5</h1>
      <h2>{`Hello ${user} again!`}</h2>
    </>
  )
}
```

完整示例：

```js
import { useState, createContext, useContext } from 'react'
import ReactDOM from 'react-dom'

const UserContext = createContext()

function Component1() {
  const [user, setUser] = useState('O.O')

  return (
    <UserContext.Provider value={user}>
      <h1>{`Hello ${user}!`}</h1>
      <Comp2 user={user} />
    </UserContext.Provider>
  )
}

function Comp2() {
  return (
    <>
      <h1>组件 2</h1>
      <Comp3 />
    </>
  )
}

function Comp3() {
  return (
    <>
      <h1>组件 3</h1>
      <Comp4 />
    </>
  )
}

function Comp4() {
  return (
    <>
      <h1>组件 4</h1>
      <Comp5 />
    </>
  )
}

function Comp5() {
  const user = useContext(UserContext)

  return (
    <>
      <h1>组件 5</h1>
      <h2>{`Hello ${user} again!`}</h2>
    </>
  )
}

ReactDOM.render(<Component1 />, document.getElementById('root'))
```

## `useRef` Hooks

`useRef` 钩子允许在渲染之间持久化值。

它可用于存储在更新时不会导致重新渲染的可变值，也可用于直接访问 DOM 元素。

### 不会导致重新渲染

如果我们试图计算应用程序使用 `useState` 钩子渲染的次数，我们将陷入无限循环，因为这个钩子本身会导致重新渲染。

为了避免这种情况，我们可以使用 `useRef` 钩子。

```js
import { useState, useEffect, useRef } from 'react'
import ReactDOM from 'react-dom'

function App() {
  const [inputValue, setInputValue] = useState('')
  const count = useRef(0)

  useEffect(() => {
    count.current = count.current + 1
  })

  return (
    <>
      <input
        type='text'
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <h1>渲染次数: {count.current}</h1>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

`useRef()` 只返回一项。它返回一个名为 `current` 的对象。

初始化 `useRef` 时，我们设置初始值为 `useRef(0)`。它其实类似于 `const count= { current:0 }`，我们可以使用 `count` 访问 `count.current`。

### 访问 DOM 元素

通常，我们希望让 React 处理所有 DOM 操作。

但在某些情况下，可以使用 `useRef` 而不会引起问题。

在 React 中，我们可以向元素添加 `ref` 属性，以便直接在 DOM 中访问它。

```js
import { useRef } from 'react'
import ReactDOM from 'react-dom'

function App() {
  const inputElement = useRef()

  const focusInput = () => inputElement.current.focus()

  return (
    <>
      <input type='text' ref={inputElement} />
      <button onClick={focusInput}>聚焦 input</button>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

### 跟踪状态变化

`useRef` 钩子还可用于跟踪先前的状态值。

这是因为我们能够在渲染之间持久化 `useRef` 值。

```js
import { useState, useEffect, useRef } from 'react'
import ReactDOM from 'react-dom'

function App() {
  const [inputValue, setInputValue] = useState('')
  const previousInputValue = useRef('')

  useEffect(() => {
    previousInputValue.current = inputValue
  }, [inputValue])

  return (
    <>
      <input
        type='text'
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <h2>当前值: {inputValue}</h2>
      <h2>先前值: {previousInputValue.current}</h2>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

这一次，我们结合使用 `useState`、`useffect` 和 `useRef` 来跟踪之前的状态。

在 `useEffect` 中，每次通过在 `input` 字段中输入文本来更新 `inputValue` 时，我们都会更新 `useRef` 当前值。

## `useReducer` Hooks

`useReducer` 钩子类似于 `useState` 钩子。它允许自定义状态逻辑。

如果您发现自己在跟踪依赖于复杂逻辑的多个状态，`useReducer` 可能会很有用。

`useReducer` 钩子接受两个参数：

- `reducer`函数包含自定义状态逻辑，`initialState` 可以是一个简单的值，但通常会包含一个对象。
- `useReducer` 钩子返回当前 `state` 和 `dispatch` 方法。

下面是计数器使用 `useReducer` 的示例：

```js
import { useReducer } from 'react'
import ReactDOM from 'react-dom'

const initialTodos = [
  {
    id: 1,
    title: 'Todo 1',
    complete: false
  },
  {
    id: 2,
    title: 'Todo 2',
    complete: false
  }
]

const reducer = (state, action) => {
  switch (action.type) {
    case 'COMPLETE':
      return state.map((todo) => {
        if (todo.id === action.id) {
          return { ...todo, complete: !todo.complete }
        } else {
          return todo
        }
      })
    default:
      return state
  }
}

function Todos() {
  const [todos, dispatch] = useReducer(reducer, initialTodos)

  const handleComplete = (todo) => {
    dispatch({ type: 'COMPLETE', id: todo.id })
  }

  return (
    <>
      {todos.map((todo) => (
        <div key={todo.id}>
          <label>
            <input
              type='checkbox'
              checked={todo.complete}
              onChange={() => handleComplete(todo)}
            />
            {todo.title}
          </label>
        </div>
      ))}
    </>
  )
}

ReactDOM.render(<Todos />, document.getElementById('root'))
```

这就是跟踪 todo 完成状态的逻辑。

通过添加更多操作，添加、删除和完成 todo 的所有逻辑都可以包含在单个 `useReducer` 钩子中。

## `useCallback` Hooks

React `useCallback` Hook 返回一个已记忆的回调函数。

这使我们能够隔离资源密集型函数，以便它们不会在每次渲染时自动运行。

`useCallbackHook` 仅在其依赖项之一更新时运行，提高了性能。

### 问题

使用 `useCallback` 的一个原因是防止组件重新渲染，除非其 `props` 已更改。

在本例中，您可能会认为 todos 组件不会重新渲染，除非 todos 发生更改：

```js
// main.js
import { useState } from 'react'
import ReactDOM from 'react-dom'
import Todos from './Todos'

const App = () => {
  const [count, setCount] = useState(0)
  const [todos, setTodos] = useState([])

  const increment = () => {
    setCount((c) => c + 1)
  }
  const addTodo = () => {
    setTodos((t) => [...t, 'New Todo'])
  }

  return (
    <>
      <Todos todos={todos} addTodo={addTodo} />
      <hr />
      <div>
        次数: {count}
        <button onClick={increment}>+</button>
      </div>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

`Todos.js` 组件:

```js
import { memo } from 'react'

const Todos = ({ todos, addTodo }) => {
  console.log('子渲染')
  return (
    <>
      <h2>Todos List</h2>
      {todos.map((todo, index) => {
        return <p key={index}>{todo}</p>
      })}
      <button onClick={addTodo}>添加 Todo</button>
    </>
  )
}

export default memo(Todos)
```

尝试运行它并单击 `+` 按钮。

您会注意到，即使 `todos` 没有更改，`Todos` 组件也会重新渲染。

为什么这不起作用？我们使用的是 `memo`，所以 `Todos` 组件不应该重新渲染，因为当 `count `增加时，`todos` 状态和 `addTodo` 函数都没有改变。

这是因为所谓的“参照平等”。

每次组件重新渲染时，都会重新创建其函数。因此，`addTodo` 函数实际上发生了变化。

### 解决方案

为了解决这个问题，我们可以使用 `useCallback` 钩子来防止函数被重新创建，除非有必要。

使用 `useCallback` 钩子可以防止 `Todos` 组件不必要地重新渲染：

```js
import { useState, useCallback } from 'react'
import ReactDOM from 'react-dom'
import Todos from './Todos'

const App = () => {
  const [count, setCount] = useState(0)
  const [todos, setTodos] = useState([])

  const increment = () => {
    setCount((c) => c + 1)
  }
  const addTodo = useCallback(() => {
    setTodos((t) => [...t, 'New Todo'])
  }, [todos])

  return (
    <>
      <Todos todos={todos} addTodo={addTodo} />
      <hr />
      <div>
        次数: {count}
        <button onClick={increment}>+</button>
      </div>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

`Todos.js` 组件:

```js
import { memo } from 'react'

const Todos = ({ todos, addTodo }) => {
  console.log('子渲染')
  return (
    <>
      <h2>Todos List</h2>
      {todos.map((todo, index) => {
        return <p key={index}>{todo}</p>
      })}
      <button onClick={addTodo}>新增 Todo</button>
    </>
  )
}

export default memo(Todos)
```

## `useMemo` Hooks

React `useMemo` 钩子返回一个已记忆的值，它仅在其中一个依赖项更新时运行，提高性能。

`useMemo` 和 `useCallback` 钩子类似。主要区别在于 `useMemo` 返回一个已记忆的值， `useCallback` 返回一个已记忆的函数。

### 性能

`useMemo` 钩子可以用来防止昂贵的、资源密集型的函数不必要地运行。

在本例中，我们有一个在每个渲染上运行的昂贵函数。

更改计数或添加 `todo` 时，您会注意到执行延迟。

```js
import { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [count, setCount] = useState(0)
  const [todos, setTodos] = useState([])
  const calculation = expensiveCalculation(count)

  const increment = () => {
    setCount((c) => c + 1)
  }
  const addTodo = () => {
    setTodos((t) => [...t, 'New Todo'])
  }

  return (
    <div>
      <div>
        <h2>Todos List</h2>
        {todos.map((todo, index) => {
          return <p key={index}>{todo}</p>
        })}
        <button onClick={addTodo}>新增 Todo</button>
      </div>
      <hr />
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
        <h2>昂贵的计算</h2>
        {calculation}
      </div>
    </div>
  )
}

const expensiveCalculation = (num) => {
  console.log('计算...')
  for (let i = 0; i < 1000000000; i++) {
    num += 1
  }
  return num
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## 使用 `useMemo`

为了解决这个性能问题，我们可以使用 `useMemoHook` 来记忆 `expensiveCalculation` 函数。这将导致该函数仅在需要时运行。

`useMemoHook` 接受第二个参数来声明依赖项。昂贵的函数只会在其依赖关系发生变化时运行。

在下面的示例中，昂贵的函数只会在 `count` 更改时运行，而不是在添加待办事项时运行。

```js
import { useState, useMemo } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const [count, setCount] = useState(0)
  const [todos, setTodos] = useState([])
  const calculation = useMemo(() => expensiveCalculation(count), [count])

  const increment = () => {
    setCount((c) => c + 1)
  }
  const addTodo = () => {
    setTodos((t) => [...t, 'New Todo'])
  }

  return (
    <div>
      <div>
        <h2>Todos List</h2>
        {todos.map((todo, index) => {
          return <p key={index}>{todo}</p>
        })}
        <button onClick={addTodo}>新增 Todo</button>
      </div>
      <hr />
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
        <h2>昂贵的计算</h2>
        {calculation}
      </div>
    </div>
  )
}

const expensiveCalculation = (num) => {
  console.log('计算...')
  for (let i = 0; i < 1000000000; i++) {
    num += 1
  }
  return num
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## 自定义 Hooks

Hooks 是可重用的函数。

当您有需要在多个组件中使用相同的组件逻辑时，我们可以将该逻辑提取到自定义 Hook。

自定义 Hooks 以 `use` 开头。本节将编写一个 `useFetch` 示例。

### 自定义 `useFetch` 钩子

在下面的代码中，我们在 `Home` 组件中获取数据并显示它。

我们将使用 **[JSONPlaceholder](https://jsonplaceholder.typicode.com/)** 服务来获取假数据。

使用 **JSONPlaceholder** 服务获取假 `Todos` 列表，并在页面上显示标题：

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'

const Home = () => {
  const [data, setData] = useState(null)

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/todos')
      .then((res) => res.json())
      .then((data) => setData(data))
  }, [])

  return (
    <>
      {data &&
        data.map((item) => {
          return <p key={item.id}>{item.title}</p>
        })}
    </>
  )
}

ReactDOM.render(<Home />, document.getElementById('root'))
```

其他组件也可能需要获取逻辑，因此我们将其提取到自定义 Hook 中。

将获取逻辑移动到一个新文件以用作自定义 Hook：

```js
// useFetch.js
import { useState, useEffect } from 'react'

const useFetch = (url) => {
  const [data, setData] = useState(null)

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => setData(data))
  }, [url])

  return [data]
}

export default useFetch
```

```js
// main.js
import ReactDOM from 'react-dom'
import useFetch from './useFetch'

const Home = () => {
  const [data] = useFetch('https://jsonplaceholder.typicode.com/todos')

  return (
    <>
      {data &&
        data.map((item) => {
          return <p key={item.id}>{item.title}</p>
        })}
    </>
  )
}

ReactDOM.render(<Home />, document.getElementById('root'))
```

我们创建了一个名为 `useFetch.js` 的新文件，其中包含一个名为 `useFetch` 的函数，该函数包含获取数据所需的所有逻辑。

我们删除了硬编码的 URL，并将其替换为可以传递给自定义钩子的 URL 变量。

最后，我们从钩子中返回数据。

在 `main.js` 中，我们导入 `useFetch` 钩子，并像其他钩子一样使用它。这就是我们传递 URL 以从中获取数据的地方。

现在我们可以在任何组件中重用这个自定义钩子，从任何 URL 获取数据。
