# 常见的 HTTP 状态码

- 1xx - 信息响应 - 传达传输协议级别的信息
- 2xx - 成功的响应 - 表示客户端的请求已成功接受
- 3xx - 重定向 - 表示客户端必须采取一些额外的操作才能完成其请求
- 4xx - 客户端错误 - 此类错误状态码指向客户端
- 5xx - 服务端错误 - 服务器对这些错误状态代码负责

## 信息响应

- 100：继续（Continue） — 到目前为止一切正常
- 101：交换协议（Switching Protocols） — 客户端要求服务器交换协议，服务器已同意这样做。
- 102：正在处理（Processing） — 正在处理请求，尚无响应

## 成功的响应

- 200：成功（OK） — 请求成功
- 201：正创建（Created） — 请求已完成，已创建新资源
- 204：无内容（No Content）— 含义与 200 相同，但响应头后没有 body 数据。
- 206：部分内容（Partial Content）— 当从客户端发送 `Range` 范围标头以只请求资源的一部分时，将使用此响应码。

## 重定向

- 301：永久移动（Moved Permanently） — 资源永久移动到新的 URL
- 302：临时移动（Moved Temporarily） — 资源临时移动到新的 URL
- 304：未修改（Not Modified）— 当协商缓存命中时会返回这个状态码。
- 307：临时重定向（Temporary Redirect） — 与 302 请求类似，用于 post 请求，它不允许更改 HTTP 方法
- 308：永久重定向（Permanent Redirect） — 与 301 请求类似，用于 post 请求，它不允许更改 HTTP 方法

## 客户端错误

此类错误状态代码指向客户端

- 400：请求错误（Bad Request） — 服务器无法理解和处理请求
- 401：未经授权（Unauthorized） — 需要验证，用户尚未验证
- 403：禁止（Forbidden） — 对资源的访问权限不足
- 404：未找到（Not Found） — 找不到请求的资源
- 405：方法不允许（Method Not Allowed） — 服务器知道请求方法，但目标资源不支持该方法。例如，API 可能不允许调用 `DELETE` 来删除资源。
- 409：冲突（Conflict） — 当客户端试图执行一个**会导致一个或多个资源处于不一致状态**的操作时。
- 410：已移除（Gone） — 由于有意移除，因此请求不再可用
- 416：请求范围不满足（Requested Range Not Satisfiable） — 客户端已使用 `Range` 标头请求文件的一部分，但服务器无法提供该部分。当响应中存在 `Accept-Ranges` 字段且不为 `none` 时，表示该服务器支持请求范围请求（最近在研究文件上传和下载，遇到 416 记录一下）

## 服务端错误

- 500：内部服务器错误（Internal Server Error）— 通用未处理的服务器错误
- 502：网关错误（Bad Gateway） — 网关服务器收到无效响应
- 503：服务不可用（Service Unavailable）— 服务器暂时无法处理请求
- 504：网关超时 （Gateway Timeout） — 网关服务器未及时获得响应

## 更多资源

- [完整的 HTTP 状态代码列表](https://httpstatuses.com/)
- [HTTP 状态码](http://www.restapitutorial.com/httpstatuscodes.html)
- [Mozilla HTTP 响应状态码](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

如果你想了解这方面更加全面、详细的内容，可查阅 [《RESTful WebServices》](https://book.douban.com/subject/3094230/) 书籍，也可以在网上查阅一些电子文档，或者摘翻。
