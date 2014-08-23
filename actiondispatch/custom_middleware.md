# Custom Middleware

[Rails on Rack](http://guides.rubyonrails.org/rails_on_rack.html)

[Where do you put your Rack middleware files and requires?](http://stackoverflow.com/questions/3428343/where-do-you-put-your-rack-middleware-files-and-requires)

[#151 Rack Middleware](http://railscasts.com/episodes/151-rack-middleware)

[Sanitizing POST params with custom Rack middleware](http://pivotallabs.com/sanitizing-post-params-with-custom-rack-middleware/)

在 app/middleware/ 目录下，按照 middleware 写，然后在 config 里 use 就行了，不用做其它的配置。

Middleware 处理的是 @app 和 env 所以也是很有限的。

---

```
> rake middleware

use Rack::Sendfile
use ActionDispatch::Static
use Rack::Lock
use #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007fc0ef0c5aa0>
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
run PolymorphicUrl001::Application.routes
```

```ruby
Rails.configuration.middleware == Rails.application.send(:middleware)

# => true
```

```ruby
Rails.application.send :middleware
 => #<ActionDispatch::MiddlewareStack:0x007f922d82fed0 @middlewares=[Rack::Sendfile, ActionDispatch::Static, Rack::Lock, #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007f922b765510>, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActiveRecord::Migration::CheckPending, ActiveRecord::ConnectionAdapters::ConnectionManagement, ActiveRecord::QueryCache, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>

Rails.application.send :default_middleware_stack
 => #<ActionDispatch::MiddlewareStack:0x007f922de455e0 @middlewares=[Rack::Sendfile, ActionDispatch::Static, Rack::Lock, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActionDispatch::Cookies, ActionDispatch::Session::CookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag]>
```
