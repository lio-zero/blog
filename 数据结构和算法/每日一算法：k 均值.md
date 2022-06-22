# 每日一算法：k 均值

> [**k-均值算法**](https://zh.wikipedia.org/wiki/K-%E5%B9%B3%E5%9D%87%E7%AE%97%E6%B3%95)（k-means clustering）源于信号处理中的一种向量量化方法，现在则更多地作为一种聚类分析方法流行于数据挖掘和机器学习领域。

## 示例

使用 k-means clustering 算法将给定的数据分组成 k 个聚类。

- 使用 `Array.from()` 和 `Array.prototype.slice()` 为簇的质心、距离和类初始化适当的变量。
- 使用 `while` 循环重复赋值和更新步骤，只要上一次迭代中有更改，如 `itr` 所示。
- 使用 `Math.hypot()`，`Object.keys()` 和 `Array.prototype.map()` 计算每个数据点和质心之间的欧氏距离 。
- 使用 `Array.prototype.indexOf()` 和 `Math.min()` 找到最近的质心。
- 使用 `Array.from()` 和 `Array.prototype.reduce()`，以及 `parseFloat()` 和 `Number.prototype.toFixed()` 计算新的质心。

```js
const kMeans = (data, k = 1) => {
  const centroids = data.slice(0, k)
  const distances = Array.from({ length: data.length }, () =>
    Array.from({ length: k }, () => 0)
  )
  const classes = Array.from({ length: data.length }, () => -1)
  let itr = true

  while (itr) {
    itr = false

    for (let d in data) {
      for (let c = 0; c < k; c++) {
        distances[d][c] = Math.hypot(
          ...Object.keys(data[0]).map((key) => data[d][key] - centroids[c][key])
        )
      }
      const m = distances[d].indexOf(Math.min(...distances[d]))
      if (classes[d] !== m) itr = true
      classes[d] = m
    }

    for (let c = 0; c < k; c++) {
      centroids[c] = Array.from({ length: data[0].length }, () => 0)
      const size = data.reduce((acc, _, d) => {
        if (classes[d] === c) {
          acc++
          for (let i in data[0]) centroids[c][i] += data[d][i]
        }
        return acc
      }, 0)
      for (let i in data[0]) {
        centroids[c][i] = parseFloat(Number(centroids[c][i] / size).toFixed(2))
      }
    }
  }

  return classes
}

kMeans(
  [
    [0, 0],
    [0, 1],
    [1, 3],
    [2, 0]
  ],
  2
) // [0, 1, 1, 0]
```

此示例来自 30 seconds of code 的 [K-means clustering](https://www.30secondsofcode.org/js/s/k-means)

## 更多资料

[k-Means](https://github.com/trekhleb/javascript-algorithms/blob/master/src/algorithms/ml/k-means) - k-Means clustering algorithm
