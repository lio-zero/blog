# HTTP 中 GET 和 POST 的区别？

从原理性看：

- 根据 HTTP 规范，GET 请求用于获取信息，而且应该是安全和幂等的，而 POST 请求表示可能修改服务器上资源

从表面上看：

- GET 请求的数据会附在 URL 后面（浏览器地址栏、Req Header 上），POST 的数据放在 HTTP 包体（Req Body 上）
- POST 安全性比 GET 安全性高

GET 请求：

- GET 请求可以被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可以添加书签
- 处理敏感数据时，绝不应使用 GET 请求
- GET 请求有长度限制
- GET 请求应仅用于检索数据

POST 请求：

- POST 请求永远不会被缓存
- POST 请求不会保留在浏览器历史记录中
- 不能为 POST 请求添加书签
- POST 请求对数据长度没有限制
