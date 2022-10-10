# 实现 instanceof 运算符

> MDN：[`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof) 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

**实现思路**：

- 判断左边变量的原型是否存在于右边构造函数的原型链上
- 通过实例对象属性的 `__proto__` 去一层层查找，如果和构造函数的 `prototype` 相等则返回 `true`，如果一直没有查找成功则返回 `false`。

```js
function myInstanceOf(left, right) {
  // 必须是对象或函数
  if (!(left && typeof ['object', 'function'].includes(typeof obj))) {
    return false
  }

  // 实例对象属性，指向其构造函数原型
  let leftValue = left.__proto__
  // 构造函数原型
  let rightValue = right.prototype
  while (true) {
    // 如果为 null，说明原型链已经查找到最顶层了，真接返回 false
    if (leftValue === null) return false
    // 查找到原型
    if (leftValue === rightValue) return true
    // 继续向上查找
    leftValue = leftValue.__proto__
  }
}
```

示例：

```js
function Fn() {}
let foo = new Fn()

myInstanceOf('helloworld', String) // false
myInstanceOf(new String('helloworld'), String) // true
myInstanceOf({}, Object) // true
myInstanceOf(null, Fn) // false
myInstanceOf(foo, Fn) // true
```

理解原型的继承机制可以很好的帮助你理解这层关系。

## 更多资料

[JavaScript 深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2)
