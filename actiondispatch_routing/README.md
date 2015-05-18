# Action Dispatch Routing

一切路由规则都可归结为: **映射路径到 Rack endpoint.**

这里的 Rack endpoint 指的不仅仅是 Controller#action，其它形式的入口也可以，例如：Engine、Sinatra 应用。

那么，Rack endpoint 是什么？
<br>
需要明确 Rack 是一个协议，符合这个协议的程序统称为 Rack application. Rack application 根据表现形式、调用方式、作用等又引申出几个概念。在这里不作讨论和区分，统一对待。也就是说：

**Rack ~= Rack middleware ~= Rack endpoint ~= Rack application** 

Action Dispatch 本身很复杂，但我们使用 Rails 进行开发的过程中接触到的主要就是 Mapper 这部分。

也就是路由机制这部分，它包括：Base、Concerns、HttpHelpers、Resources、Scoping.
<br>
除了 Mapper 外，还用到的就只剩：Redirection、Polymorphic Routes、Url For.

**范围**：
<br>
Routing 指的是除 RouteSet、Routes Proxy 和 Journey 外，routing 目录里的其它模块。
