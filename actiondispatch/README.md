# Action Dispatch

路由相关是它的老本行。
对内：RouteSet；
对外：除 RouteSet 外，routing 目录里的其它模块。

四部分

## Routing

一切路由规则都可归结为: **map path to the Rack endpoint**

Rack 是一个协议，符合这个协议的程序统称为 Rack application. Rack application 根据表现形式、调用方式、作用等不同又引申出几个概念。在这里不作讨论和区分，统一对待。也就是说：

**Rack ~= Rack middleware ~= Rack endpoint ~= Rack application** 

- 除 route_set.rb 外，routing 目录里的其它模块
- 对外提供接口

```ruby
Mapper

Redirection

UrlFor
PolymorphicRoutes
```

其它

```
Endpoint # Constraints、 Redirect 和 Dispatcher 的基类，它们都是 endpoint.

RouteWrapper       # 被 RoutesInspector 调用。
RoutesInspector    # 被 middleware DebugExceptions 和 rake routes 调用。
HtmlTableFormatter # 格式化 HTML 里的路由信息，给人阅读的。
ConsoleFormatter   # 格式化控制台里的路由信息，给人阅读的。
```

## RouteSet

- 特指 route_set.rb
- 本身就充满魔法
- 还是内外沟通的桥梁
- 内指 Journey
- 外指对外的接口及 routing 目录里的其它内容

```ruby
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new

routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end
```

从 ActionDispatch 转换站场到 ActionController.
(准确点：ActionDispatch -> Metal -> AbstractController -> ActionController)

```ruby
def dispatch(controller, action, env)
  controller.action(action).call(env)
end
```

除上述外，还有：

```
RoutesProxy # 从 RouteSet 里抽取而来
```

## Http

它影响的主要是 http 相关的部分(如：request, response)，和我们的业务逻辑没有直接关联。

虽然，大部分为 Controller 所用，但要区别开来。Controller 属于 MVC 里的 C，和我们的业务逻辑有着直接关联，而它不是这样的。

Request 和 Response 是连接 ActionController 和 ActionDispatch::Http 主要手段，另外还有很小一部分用其它方式实现。

## Middleware

**middleware 在路由转发之后，Controller接收之前！**

```ruby
Rails.application.send :default_middleware_stack

=> #<ActionDispatch::MiddlewareStack:0x007f7f64f7d6c8
 @middlewares=
  [Rack::Sendfile,
   ActionDispatch::Static,
   Rack::Lock,
   Rack::Runtime,
   Rack::MethodOverride,
   ActionDispatch::RequestId,
   Rails::Rack::Logger,
   ActionDispatch::ShowExceptions,
   ActionDispatch::DebugExceptions,
   ActionDispatch::RemoteIp,
   ActionDispatch::Reloader,
   ActionDispatch::Callbacks,
   ActionDispatch::Cookies,
   ActionDispatch::Session::CookieStore,
   ActionDispatch::Flash,
   ActionDispatch::ParamsParser,
   Rack::Head,
   Rack::ConditionalGet,
   Rack::ETag]>
```

middleware 的调用是一级级的：

```ruby
use Rack::Sendfile
use ActionDispatch::Static
use Rack::Lock
use #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007fb09b31d078>
use Rack::Runtime
use Rack::MethodOverride
use ActionDispatch::RequestId
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use ActionDispatch::DebugExceptions
use ActionDispatch::RemoteIp
use ActionDispatch::Reloader
use ActionDispatch::Callbacks
use ActiveRecord::Migration::CheckPending
use ActiveRecord::ConnectionAdapters::ConnectionManagement
use ActiveRecord::QueryCache
use ActionDispatch::Cookies
use ActionDispatch::Session::CookieStore
use ActionDispatch::Flash
use ActionDispatch::ParamsParser
use Rack::Head
use Rack::ConditionalGet
use Rack::ETag
run AppName::Application.routes
```

从应用的日常报错上，也能看出应用所使用的 middleware 及其顺序，在此就不贴示例了。
