# ActionDispatch

路由相关是它的老本行。
对内：RouteSet
对外：除 RouteSet 外，routing 目录里的其它模块



---

四部分

## Http

它影响的主要是 http 相关的部分(如：request, response)，和我们的业务逻辑没有直接关联。

虽然，大部分为 Controller 所用，但要区别开来。Controller 属于 MVC 里的 C，和我们的业务逻辑有着直接关联，而它不是这样的。

Request 和 Response 是连接 ActionController 和 ActionDispatch::Http 主要手段，另外还有很小一部分用其它方式实现。

## Middleware

**middleware 在路由转发之后，Controller接收之前！**


```ruby
Rails.application.send :default_middleware_stack
 => #<ActionDispatch::MiddlewareStack:0x007f922de455e0 @middlewares=[Rack::Sendfile, ActionDispatch::Static, Rack::Lock, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>
```

middleware 的调用是一级级的：

```
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

```
  rack (1.5.2) lib/rack/etag.rb:26:in `call'
  rack (1.5.2) lib/rack/conditionalget.rb:25:in `call'
  rack (1.5.2) lib/rack/head.rb:11:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/params_parser.rb:27:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/flash.rb:254:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/cookies.rb:560:in `call'
  activerecord (4.1.0) lib/active_record/query_cache.rb:36:in `call'
  activerecord (4.1.0) lib/active_record/connection_adapters/abstract/connection_pool.rb:621:in `call'
  activerecord (4.1.0) lib/active_record/migration.rb:380:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/callbacks.rb:27:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/reloader.rb:73:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/remote_ip.rb:76:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/debug_exceptions.rb:17:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/show_exceptions.rb:30:in `call'
  railties (4.1.0) lib/rails/rack/logger.rb:20:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/request_id.rb:21:in `call'
  rack (1.5.2) lib/rack/methodoverride.rb:21:in `call'
  rack (1.5.2) lib/rack/runtime.rb:17:in `call'
  activesupport (4.1.0) lib/active_support/cache/strategy/local_cache_middleware.rb:26:in `call'
  actionpack (4.1.0) lib/action_dispatch/middleware/static.rb:64:in `call'
  rack (1.5.2) lib/rack/sendfile.rb:112:in `call'
  railties (4.1.0) lib/rails/engine.rb:514:in `call'
  railties (4.1.0) lib/rails/application.rb:144:in `call'
  rack (1.5.2) lib/rack/lock.rb:17:in `call'
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



## Routing

一切路由都可归结为: **map path to the Rack endpoint**

What does "Rack endpoint" actually mean? Rack is a modular web server abstraction layer that unifies the API for the interaction of Ruby web application frameworks and application servers. It specifies a simple interface for Rack-compliant applications, and defines standard request and response objects and application server adapters to abstract dealing with the low level details of serving web requests. A Rack endpoint is just an application that adheres to the Rack spec.

**Rack ~= Rack middleware ~= Rack endpoint ~= Rack application** 在这里不作讨论和区分，统一对待。

- 除 route_set.rb 外，routing 目录里的其它模块
- 对外提供接口

```ruby
Mapper
RoutesProxy

Redirection

UrlFor
PolymorphicRoutes
```
