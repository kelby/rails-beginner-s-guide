# Action Dispatch Middleware 中间件

**Middleware - 在路由转发之后，Controller\#action 接收之前，对环境和应用进行处理。**

路由转发\(Dispatcher\#dispatch\) --&gt; 中间地带\(middleware\) --&gt; 控制器\(Controller\#action\)。

已经进入 Rails, 但还没正式进入我们应用，这时候需要做一些事。

Web 服务器已经处理过，转发过来了。做一些什么事呢？看一看下面的 middleware 都提供了什么功能就知道了。

下面只是做简介，详情可以查看 API 说明，或阅读源代码。

