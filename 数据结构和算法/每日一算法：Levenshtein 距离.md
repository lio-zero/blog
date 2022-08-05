# 每日一算法：Levenshtein 距离

> [**莱文斯坦距离**](https://zh.wikipedia.org/wiki/%E8%90%8A%E6%96%87%E6%96%AF%E5%9D%A6%E8%B7%9D%E9%9B%A2)，又称**Levenshtein 距离**，是编辑距离的一种。指两个字串之间，由一个转成另一个所需的最少编辑操作次数。

允许的编辑操作包括：

- 将一个字符替换成另一个字符
- 插入一个字符
- 删除一个字符

## JavaScript 实现

使用 Levenshtein 距离算法计算两个字符串之间的差。

- 如果两个字符串中的任何一个 `length` 为零，则返回另一个字符串的 `length`。
- 使用 `for` 循环迭代目标字符串的字母，使用嵌套 `for` 循环迭代源字符串的字母。
- 计算替换目标和源中 `i-1` 和 `j-1` 对应的字母的成本（如果相同，则为 `0`，否则为 `1`）。
- 使用 `Math.min()` 用上面的单元格的最小值加 1、左边的单元格的最小值加 1 或左上角的单元格的最小值加上先前计算的成本填充二维数组中的每个元素。
- 返回所生成数组最后一行的最后一个元素。

```js
const levenshteinDistance = (s, t) => {
  if (!s.length) return t.length
  if (!t.length) return s.length
  const arr = []
  for (let i = 0; i <= t.length; i++) {
    arr[i] = [i]
    for (let j = 1; j <= s.length; j++) {
      arr[i][j] =
        i === 0
          ? j
          : Math.min(
              arr[i - 1][j] + 1,
              arr[i][j - 1] + 1,
              arr[i - 1][j - 1] + (s[j - 1] === t[i - 1] ? 0 : 1)
            )
    }
  }
  return arr[t.length][s.length]
}

levenshteinDistance('duck', 'dark') // 2
```

此示例来自 30 seconds of code 的 [levenshteinDistance](https://www.30secondsofcode.org/js/s/levenshtein-distance)

## LeetCode 关于 Levenshtein 距离的题目

[编辑距离](https://leetcode-cn.com/problems/edit-distance/)

## 更多资料

- [Levenshtein Distance](https://github.com/trekhleb/javascript-algorithms/blob/master/src/algorithms/string/levenshtein-distance) - minimum edit distance between two sequences
- [Levenshteins Edit Distance](https://algorithm-visualizer.org/dynamic-programming/levenshteins-edit-distance) 可视化 Levenshtein 距离
