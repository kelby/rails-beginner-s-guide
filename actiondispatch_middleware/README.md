# Action Dispatch Middleware

**middleware 在路由转发之后，Controller 接收之前！**

已经进入 Rails, 但还没正式进入我们应用，这时候需要做一些事。

Web 服务器已经处理过，转发过来了。做一些什么事呢？看一看下面的 middleware 都提供了什么功能就知道了。

下面只是做简介，详情可以查看 API 说明，或阅读源代码。

可以参考【Rack - Ruby Web server 接口】章节了解更多关于 Rack 的内容。

---

路由 --> 中间地带(middleware) --> 控制器。
