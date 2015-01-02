# Action Dispatch Http

**重点是 request 和 response.**

它影响的主要是 http 相关的部分(如：request, response)，和我们的业务逻辑没有直接关联。

虽然，大部分为 Controller 所用，但要区别开来。Controller 属于 MVC 里的 C，和我们的业务逻辑有着直接关联，而它不是这样的。

Request 和 Response 是连接 ActionController 和 ActionDispatch::Http 主要方式，另外还有很小一部分用其它方式实现。

和 Web 服务器已经很接近了，接收的是 request, 响应的是 response.

每一个请求都有 request 和 response.

request 为 ActionDispatch::Request 的实例对象。<br>
response 为 ActionDispatch::Response 的实例对象。

- **Request**

包含了 Cache::Request、MimeNegotiation(又涉及 Mime)、Parameters、FilterParameters 和 URL.

以及 Headers、Uploaded File 和 URL.

- **Response**

包含了 FilterRedirect 和 Cache::Response.

以及 Mime.
