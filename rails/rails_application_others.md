还有两个类: Default Middleware Stack 和 Routes Reloader. 

它们都放在 application/ 目录下，为了完成某项任务而存在，但两者之间没有联系。

### Default Middleware Stack

配置 Rails 项目默认的 middleware stack.

相关方法为 `build_stack`，在 Finisher 里有调用到。

可以通过以下命令查看：

```ruby
Rails.application.send(:default_middleware_stack)

Rack::Sendfile, 
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
Rack::ETag
```

### Routes Reloader

充分运用了 File Update Checker，当"路由"相关文件有改动时，可以实现自动重新加载。

开发环境下，不用重启，重新加载路由：

```ruby
Rails.application.reload_routes!
```

相关代码：

```ruby
def reload!
  clear!
  load_paths
  finalize!
ensure
  revert
end
```

这部分，更多信息可以查看"Active Support autoload 的类和模块"下面的【File Update Checker】章节。
