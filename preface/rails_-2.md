## 一步步分析从请求到响应涉及到 Rails 的哪些模块

本文对 Rails 的使用几乎没有帮助，但有利于剖析 Rails 源码，这符合本书的写作目标，故成文。

你可以比较独立的使用 Rails 的各个模块。

### 纯 Rack 实现

```ruby
# config.ru
require 'bundler/setup'

run Proc.new {|env|
 if env["PATH_INFO"] == "/"
   [200, {"Content-Type" => "text/html"}, ["<h1>Hello World</h1>"]]
 else
   [404, {"Content-Type" => "text/html"}, ["<h1>Not Found</h1>"]]
 end
}
```

可通过 `rackup config.ru` 运行以上代码，默认在 http://localhost:9292/ 可以查看运行结果。

### 引入 ActionDispatch & 纯手动实现 Controller#actions

```ruby
# config.ru
require 'bundler/setup'
require 'action_dispatch'

routes = ActionDispatch::Routing::RouteSet.new
routes.draw do
  get '/' => 'mainpage#index'
  # 和以下写法效果一样，但'这里'要把它定义在 MainpageController 后面
  # get '/' => MainpageController.action("index")
  
  get '/page/:id' => 'mainpage#show'
end

class MainpageController
  def self.action(method)
    controller = self.new
    controller.method(method.to_sym)
  end

  def index(env)
    [200, {"Content-Type" => "text/html"}, ["<h1>Front Page</h1>"]]
  end

  def show(env)
    [200, {"Content-Type" => "text/html"},
     ["<pre> #{env['action_dispatch.request.path_parameters'][:id]} #</pre>"]]
  end
end

run routes
```

### 引入 'action_controller'，使用 Metal

```ruby
# config.ru
require 'bundler/setup'
require 'action_dispatch'
require 'action_controller'

routes = ActionDispatch::Routing::RouteSet.new
routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end

class MainpageController < ActionController::Metal
  def index
    self.response_body = "<h1>Front Page</h1>"
  end
  
  def show
    self.status = 404
    self.response_body =
      "<pre>#{env['action_dispatch.request.path_parameters'] [:id]}</pre>"
  end
end

run routes
```

### 项目使用其它 middleware, Controller 包含其它模块 & Controller 里纯手工打造 View 渲染相关代码

```ruby
# config.ru
require 'bundler/setup'
require 'action_dispatch'
require 'action_controller'

routes = ActionDispatch::Routing::RouteSet.new
routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end

class MainpageController < ActionController::Metal
  include AbstractController::Rendering
  include ActionController::Rendering
  include ActionController::ImplicitRender

  def index
    # self.response_body = "<h1>Front Page</h1>"
    @local_var = 12345
  end

  def show
    self.status = 404
    self.response_body =
      "<pre>#{env['action_dispatch.request.path_parameters'] [:id]}</pre>"
  end

  def render_to_body(*args)
    template = ERB.new File.read("#{params[:action]}.html.erb")
    template.result(binding)
  end
end

run routes
```

```ruby
# index.html.erb

Number is: <%= @local_var %>
```

### 引进 'action_view'

```ruby
# config.ru
require 'bundler/setup'
require 'action_dispatch'
require 'action_controller'
require 'action_view'

routes = ActionDispatch::Routing::RouteSet.new
routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end

class MainpageController < ActionController::Metal
  include AbstractController::Rendering
  include ActionView::Rendering
  include ActionController::Rendering
  include ActionController::ImplicitRender

  prepend_view_path('app/views')

  def index
    @local_var = 12345
  end

  def show
  end
end

use ActionDispatch::DebugExceptions
run routes
```

```ruby
# app/views/mainpage/index.html.erb

Number is: <%= @local_var %>
```

```ruby
# app/views/mainpage/show.html.erb

Content is: <pre><%= env['action_dispatch.request.path_parameters'][:id] %></pre>
```

### 直接使用 ActionController::Base

```ruby
# config.ru
# require 'bundler/setup'
# require 'action_dispatch'
# require 'action_view'
require 'action_controller'

routes = ActionDispatch::Routing::RouteSet.new
routes.draw do
  get '/' => 'mainpage#index'
  get '/page/:id' => 'mainpage#show'
end

class MainpageController < ActionController::Base
  prepend_view_path('app/views/')

  def index
    @local_var = 12345
  end
  
  def show
  end
end

use ActionDispatch::DebugExceptions
run routes
```

对应以上代码，我们需要创建视图文件。

```ruby
# app/views/mainpage/index.html.erb
```

和

```ruby
# app/views/mainpage/show.html.erb
```

### 汇总以上进化过程

`run` 由应用服务器提供，运行一个 Rack application.

1. 纯 Rack 实现
2. 引入 'action_dispatch', 使用 routes
3. 纯手动实现 Controller#actions
4. 引入 'action_controller'，使用 Metal
5. 项目使用其它 middleware, Controller 包含其它模块
6. Controller 里纯手工打造 View 渲染相关代码
7. 引进 'action_view'
8. 直接使用 ActionController::Base

9. 没有使用 ActionMailer，ActiveJob, ActiveModel, ActiveRecord
10. 使用但没感受到 AbstractController，ActiveSupport, Railties

参考

[A Rails App in a Single File ](http://rofish.net/rails_single_file.pdf)
