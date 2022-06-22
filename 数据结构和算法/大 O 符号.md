# 大 O 符号

[大 O 符号](https://zh.wikipedia.org/wiki/%E5%A4%A7O%E7%AC%A6%E5%8F%B7)（Big O Notation），又称为渐进符号，用于描述算法的时间复杂度。最好的算法将执行得最快并且具有最简单的复杂性。

算法并不总是执行相同的操作，并且可能会根据提供的数据而有所不同。虽然在某些情况下它们会执行非常快，但在其他情况下它们会执行缓慢，即使要处理相同数量的元素。

在以下示例中，基准时间为 1 个元素 = `1ms`。

## O(1) — 恒定时间复杂性

```js
arr[arr.length - 1]
```

1000 个元素 = `1ms`

无论数组有多少个元素，理论上执行所需的时间（不包括实际的变化）都是相同的。

## O(N) — 线性时间复杂度

```js
arr.filter(fn)
```

1000 个元素 = `1000ms`

执行时间将随着数组元素的数量线性增加。如果数组有 1000 个元素，并且函数需要 1ms 才能执行，那么 7000 个元素将需要 7ms 执行。这是因为函数必须在返回结果之前遍历数组的所有元素。

## O([1, N]) — 恒定/线性时间复杂度

```js
arr.some(fn)
```

1000 个元素 =`1ms <= x <= 1000ms`

执行时间取决于提供给函数的数据，它可能会很早或很晚返回。这里最好的情况是 `O(1)`，最坏的情况是 `O(N)`。

## O(NlogN) — 线性时间复杂度

```js
arr.sort(fn)
```

1000 个元素 ~= `10000ms`

浏览器通常为 `sort()` 方法实现快速排序算法，快速排序的平均时间复杂度为 `O(NlogN)`。这对于大型集合非常有效。

## O(N^2) — 二次方时间复杂度

```js
for (let i = 0; i < arr.length; i++) {
  for (let j = 0; j < arr.length; j++) {
    // ...
  }
}
```

1000 个元素 = `1000000ms`

执行时间随着元素的数量呈二次曲线增长。通常是嵌套循环的结果。

## O(N!) — 阶乘时间复杂度

```js
const permutations = (arr) => {
  if (arr.length <= 2) return arr.length === 2 ? [arr, [arr[1], arr[0]]] : arr
  return arr.reduce(
    (acc, item, i) =>
      acc.concat(
        permutations([...arr.slice(0, i), ...arr.slice(i + 1)]).map((val) => [
          item,
          ...val
        ])
      ),
    []
  )
}
```

1000 个元素 = `Infinity`（实际上）

即使只在数组中添加一个，执行时间也会增长得非常快。

> **Tips**：随着执行时间呈指数增长，需要注意嵌套循环。

## 更多资料

- [Big O Notation: A primer for beginning devs](https://blog.educative.io/a-big-o-primer-for-beginning-devs/)
- [A beginner's guide to Big O Notation](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/)
- [A Simplified Explanation of the Big O Notation](https://medium.com/karuna-sehgal/a-simplified-explanation-of-the-big-o-notation-82523585e835)
- [All you need to know about “Big O Notation” to crack your next coding interview](https://www.freecodecamp.org/news/all-you-need-to-know-about-big-o-notation-to-crack-your-next-coding-interview-9d575e7eec4/)
- [A story of Big O](https://medium.com/@deadcookies/a-story-of-big-o-453471a35e62)
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- [The Big 'O' Notation - An Introduction Sarah Chima - Front-End Developer](https://sarahchima.com/blog/big-o-notation/)
