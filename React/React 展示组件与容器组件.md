# React 展示组件与容器组件

在 React 中，组件通常分为两大类：**展示组件和容器组件**。

每一个都有其独特的特点。

- 展示组件主要涉及生成一些要输出的标签。它不管理任何类型的状态，除了与展示相关的状态
- 容器组件主要关注**后端**操作。它可以处理各种子组件的状态。它们可能包含几个展示组件。它们可能与 Redux 交互。

作为一种简化区分的方法，我们可以说展示组件关注外观，容器组件关注数据交互。

例如，这是一个展示组件。它从 `props` 中获取数据，并且只关注于显示一个元素：

```js
const Users = (props) => (
  <ul>
    {props.users.map((user) => (
      <li key={user.id}>{user.title}</li>
    ))}
  </ul>
)
```

另一方面，这是一个容器组件。它管理和存储自己的数据，并使用展示组件显示数据。

```js
function UsersContainer() {
  const [state, setState] = useState({
    users: []
  })

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then((res) => res.json())
      .then((users) => setState({ users: users }))
  }, [])

  return <Users users={state.users} />
}
```
