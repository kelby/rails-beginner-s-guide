
### Default Middleware Stack

配置 Rails 项目默认的 middleware stack.

相关方法为 `build_stack`，在 Finisher 里有调用到。

可以通过以下命令查看：

```ruby
Rails.application.send(:default_middleware_stack)
```

如下：

```
Rack::Sendfile

ActionDispatch::Static
ActionDispatch::LoadInterlock

Rack::Runtime
Rack::MethodOverride

ActionDispatch::RequestId

Rails::Rack::Logger

ActionDispatch::ShowExceptions
ActionDispatch::DebugExceptions
ActionDispatch::RemoteIp
ActionDispatch::Reloader
ActionDispatch::Callbacks
ActionDispatch::Cookies
ActionDispatch::Session::CookieStore
ActionDispatch::Flash

Rack::Head
Rack::ConditionalGet
Rack::ETag
```
