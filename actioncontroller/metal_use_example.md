## Metal 使用举例

#### 原生的 Metal

在 Rails 里 metal 也属于 middleware，我们可以这么用：

```ruby
# config/routes.rb
get 'hello' => 'hello#index'
# ...
```

```ruby
# app/controllers/hello_controller.rb
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

然后浏览器访问：http://localhost:3000/hello 可以获取刚才的内容。

日志会由原来的：

```
Started GET "/hello" for 127.0.0.1 at 2014-04-27 09:04:49 +0800
Processing by HelloController#index as HTML
Completed 200 OK in 1ms (ActiveRecord: 0.0ms)
```

变为：

```
Started GET "/hello" for 127.0.0.1 at 2014-04-27 08:57:07 +0800
```

#### 加入 Rendering 模块

默认 ActionController::Metal 是没有提供渲染视图、模板和其它需要明确调用到 response_body=, content_type=, status= 等方法。如果你需要这些，可以引入它们：

```ruby
class HelloController < ActionController::Metal
  include AbstractController::Rendering
  include ActionView::Layouts

  append_view_path "#{Rails.root}/app/views"

  def index
    render "hello/index"
  end
end
```

#### 加入 Redirection 模块

想使用重定向相关代码，你也需要引入它们：

```ruby
class HelloController < ActionController::Metal
  include ActionController::Redirecting
  include Rails.application.routes.url_helpers

  def index
    redirect_to root_url
  end
end
```

#### 加入其它模块

参考 ActionController::Base 引入其它模块，以达到目的。

```ruby
class ApiController < ActionController::Metal # 使用 Metal, 而不是 Base
  include ActionController::Helpers
  include ActionController::Redirecting
  include ActionController::Rendering
  include ActionController::Renderers::All
  include ActionController::ConditionalGet

  # 需要响应 .json .xml 等不同格式的话，使用它。
  include ActionController::MimeResponds
  include ActionController::RequestForgeryProtection
  # 如果你需要 SSL
  include ActionController::ForceSSL
  include AbstractController::Callbacks
  # 想要知道 Controller 处理过程中，性能这方面的数据。
  include ActionController::Instrumentation
  # 需要转换 params 类型的话，可以使用它。
  include ActionController::ParamsWrapper
  include Devise::Controllers::Helpers

  # 在项目里使用路由相关的 helper 方法。
  include Rails.application.routes.url_helpers

  # 视图文件放在哪？
  append_view_path "#{Rails.root}/app/views"

  # 需要转换 params 类型的话，可以使用它。
  # { "person": { "name": "Kelby", "email": "leekelby@gmail.com" }}
  wrap_parameters format: [:json]

  # 根据客户端决定是否需要。(另，如果客户端不是浏览器的话，它会自动跳过)
  protect_from_forgery
end
```

上述代码，仅供参考。关于各模块及其作用，详情可以参考对应章节。

参考

[Developing api with rails metal](http://www.slideshare.net/artellectual/developing-api-with-rails-metal)
