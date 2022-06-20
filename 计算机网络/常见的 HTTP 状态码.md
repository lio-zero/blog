# 常见的 HTTP 状态码

## 1xx - 信息 - 传达传输协议级别的信息

- 100：继续（Continue） —— 到目前为止一切正常
- 102：正在处理（Processing） —— 正在处理请求，尚无响应

## 2xx - 成功 - 表示客户端的请求已成功接受

- 200：成功（OK） —— 请求成功
- 204：无内容（No Content）—— 含义与 200 相同，但响应头后没有 body 数据。
- 201：正创建（Created） —— 请求已完成，已创建新资源

## 3xx - 重定向 - 表示客户端必须采取一些额外的操作才能完成其请求

- 301：永久移动（Moved Permanently） —— 资源永久移动到新的 URL
- 302：临时移动（Moved Temporarily） —— 资源临时移动到新的 URL
- 304：未修改（Not Modified）—— 当协商缓存命中时会返回这个状态码。

## 4xx - 客户端错误 - 此类错误状态码指向客户端

此类错误状态代码指向客户端

- 400：请求错误（Bad Request） —— 服务器无法理解和处理请求
- 401：未经授权（Unauthorized） —— 需要验证，用户尚未验证
- 403：禁止（Forbidden） —— 对资源的访问权限不足
- 404：未找到（Not Found） —— 找不到请求的资源
- 409：冲突（Conflict） —— 当客户端试图执行一个**会导致一个或多个资源处于不一致状态**的操作时。
- 410：已移除（Gone） —— 由于有意移除，因此请求不再可用

## 5xx - 服务端错误 - 服务器对这些错误状态代码负责

- 500：内部服务器错误（Internal Server Error）—— 通用未处理的服务器错误
- 502：网关错误（Bad Gateway） —— 网关服务器收到无效响应
- 503：服务不可用（Service Unavailable）—— 服务器暂时无法处理请求
- 504：网关超时 （Gateway Timeout） —— 网关服务器未及时获得响应

## 更多资源

- [完整的 HTTP 状态代码列表](https://httpstatuses.com/)
- [HTTP 状态码](http://www.restapitutorial.com/httpstatuscodes.html)
- [Mozilla HTTP 响应状态码](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

如果你想了解这方面更加全面、详细的内容，可查阅 [《RESTful WebServices》](https://book.douban.com/subject/3094230/) 书籍，也可以在网上查阅一些电子文档，或者摘翻。
