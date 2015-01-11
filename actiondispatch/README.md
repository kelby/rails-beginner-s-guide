# Action Dispatch

四部分：

- Routing

路由相关对外<br>
指的是除 RouteSet 和 Routes Proxy  外，routing 目录里的其它模块。

- RouteSet

路由相关对内，包含了 Routes Proxy

- Http

HTTP request 和 response.

- Middleware

路由 --> 中间地带(middleware) --> 控制器。
