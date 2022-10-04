# HTTP 缓存

缓存是一种可以帮助网络连接更快的技术，因为需要传输的东西越少越好。

许多资源可能非常大，检索的时间和实际成本（例如，在移动设备上）都非常昂贵。

HTTP 提供了不同的缓存策略供浏览器使用。

## 无缓存

首先，`Cache-Control` header 可以通过使用 `no-cache` 值，告诉浏览器在不首先检查 `ETag` 值（稍后将对此进行详细介绍）的情况下永远不要使用资源的缓存版本：

```http
Cache-Control: no-cache
```

更严格的 `no-store` 选项告诉浏览器（以及所有中间网络设备）不将资源存储在其缓存中：

```http
Cache-Control: no-store
```

如果 `Cache-Control` 有 `max-age` 值，则该值用于确定此资源作为缓存有效的秒数：

```http
Cache-Control: max-age=7200
```

## `Expires` header

当发送 HTTP 请求时，浏览器根据所需的 URL 检查缓存中是否有该页面的副本。

如果有，它会检查页面的新鲜度。

如果 HTTP 响应 `Expires` header 值小于当前日期时间，则页面是最新的。

`Expires` header 采用以下形式：

```http
Expires: Tue, 04 Oct 2022 19:00:00 GMT
```

## 强缓存

HTTP/1.0 使用的是 `Expires` header，HTTP/1.1 使用的是 `Cache-Control` header。

- `Expires` 即过期时间，时间是相对于服务器的时间而言的，存在于服务端返回的响应头中，在这个过期时间之前可以直接从缓存中获取数据，无需再次请求。但这种方式存在一个问题：**无需再次请求服务器的时间和浏览器的时间可能并不一致**。
- 在 HTTP/1.1 中，使用的是 `Cache-Control` header，这个 header 采用的时间是过期时长，对应的值是 `max-age=*`。

当 `Expires` 和 `Cache-Control` 同时存在时，优先考虑 `Cache-Control` header。当缓存资源失效了，也就是没有命中强缓存，接下来就进入协商缓存。

## 协商缓存

如果缓存过期了，我们就可以使用协商缓存来解决问题。协商缓存需要请求，如果缓存有效会返回 `304 Not Modified`。

协商缓存需要客户端和服务端共同实现，和强缓存⼀样，也有两种实现方式：

- 使用 `If-Modified-Since` 和 `Last-Modified`
- 使用 `If-None-Match` 和 `ETag`

所有这些都基于使用 `If-*` 请求头，并且 `ETag` 优先级⽐ `Last-Modified` 高。

### `If-Modified-Since` 和 `Last-Modified`

它添加一个 `If-Modified-Since` header，基于从当前缓存页面获得的 `Last-Modified` header 值。

`Last-Modified` 表示本地文件最后修改时间，`If-Modified-Since` 会将当前缓存页面获得的 `Last-Modified` 的 header 值发送给服务器，询问服务器在该时间后资源是否有更新，有更新的话就会将新的资源发送回来。

否则，服务器将返回一个 `304 Not Modified` 响应。

> 这包括了发送的请求和页面请求。

但是如果在本地打开缓存文件，就会造成 `Last-Modified` 被修改，所以在 HTTP/1.1 出现了 `ETag`。

### `If-None-Match` 和 `ETag`

web 服务器（取决于设置、页面的服务方式等）可以发送 `ETag` header。

这是**资源的标识符**，类似于文件指纹。每次资源改变时，`ETag` 也应该改变。

它就像一个校验和（checksum）。

浏览器发送一个 `If-None-Match` header，其中包含一个（或多个）`ETag` 值。发送到服务器后，询问该资源 `ETag` 是否变动，有变动的话就将新的资源发送回来。

否则返回 `304 Not Modified` 响应。

### `Last-Modified` 与 `ETag` 的对比

- 性能上，`Last-Modified` 优于 `ETag`，`Last-Modified` 记录的是时间点，而 `ETag` 需要根据文件的 MD5 算法生成对应的 hash 值。
- 精度上，`ETag` 优于 `Last-Modified`。`ETag` 按照内容给资源带上标识，能准确感知资源变化，`Last-Modified` 在某些场景并不能准确感知变化。

如果两者都存在，优先判断 `If-None-Match` 进行 `ETag` 协商缓存。

## 缓存位置

当命中强缓存和协商缓存返回 304 时，浏览器会从缓存中获取资源。

浏览器中的缓存位置一共有四种，按优先级从高到低排列分别是：

- Service Worker — 其借鉴了 Web Worker 思路，主要功能有：离线缓存、消息推送和网络代理，其中离线缓存就是 `Service Worker Cache`。
- Memory Cache — 内存缓存，从效率上讲它是最快的，从存活时间来讲又是最短的，当渲染进程结束后，内存缓存也就不存在了。
- Disk Cache — 存储在磁盘中的缓存，从存取效率上讲是比内存缓存慢的，优势在于存储容量和存储时长。
- Push Cache — 推送缓存，它浏览器缓存的最后一道防线，它是 HTTP/2 的内容，详细内容可以查看 [HTTP/2 push is tougher than I thought](https://jakearchibald.com/2017/h2-push-tougher-than-i-thought/)

浏览器在选择 Disk Cache 与 Memory Cache 的存储上：内容使用率高的，文件优先进入磁盘。比较大的 JS、CSS 文件会直接放入磁盘，反之放入内存。

## 强缓存与协商缓存区别

- **强缓存** — 浏览器不会与服务端协商，而是直接获取浏览器缓存。
- **协商缓存** — 浏览器会先向服务器确认资源的有效性后，才决定是从缓存中获取资源还是重新获取资源。
- **强缓存在浏览器进行判断，而协商缓存在服务端进行判断**

## 最后

最后，我们来看一下整体的 HTTP 缓存的过程：

首先，检查 `Cache-Control` header，验证强缓存是否可用

- 如果可用的话，直接使用
- 否则进入协商缓存，发送 HTTP 请求，服务器通过请求头中的 `If-Modified-Since` 或 `If-None-Match` header 检查资源是否更新
  - 资源更新，返回资源和 200 状态码。
  - 否则，返回 304 状态码，并告诉浏览器直接从缓存中获取资源。

另外，我使用了 Node.js 模拟 HTTP 缓存行为，具体可以看看 [http.js](https://gist.github.com/lio-zero/cae442ccb74de84354398858d3486987)。

## 更多资料

[深入理解浏览器的缓存机制](https://www.jianshu.com/p/54cc04190252)
