# Action Dispatch Routing

#### 先说说 Rack endpoint

Rack endpoint 是什么？
<br>
需要明确 Rack 是一个协议，符合这个协议的程序统称为 Rack application. Rack application 根据表现形式、调用方式、作用等又引申出几个概念，其中就包括 Rack endpoint. 在这里不作讨论和区分，统一对待。也就是说：

Rack ~= Rack middleware ~= Rack endpoint ~= Rack application

#### 再说 Routing

一切路由规则都可归结为: 映射路径到 Rack endpoint.

这里的 Rack endpoint 指的不仅仅是 Controller#action，其它形式的入口也可以，例如：Engine、Sinatra 应用。

**Routing 主要包含两部分：**

Mapper 这部分，也就是路由机制这部分，这是我们接触得最多的，它包括：Base、Concerns、HttpHelpers、Resources、Scoping.

除了 Mapper 外，用到的还有：Redirection、Polymorphic Routes、Url For.

Routing 所在文件、目录：除 RouteSet、Routes Proxy 和 Journey 外，routing 目录里的其它模块。
