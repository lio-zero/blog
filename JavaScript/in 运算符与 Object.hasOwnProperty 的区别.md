# in 运算符与 Object.hasOwnProperty 的区别

`in` 运算符和 `hasOwnProperty` 方法是检查对象是否包含特定键的常用方法。

```js
const person = {
  name: 'foo'
}

console.log('name' in person) // true
console.log(person.hasOwnProperty('name')) // true
```

## 区别

对于继承的属性，`in` 将返回 `true`，它会去检查对象的原型链。顾名思义，`hasOwnProperty` 将检查属性是否为自身所有，并忽略继承的属性（忽略原型链）。

让我们重用上一个示例中的 `person` 对象。由于它是一个具有内置属性的 JavaScript 对象，例如`constructor`，`__proto__`，因此以下检查返回 `true`：

```js
'constructor' in person // true
'__proto__' in person // true
'toString' in person // true
```

而 `hasOwnProperty` 在检查这些属性和方法时返回 `false`：

```js
person.hasOwnProperty('constructor') // false
person.hasOwnProperty('__proto__') // false
person.hasOwnProperty('toString') // false
```

对于类的 `get` 和 `set` 方法，`hasOwnProperty` 也返回 `false`。

例如，我们有一个表示三角形的简单类：

```js
class Triangle {
  get vertices() {
    return 3
  }
}

// 创建新实例
const triangle = new Triangle()
```

尽管 `vertices` 是 `triangle` 的属性：

```js
triangle.vertices // 3
'vertices' in triangle // true
```

`hasOwnProperty` 仍然忽略它：

```js
triangle.hasOwnProperty('vertices') // false
```

## 建议

要检查属性是否存在，请使用 `hasOwnProperty`。使用 `in` 检查方法的存在。
