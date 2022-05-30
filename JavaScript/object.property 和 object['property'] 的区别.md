# object.property 和 object['property'] 的区别

以下是使用引号和不使用引号声明和访问对象属性的方式：

```js
const obj = { key: 'value' }
obj.key

// Or
const obj = { key: 'value' }
obj['key']
```

## 区别

如果属性名是数字文字，则可能需要使用不同的键来访问属性值。

```js
const obj = { 12e34: 'hello' }

console.log(obj['1.2e34']) // undefined
console.log(obj['1.2e+35']) // 'hello'
```

用引号括住属性将有助于我们避免问题：

```js
const obj = { '12e34': 'hello' }

console.log(obj['12e34']) // 'hello'
```

如果属性是保留[关键字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)之一，那么我们必须使用引号来访问属性值。

```js
const styles = { class: 'foo' }

// 将引发异常，因为类是保留关键字
styles[class]

// 返回 'foo'
styles['class']
```

对于包含特殊字符（如 `-`）的属性，也会发生同样的情况。
