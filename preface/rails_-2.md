# Rails - 其它


比较有趣好玩，但现在还没想好如何放到文中。暂时记录在此。

你可以比较独立的使用 Rails 的各个模块。

直接使用 Rack

```ruby
# config.ru
require 'rubygems'
require 'bundler/setup’ # Gemfile only lists 'rack'

run Proc.new {|env|
 if env["PATH_INFO"] == "/"
   [200, {"Content-Type" => "text/html"}, ["<h1>Front Page</h1>"]]
 else
   [404, {"Content-Type" => "text/html"}, ["<h1>Not Found</h1>"]]
 end
}
```

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

[A Rails App in a Single File ](http://rofish.net/rails_single_file.pdf)

https://gist.github.com/ROFISH/11273048
