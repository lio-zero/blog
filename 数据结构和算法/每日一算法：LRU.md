# 每日一算法：LRU 缓存

[LRU](<https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)>)（Least Recently Used，最近最少使用）缓存是一种数据缓存策略，其核心思想是**删除最近最少使用的数据来释放空间**。

实现 LRU 缓存的方法有很多，常见的有两种：

- 使用双向链表 — 每次访问缓存数据时将其移到链表头部，当缓存满时将链表尾部的数据删除
- 使用哈希表加队列 — 每次访问缓存数据时将其移到队列头部，当缓存满时将队列尾部的数据删除，同时在哈希表中删除对应的键值对。

还有一点需要注意，**新数据将插入到链表/队列头部**。

## JavaScript 实现

我们可以使用 JavaScript 的 Map 来实现的一个简单的 LRU 缓存。其中 `get` 方法根据给定的键获取值，并将其放到缓存的最前面。`put` 方法则用来向缓存中添加数据，如果缓存已满，则会删除最近最少使用的数据。

```js
class LRUCache {
  constructor(capacity) {
    // 初始化 LRU 最大容量
    this.capacity = capacity
    this.cache = new Map()
  }

  get(key) {
    if (!this.cache.has(key)) return -1
    const value = this.cache.get(key)
    if (typeof value === 'number') {
      this.cache.delete(key)
      this.cache.set(key, value)
      return value
    }
  }

  put(key, value) {
    if (this.cache.has(key)) this.cache.delete(key)
    this.cache.set(key, value)
    if (this.cache.size > this.capacity) {
      this.cache.delete(this.cache.keys().next().value)
    }
  }
}
```

在这个算法中，获取数据和添加数据的时间复杂度都为 `O(1)`，因为我们使用了 JavaScript 的 Map 实现。

它是基于哈希表实现的，具有高效的键值对存储和查询。而限制容量大小也是 `O(1)` 的，因为我们使用了 `Map.size` 来进行容量限制，每次添加时都会检查是否超过容量。

综上所述，这个简单的 LRU 缓存的时间复杂度是 `O(1)`。

当然实际的实现可能有更优秀的算法和数据结构，更多的调优可能使它更加高效稳定，这里只是一个简单的实现例子。

使用示例：

```js
const cache = new LRUCache(2)

cache.put(1, 1)
cache.put(2, 2)
cache.get(1) // 1
cache.put(3, 3) // 该操作会使得 key 2 被移除
cache.get(2) // -1 => 未找到
cache.put(4, 4) // 该操作会使得 key 1 被移除
cache.get(1) // -1 => 未找到
cache.get(3) // 3
cache.get(4) // 4
```

## `get` 和 `put` 哪个操作使用频率更高？

对于 LRU 算法来说，`get` 操作的使用频率通常比 `put` 操作的使用频率高，因为缓存的主要目的是为了提高获取数据的速度，而 `put` 操作的主要目的是为了更新缓存。

## LRU 的使用场景

LRU 算法通常用于缓存数据，最常见的应用场景包括：

- 分布式缓存系统，如 Memcached 和 Redis
- 浏览器缓存，如 HTTP 缓存和浏览器缓存
- 操作系统缓存，如文件系统缓存和网络缓存
- 运算中使用缓存，如矩阵乘法，这样可以提高运算的效率

很多前端框架、库都使用到了 LRU 缓存，例如 Vue 的 `keep-alive` 的 `max` 属性便使用了 LRU 缓存策略来支持可以缓存多少组件实例。

总之，LRU 缓存算法在那些需要快速访问最近使用的数据，同时需要限制缓存大小的场景中是非常有用的。

## 更多资料

- [LeetCode 上有多道 LRU 缓存相关的算法题](https://leetcode.cn/search/?q=LRU)。
- [node-lru-cache](https://github.com/isaacs/node-lru-cache) — 一个用于 Node.js 的 LRU 缓存库，基于 JavaScript 的对象和链表。你可以尝试解读该库[源码](https://github.com/isaacs/node-lru-cache/blob/main/index.js)。
