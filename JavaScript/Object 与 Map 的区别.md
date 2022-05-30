# Object 与 Map 的区别

存储键值对是我们在 JavaScript 中必须处理的常见问题。最基本的方法是使用对象：

```js
const person = {}
person.name = 'Foo'
person.age = 20

// or
const person = {
  name: 'Foo',
  age: 20
}
```

从 ES6 引入的 `Map` 数据结构提供了相同的功能。上面的示例代码可以用 `Map` 重写，如下所示：

```js
const person = new Map()
person.set('name', 'Foo')
person.set('age', 20)
```

## 区别

- 对象仅接受字符串和 `Symbol` 键。其他类型将自动转换为字符串。另一方面，`Map` 接受任何类型的键。

- 我们可以使用 `forEach` 或 `for...of` 直接迭代 `map` 的属性：

```js
const styles = new Map()
styles.set('color', 'blue')
styles.set('fontSize', '12px')

styles.forEach((value, key) => console.log(key, '=', value))

// color = blue
// fontSize = 12px
```

对象不能直接进行迭代。为了循环对象的属性，我们必须使用 `object.keys`、`object.values` 或 `object.entries` 来接收键、值或键值对的列表。

```js
const styles = {
  color: 'blue',
  fontSize: '12px'
}

Object.keys(styles).forEach((key) => console.log(key, '=', styles[key]))

// color = blue
// fontSize = 12px
```

对象具有特殊属性，如 `constructor`、`proto` 等。

```js
let person = {}
person['constructor'] // ƒ Object() { [native code] }
```

而 `Map` 仅包含我们定义的内容：

```js
let person = new Map()
person.get('constructor') // undefined
```

JSON 支持 `Object`：

```js
const person = {}
person.name = 'Foo'
person.age = 20

JSON.stringify(person) // "{"name":"Foo","age":20}"
```

使用 `Map`，当使用 JSON 序列化时，无法获取正确的数据：

```js
const person = new Map()
person.set('name', 'Foo')
person.set('age', 20)

JSON.stringify(person) // "{}"
```

## 很高兴知道

`map` 保持项目的顺序。这意味着，当您在 `map` 的关键点上循环时，我们将看到与它们插入 `map` 时相同的顺序。

对于仅由 `string` 键和 `symbol` 键组成的对象也是如此。如果存在需要转换为 `string` 的键，则不会保留 `object` 键的顺序。

```js
let codes = { A: 65, B: 66, C: 67, 0: 48 }
codes // {0: 0, A: 65, B: 66, C: 67}
Object.keys(codes) // ["0", "A", "B", "C"]
```

## 建议

如果我们想存储键/值对而不关心在 JSON 中序列化它们，那么就使用 `map`。通过循环获取 `map` 的大小比使用对象更舒适。

如果我们希望在原始数据和 JSON 之间来回转换，或者包含特定的业务逻辑，则应使用 `Object`：

```js
const person = {
  firstName: 'Foo',
  lastName: 'Bar',
  getFullName() {
    return `${this.firstName} ${this.lastName}`
  }
}
```

## 其他差异

对于对象，我们需要手动计算。而 `map` 有一个内置的 `size` 属性，可以用来跟踪键/值对的数量。

```js
const obj = new Map()
obj.set('name', 'O.O')
obj.set('age', 18)

obj.size // 2
```

> 您可以查看[如何在 JavaScript 中获取对象的长度](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E8%8E%B7%E5%8F%96%E5%AF%B9%E8%B1%A1%E7%9A%84%E9%95%BF%E5%BA%A6.md)，以了解获取对象长度的多种方式。

- 清除对象需要手动操作，并且在某些情况下可能并不简单。`map` 通过使用 `Map.prototype.clear()` 解决这个问题。
- 您可以使用 `in` 运算符或 `Object.prototype.hasOwnProperty()` 检查对象中是否存在给定键。`map` 使用 `Map.prototype.has()` 完成同样的事情。
