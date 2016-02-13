## 查看项目用了哪些 Middleware

**命令行里查看**，项目有哪些 middleware:

```ruby
> rails middleware

use Rack::Sendfile
use ActionDispatch::Static
use ActionDispatch::LoadInterlock
use ActiveSupport::Cache::Strategy::LocalCache::Middleware
use Rack::Runtime
use Rack::MethodOverride
use ActionDispatch::RequestId
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use WebConsole::Middleware
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
use Rack::Head
use Rack::ConditionalGet
use Rack::ETag
use ActionView::Digestor::PerRequestDigestCacheExpiry
run AppName::Application.routes
```

**控制台里查看**，项目有哪些 middleware:

```ruby
Rails.application.send :default_middleware_stack

=> #<ActionDispatch::MiddlewareStack:0x007feaf533fbc0
 @middlewares=
  [Rack::Sendfile,
   ActionDispatch::Static,
   ActionDispatch::LoadInterlock,
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

```ruby
Rails.application.send :middleware

=> #<ActionDispatch::MiddlewareStack:0x007feaf45cc7b8
 @middlewares=
  [Rack::Sendfile,
   ActionDispatch::Static,
   Rack::Lock,
   # 1
   #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007feaf519dda8>,
   Rack::Runtime,
   Rack::MethodOverride,
   ActionDispatch::RequestId,
   Rails::Rack::Logger,
   ActionDispatch::ShowExceptions,
   ActionDispatch::DebugExceptions,
   ActionDispatch::RemoteIp,
   ActionDispatch::Reloader,
   ActionDispatch::Callbacks,
   # 2
   ActiveRecord::Migration::CheckPending,
   # 3
   ActiveRecord::ConnectionAdapters::ConnectionManagement,
   # 4 允许 ActiveRecord 查询缓存
   ActiveRecord::QueryCache,
   ActionDispatch::Cookies,
   ActionDispatch::Session::CookieStore,
   ActionDispatch::Flash,
   ActionDispatch::ParamsParser,
   Rack::Head,
   Rack::ConditionalGet,
   Rack::ETag]>
```


**说明：**默认添加 middleware 可以在 Rails::Application::DefaultMiddlewareStack#build_stack 里查看；而我们手动添加的 middleware 通常在 config/ 下添加。执行middleware 由 ActionController::Metal.action 完成。

在上面例子里，除默认 middleware 外，我们还额外使用了 4 个 middleware. 在实际项目中，你应该可以看到比这多得多的 middleware.

另外，从应用的日常报错上，也能看出应用所使用的 middleware 及其顺序，在此就不贴示例了。
