## 一步步分析从请求到响应涉及到 Rails 的哪些模块

通过本章节，你可以比较独立的使用 Rails 的各个模块，有利于理解 Rails 源码各个模块的主要工作。

#### 纯 Rack 实现

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

#### 引入 Action Dispatch & 纯手动实现 Controller#actions

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

#### 引入 Action Controller，使用 Metal

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

**注意：**上面已经使用到了 Action Dispatch 里的"各个 Middleware 组件"，但并没有使用到 Action Controller 里的"各个 Metal 增强组件"。

#### 引用 Metal 增强模块 & Controller 里纯手工打造 View 渲染相关代码

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

#### 引进 Action View

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

到此，Routing、Controller、View 3者已经到位。Model 我们可以直接调用，不影响这里的整个请求、响应处理过程。

#### 直接使用 ActionController::Base

前面提到过 ActionController::Metal 相当于一个加强版本的 Rack，功能非常有限，实际开发中建议使用继承于它的 ActionController::Base.

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

#### 其它

上述代码：
<br>
没有使用 Action Mailer，Active Job, Active Model, Active Record 等；
<br>
使用但没感受到 Abstract Controller，Active Support, Railties 等；
<br>
没有使用 Engine 等。

上面代码里的 `run` 方法由应用服务器提供，用于运行一个 Rack application.

参考

[A Rails App in a Single File ](http://rofish.net/rails_single_file.pdf)
