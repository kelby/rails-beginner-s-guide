# Rails - 其它

各个 release note 一定要认真看！

比较有趣好玩，但现在还没想好如何放到文中。暂时记录在此。

[A Rails App in a Single File ](http://rofish.net/rails_single_file.pdf)

You can use Rails modules independently.

Start with Rack

```ruby config.ru
require 'rubygems'
require 'bundler/setup’ # Gemfile only lists ‘rack’

run Proc.new {|env|
 if env["PATH_INFO"] == "/"
 [200,
 {"Content-Type" => "text/html"},
 ["<h1>Front Page</h1>"]
 ]
 else
 [404,
 {"Content-Type" => "text/html"},
 ["<h1>Not Found</h1>"]
 ]
 end
}
```

All you need is the routes, the controllers, the views, and everything else.

```ruby config.ru
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new

routes.draw do
 get '/' => 'mainpage#index'
 get '/page/:id' => 'mainpage#show'
end
```

```ruby mainpage_controller.rb
require 'action_controller'

class MainpageController < ActionController::Metal
  include AbstractController::Rendering

  include ActionView::Rendering
  prepend_view_path('/path/to/templates')
  include ActionController::Rendering
  include ActionController::ImplicitRender

  def index
    self.response_body = "<h1>Front Page</h1>"
  end

  def show
    self.status = 404
    self.response_body = "<pre>#{env['action_dispatch.request.path_parameters'][:id]}</pre>"
  end
end
```

https://gist.github.com/ROFISH/11273048

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣


在 Rails 里 metal 也属于 middleware，我们可以这么用：

```ruby config/routes.rb
  get 'hello' => 'hello#index'
  ...
```

```ruby app/controllers/hello_controller.rb
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

然后浏览器访问：http://localhost:3000/hello 可以获取刚才的内容。

日志
    Started GET "/hello" for 127.0.0.1 at 2014-04-27 08:57:07 +0800

改为

Started GET "/hello" for 127.0.0.1 at 2014-04-27 09:04:49 +0800
Processing by HelloController#index as HTML
Completed 200 OK in 1ms (ActiveRecord: 0.0ms)

> rake middleware

```
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

--------
## Rendering empty responses

Sometimes (and especially once you start dealing with writing web services) you’ll find yourself wanting to return an empty response, with only a status code and (possibly) a few headers set.

You can do this easily enough using the render method:

```ruby
headers['Location'] = person_url(@person)
render :nothing => true, :status => "201 Created"
```

That, however, is unbearably verbose, especially when you find yourself needing to do it in multiple places.

Enter the head method:

```ruby
head :created, :location => person_url(@person)
```

There, isn’t that beautiful?
------------

黄志敏的 gem 'simple_cacheable' 应该属于对象缓存了。在对象级别做缓存！

------------

缓存里的不同对象，能够知道它们之间的关联关系，好神奇。
另外，[Web应用的缓存设计模式](http://robbinfan.com/blog/38/orm-cache-sumup)

------------

n + 1 并不可怕，有时候跨表查询的性能反而比较差。

使用 ORM，有对象缓存，并且转化成 SQL 后都是基于主键查询。
不考虑对象缓存，数据库层面对简单的 SQL 也有缓存，而对复杂 SQL 却没有。
不要小看了缓存的力量！

拆分n+1条查询本质上是以增加n条SQL语句为代价，简化复杂SQL，换取数据库服务器磁盘IO的降低

[ORM对象缓存探讨](http://robbinfan.com/blog/3/orm-cache)

-------------
