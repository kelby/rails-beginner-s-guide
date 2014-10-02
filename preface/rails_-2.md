# Rails - 其它

用来理解原理，很好，但不实用。

比较有趣好玩，但现在还没想好如何放到文中。暂时记录在此。

你可以比较独立的使用 Rails 的各个模块。

直接使用 Rack

```ruby
# config.ru
require 'bundler/setup' # Gemfile only lists 'rack'

run Proc.new {|env|
 if env["PATH_INFO"] == "/"
   [200, {"Content-Type" => "text/html"}, ["<h1>Front Page</h1>"]]
 else
   [404, {"Content-Type" => "text/html"}, ["<h1>Not Found</h1>"]]
 end
}
```

可通过 `rackup config.ru` 运行以上代码，默认在 http://localhost:9292/ 可以查看运行结果。

如果你需要 Route, Controller, View，还有其它：

```ruby
# config.ru
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new

routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end
```

```ruby
# mainpage_controller.rb
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

参考

[A Rails App in a Single File ](http://rofish.net/rails_single_file.pdf)

https://gist.github.com/ROFISH/11273048

## 解读以上进化过程

`run` 由应用服务器提供，运行一个 Rack application.

1. 纯 Rack 实现
2. 引入 'action_dispatch', 使用 routes
3. 纯手动实现 Controller#actions
4. 引入 'action_controller'，使用 Metal
5. 项目使用其它 middleware, Controller 包含其它模块
6. Controller 里纯手工打造 View 渲染相关代码
7. 引进 'action_view'
 

8. 和 Rails 对比，没有使用 ActionMailer，ActiveJob, ActiveModel, ActiveRecord
9. 和 Rails 对比，使用但没感受到 AbstractController，ActiveSupport, Railties
