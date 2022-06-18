# JavaScript 中的一等函数

JavaScript 的函数是**一等函数**。以下三个点表示重要的一等函数行为：

- 函数可以分配给变量
- 函数可以作为参数传递给其他函数
- 函数可以从其他函数返回

让我们举一些简单的例子来理解这几点。

## 为变量赋值函数

让我们创建一个返回文本 `Hello` 的函数，然后将该函数赋给一个命名为 `sayHello` 的变量。

```js
const sayHello = () => 'Hello'

console.log(sayHello()) // "Hello"
```

## 将函数作为参数传递

让我们采用上述 `sayHello` 函数，并将其作为参数传递给另一个函数。

```js
const sayHelloToPerson = (greeter, person) => greeter() + ' ' + person

console.log(sayHelloToPerson(sayHello, 'IU')) // Hello IU
```

在 `sayHelloToPerson` 函数中，变量 `greeter` 现在将指向内存中的 `sayHello` 函数。当我们调用 `greeter()` 时，函数被调用！

你如果阅读过我之前写过的文章，那么你可能会马上想到[高阶函数](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0.md)！以下给你回忆以下：

> 高阶函数：之所以可以使用 JavaScript 编写高阶函数，是因为函数是值，这意味着它们可以分配给变量并作为值传递。
>
> 当引用作为参数传递的函数时，您可能还会经常听到术语**回调**，因为它是由高阶函数调用的。这在 JavaScript 中尤其常见，事件处理，异步代码和数组操作在很大程度上依赖于回调的概念。

## 从另一个函数返回一个函数

也许我们并不总是想说 `"Hello"`，而是想选择创建任意数量的不同类型的问候者。让我们使用一个函数来创建问候函数。

```js
const greeterMaker = (greeting) => (person) => greeting + ' ' + person

const sayHello1 = greeterMaker('Hello')
const sayHello2 = greeterMaker('你好')

console.log(sayHello1('IU')) // "Hello IU"

console.log(sayHello2('IU')) // "你好 IU"
```

我们的 `greeterMaker` 是一个创建函数的函数！

既然你已经了解了一等函数的基本知识。那么我们来看看几个实际的例子！

## 对象验证

也许你有一系列的条件，一个对象（例如，新的用户信息）需要传递才能被认为是“有效的”。让我们创建一个函数来迭代所有的条件，并返回该对象是否有效。

```js
const usernameLongEnough = (obj) => obj.username.length >= 5

const passwordsMatch = (obj) => obj.password === obj.confirmPassword

const objectIsValid = (obj, ...funcs) => {
  for (let i = 0; i < funcs.length; i++) {
    if (funcs[i](obj) === false) return false
  }
  return true
}

const obj1 = {
  username: 'admin',
  password: '123456',
  confirmPassword: '123456'
}

const obj1Valid = objectIsValid(obj1, usernameLongEnough, passwordsMatch)
console.log(obj1Valid) // true

const obj2 = {
  username: 'editor',
  password: '654321',
  confirmPassword: '654321'
}

const obj2Valid = objectIsValid(obj2, usernameLongEnough, passwordsMatch)
console.log(obj2Valid) // false
```

如果你对 JavaScript 还比较陌生，那么你可能需要通读几次才能理解到底发生了什么，但是相信我，一旦你理解了，回报是巨大的！

## API 密钥

也许我们遇到了这样一种情况：我们想使用 API 密钥连接到 API。我们可以在每个请求上添加这个密钥。或者，我们可以创建一个函数，该函数接受一个 API 密钥参数，并在每个请求中返回包含 API 密钥的函数。

**注意：不要在前端代码中放置机密 API 密钥。相反，假设以下代码位于基于 Node 的后端中。**

```js
const apiConnect = (apiKey) => {
  const getData = (route) => {
    return axios.get(`${route}?key=${apiKey}`)
  }

  const postData = (route, params) => {
    return axios.post(route, {
      body: JSON.stringify(params),
      headers: {
        Authorization: `Bearer ${apiKey}`
      }
    })
  }

  return { getData, postData }
}

const api = apiConnect('my-secret-key')

// 不需要再包含 apiKey 了
api.getData('http://www.example.com/get-endpoint')
api.postData('http://www.example.com/post-endpoint', { name: 'IU' })
```

## 最后

现在，您已经看到 JavaScript 中的一等函数公民的作用，并且这些函数对您的 JavaScript 开发业务有多么重要。建议您可以在各种代码中使用函数的方式！
