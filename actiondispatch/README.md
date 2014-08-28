# ActionDispatch

路由相关是它的老本行。
对内：RouteSet
对外：除 RouteSet 外，routing 目录里的其它模块

外部请求进来的第一道和第二道闸门。
第一道：Middleware
第二道：Http

---

四部分

## Http

大部分为 Controller 所用。类似 Controller 的方法了，但却不是。

## Middleware

```ruby
Rails.application.send :default_middleware_stack
 => #<ActionDispatch::MiddlewareStack:0x007f922de455e0 @middlewares=[Rack::Sendfile, ActionDispatch::Static, Rack::Lock, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>
```

## RouteSet

- 物指 route_set.rb
- 本身就充满魔法
- 还是内外沟通的桥梁
- 内指 Journey
- 外指对外的接口及 routing 目录里的其它内容

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
