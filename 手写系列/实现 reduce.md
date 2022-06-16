# 实现 reduce

**重点**：考虑是否有初始值

```js
Array.prototype.myReduce = function (cb, initialValue) {
  var total = initialValue || this[0]
  var start = initialValue ? 0 : 1
  for (var i = start; i < this.length; i++) {
    total = cb(total, this[i], i, this)
  }
  return total
}

var arr = [1, 2, 3, 4, 5]
arr.myReduce((accumulator, elem) => (accumulator += elem)) // 15
arr.myReduce((accumulator, elem) => (accumulator += elem), 100) // 115
```
