# ++value 和 value++ 的区别

`++` 是在操作数上加 `1` 的自增操作符。操作符有两个变量使用 `++` 作为前缀或后缀：`++value` 和 `value++`。

它们在 `for` 循环中也有同样的作用。下面的循环输出从 `0` 到 `4` 的相同结果:

```js
for (let i = 0; i < 5; i++) {
  console.log(i)
}

for (let i = 0; i < 5; ++i) {
  console.log(i)
}
```

## 区别

当返回一个值时，`return value++` 在值增加之前返回原始值。而 `return ++value` 则增加值并返回更新后的值。

下面的 `foo` 和 `bar` 函数返回不同的结果:

```js
const foo = (x) => x++
const bar = (x) => ++x

foo(1) // 1
bar(1) // 2
```

## 扩展

`value += 1` 是后缀递增值 `++` 的另一种选择。值得注意的是，当使用 `string` 时，它们可以提供不同的结果。

后缀递增值 `++` 将先将值转换为数字，然后再递增值:

```js
let value = '5'
value++
value // 6
```

另一方面，如果 `value` 是字符串，`value += 1` 首先将第二个参数（`1`）转换为字符串，然后将它们连接在一起。

```js
let value = '5'
value += 1
value // '51'
```
