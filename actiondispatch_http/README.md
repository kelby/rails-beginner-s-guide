# Action Dispatch Http

**一次完整的 HTTP 里，客户端的 request 对象，和服务端的 response 对象。**

它和 Web 服务器的关系比较近，影响的主要是 http 相关的部分(也就是 request 和 response)，和我们的业务逻辑没有直接关联。

虽然，大部分为 Controller 所用，但要区别开来。Controller 属于 MVC 里的 C，和我们的业务逻辑有着直接关联，而它不是这样的。

Request 和 Response 是连接 ActionController 和 ActionDispatch::Http 主要方式，另外还有很小一部分用其它方式实现。

每一个请求都有 request 和 response.

`request` 为 ActionDispatch::Request 的实例对象。
<br>
`response` 为 ActionDispatch::Response 的实例对象。
