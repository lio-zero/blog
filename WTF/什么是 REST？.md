# 什么是 REST？

[REST](https://restfulapi.net/)（REpresentational State Transfer）是一种用于网络架构的软件设计模式。RESTful web 应用程序以其资源信息的形式公开数据。

通常，这个概念用于 web 应用程序管理状态。对于大多数应用程序，读取、创建、更新和删除数据是一个共同的主题。数据被模块化成单独的表，比如 `posts`，`users`，`comments`。

一个 RESTful API 通过以下方式公开了对这些数据的访问:

- 资源的标识符。这称为资源的端点或 URL。
- 服务器应该以 HTTP 方法或谓词的形式对该资源执行的操作。常见的 HTTP 方法有 `GET`、`POST`、`PUT` 和 `DELETE`。

下面是一个带有 `posts` 资源的 URL 和 HTTP 方法的示例:

- 读取：=> `/posts/` GET
- 读取所有 => `/record/` GET
- 创建：=> `/posts/new` POST
- 更新：=> `/posts/:id` PUT
- 删除：=> `/posts/:id` DELETE

该模式的替代方案，如 [GraphQL](https://graphql.org/)

## 更多资料

- [RESTful API 设计指南](https://www.ruanyifeng.com/blog/2014/05/restful_api.html)
- [What is REST — A Simple Explanation for Beginners, Part 1: Introduction](https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-1-introduction-b4a072f8740f)
- [What is REST — A Simple Explanation for Beginners, Part 2: REST Constraints](https://medium.com/@shifrb/what-is-rest-a-simple-explanation-for-beginners-part-2-rest-constraints-129a4b69a582)
