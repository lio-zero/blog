# 实现 instanceof 运算符

> MDN：[`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof) 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

**实现思路**

判断右边变量的原型是否存在于左边变量的原型链上。通过实例对象属性的 `__proto__` 去一层层查找，如果和构造函数的 `prototype` 相等则返回 `true`，如果一直没有查找成功则返回 `false`。

```js
function myInstanceOf(left, right) {
  if (typeof instance !== 'object' && typeof instance !== 'function') {
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

理解原型的继承机制可以很好的帮助你理解这层关系。
